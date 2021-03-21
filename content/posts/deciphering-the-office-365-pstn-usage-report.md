+++
title = "Deciphering the Office 365 PSTN Usage Report"
date = "2020-04-28"
author = "John Fabry"
authorTwitter = "UnusuallyPink"
showFullContent = false
keyword = ["audioconferencing", "dial-out-pool", "o365", "office-365", "pstn", "python", "scripting"]
tags = ["audioconferencing", "dial-out-pool", "o365", "office-365", "pstn", "python", "scripting"]
+++

## Background

Last week I received a bit of a mystifying alert from Office 365 letting me know that our pool of PSTN dial-out minutes for the month had been 80% consumed. This was mystifying due to the fact that as someone who has managed O365 for nearly a decade I didn't realize there _was_ a pool of minutes. PSTN (Public Switched Telephone Network) had to be related in some way or another to the capability my company has purchased from Office 365 to have a dial-in option added to our Microsoft Teams meetings. Any meeting we create has a dial-in number so that anyone who has a poor data connection can still dial a telephone and at least join the audio portion of the meeting.

While the main function of the Audioconferencing license is to provide that dial-**in** functionality, dial-**out** options also exist. For example, while in a Teams meeting if you realize you need someone else to participate you have the option to have Teams dial that person if you provide a phone number. Likewise, if you're struggling in a low-coverage area you can tap a button in the Teams mobile app to have the service call you.

Sure enough (say what you want about Microsoft, but their documentation for Office 365 tends to be extremely good), I found the [official confirmation](https://docs.microsoft.com/en-us/microsoftteams/audio-conferencing-subscription-dial-out). It just seems like a bit of a forgotten caveat considering you have to go to the legacy Skype portal to see it; the numbers are completely absent from the Teams portal:

> You can monitor the usage against your dial-out minute pool in the "legacy" Skype for Business Admin Center. In the Microsoft Teams Admin Center, navigate to Legacy portal > Reports > PSTN Minute Pools. The Zone A dial-out minute pool will be labeled in the report as "Outbound Calls to Zone A Countries."

## Digging In

When I checked the total number of minutes we had in our pool, I did some quick math to confirm that we were allotted 60 dial-out minutes per Audioconferencing license that was **assigned**; purchased licenses that were not yet assigned to a user didn't factor into the equation.

I was still a bit stumped as to _who_ would be eating through our minutes, though, considering I figured dial-out to be a pretty niche feature. Luckily, the reporting section of the legacy Skype portal gives a report of how the minutes are used. There's even an option to export it for a particular time range. I exported the data on the month-to-date in my tenant, which saved a .csv file. Opening it up showed me that there was still some work to do. The report listed dates for each call, the UPN (userPrincipalName) of the user, a source number, a destination number, and the duration of the call... in seconds. It also included both the dial-out information I wanted **and** the dial-in information that I don't care about since that is unlimited. Ick.

I first fired up my text editor and put together a Python script. It weeded out any of the dial-in entries. For all of the dial-out entries, it added the UPN of the user to a list of dictionaries with a count of the seconds for their call. For each entry in the report, it was checked against my list of dictionaries, one for each user. If the user already had a dictionary in the list, the duration of that call was added to the user's existing total. If the user wasn't in the list yet, the script would append a new dictionary to the list for that user with the duration of the current call serving as their starting point. So the key for each dictionary was the UPN and the value was the number of seconds. Each dictionary in the list only needed one key-value pair.

I'm not sharing this initial version of the script because while the code worked perfectly, the logic was flawed. The report actually showed that [Craft Brew Geek](https://instagram.com/craftbrewgeek) was using _significantly_ more minutes than anyone else. While I was aware of the fact that he was frequently using his phone for Teams meetings while everyone is under quarantine due to his fixed-line provider having issues, I thought he was doing that via LTE rather than PSTN. He described his method of joining meetings to me, and he was literally tapping the "Join" button from the Teams app on his phone. To verify this was using LTE data, I even noted the LTE data consumed by the Teams app on my phone for the month, did a 10 minute Teams audio-only call with Brandi by joining the same way Craft Brew Geek was, and then checking the data usage again. Sure enough, it went up by 10 MB; this process wasn't touching PSTN at all. I made doubly sure by assigning a policy to just my O365 account in Teams to block dial-out. I verified that tapping the "Join" button on my phone still worked fine while opting for the explicit "Dial me" option ended in an error.

The report also struck me as odd because it only listed 2 days for Craft Brew Geek, despite the fact that I know he's been joining Teams calls the same way for 2 months now while we all work from home. I went back to the data this time paying attention to the source and destination numbers listed in the report. I noticed for all of Craft Brew Geek's entries, the number listed as the source number was the number from Microsoft that we selected as the "default" number for our tenant due to it having the closest proximity to the majority of our employees. The destination number was the same every time as well, but it _wasn't_ Craft Brew Geek's number. It was the number for a different employee in our company.

This is when I finally realized that the UPN listed in the report is **not** necessarily the UPN of the user who consumed the minutes; that wouldn't even necessarily make sense considering you could have a meeting dial-out to an external participant (e.g. if you were using Teams to conduct an interview) or you may not have any telephone information stored for your users in Office 365. Instead, the UPN listed in the report is who **scheduled** the meeting.

Realizing that my logic was flawed due to the ambiguity of the report, I modified my script and replaced the UPN as the key in each dictionary with the destination telephone number. What I ended up with is [this Python script](https://gist.github.com/JFFail/a2da00a7c379d2805825a27a07bc89b4). It's quick and dirty since I wasn't trying anything fancy; I just wanted a quick, sorted listing of our dial-out usage.

```python3
import csv

usage\_tracker \= {}

report\_file \= "./pstn\_report.csv"
with open(report\_file, "r", newline\="") as csvfile:
    reader \= csv.DictReader(csvfile)

    for row in reader:
        if row\["Call Type"\] \== "conf\_out":
            seconds \= int(row\["Duration Seconds"\])
            if row\["Destination Number"\] in usage\_tracker.keys():
                usage\_tracker\[row\["Destination Number"\]\] += seconds
            else:
                usage\_tracker\[row\["Destination Number"\]\] \= seconds

usage\_tuple\_list \= sorted(usage\_tracker.items(), key\=lambda x: x\[1\], reverse\=True)

total\_minutes \= 0
for element in usage\_tuple\_list:
    current\_minutes \= round(element\[1\] / 60)
    total\_minutes += current\_minutes
    print(element\[0\] + ": " + str(current\_minutes))

print("============================")
print(total\_minutes)
```

At this point, all I needed to do was figure out who the telephone numbers my script spit out belonged to. Our company maintains a listing of numbers for each employee, so it was simple for me to search it for each entry. This showed us that one employee who was using dial-out heavily for himself as a matter of convenience during the quarantine was using basically all of our minutes.

## Aftermath

The main takeaways from this incident are:

1. Be mindful of the PSTN dial-out pool in Office 365 if you're doing audioconferencing. I hadn't thought of it because the O365 instance I managed previously was absolutely massive; this means we had significantly more minutes in the pool for a feature most people never use since the size of the pool is based on license assignment. Keep tabs on it in a smaller organization, especially when everyone is suddenly working remotely.
2. The report from the legacy Skype admin center is pretty confusing, and it's easy to mistake who is consuming dial-out minutes. Keep tabs on the destination number(s) in the report to find out who is actually doing it.

That's it! Stay pink (from a socially acceptable distance, of course.)
