+++
title = "PowerShell: Export Non-Standard and Multi-Value AD Attributes to CSV"
date = "2019-06-01"
author = "John Fabry"
authorTwitter = "UnusuallyPink"
showFullContent = false
keywords = ["active-directory", "ad", "code", "powershell", "scripting"]
tags = ["active-directory", "ad", "code", "powershell", "scripting"]
+++

I’ve recently been spending a lot of my time at work doing PowerShell scripting for a project that’s currently underway. While working on a bit of code late last week I ran into two interesting problems that I had yet to really stumble across despite doing heavy PowerShell scripting against Active Directory for nearly a decade now.

## Exporting Non-standard Attributes To CSV

My first issue was that I needed to export some non-standard attributes from contact objects in AD to a .csv file. When I say “non-standard” I don’t mean that we’ve done a custom schema extension or anything like that. Instead, I simply mean that the AD PowerShell module only wants to include certain attributes when exporting AD objects to a .csv file. For example, if I run **Get-ADObject** **\-Properties \*** against a particular contact object and simply let the output dump to my shell, I see all of the attributes I expect including stuff like **givenName** and **sn**. However, if I run the exact same cmdlet but pipe the result to **Export-Csv** rather than sending it to the shell I will not have all the same attributes; ones like **givenName** and **sn** will now be missing. That’s a problem.

## Storing Mutli-Value Attributes In a CSV

The other problem was that one of the attributes I needed is **proxyAddresses**. This is a multi-value attribute as contacts could very likely have multiple proxy addresses associated with a particular mailbox. They may have an alias, x500 addresses, etc. If you use PowerShell to pull this information from AD and dump it to the screen you’ll see the expected proxy address information. If you call **GetType()** on the **proxyAddresses** attribute you’ll see it listed as an array. That’s why if you export a contact to a .csv file and include **proxyAddresses**, the value for each contact in that particular column will simply read:

> Microsoft.ActiveDirectory.Management.ADPropertyValueCollection

PowerShell isn’t sure what to do with a multi-value attribute as far as the .csv file goes so it stores the data type. This is also a problem!

## The Solution

My solution to this was the following. Note that it’ll be easier to view [the Gist for it](https://gist.github.com/JFFail/6dd9acb0020845a905eeda8fbc8de1a7).

```
# Get the objects with the required attributes and storing proxyAddresses properly in the .csv file.
Get-ADObject -Filter { objectClass -eq "contact" } -Server myserver.mydomain.net -SearchBase "OU=Contacts,DC=mydomain,DC=net" -SearchScope OneLevel -Properties name,givenName,sn,displayName,telephoneNumber,proxyAddresses,targetAddress,mail,mailNickname,company,department,l,physicalDeliveryOfficeName,postalCode,st,streetAddress,title | Select-Object name,givenName,sn,displayName,telephoneNumber,targetAddress,mail,mailNickname,company,department,l,physicalDeliveryOfficeName,postalCode,st,streetAddress,title,@{name="proxyAddresses"; expression={$_.proxyAddresses -join ";"}} | Export-Csv -Path .\sample.csv -Encoding ASCII -Append -NoClobber -NoTypeInformation

# Shows how to then use the proxyAddresses attribute in the future.
$allContacts = Import-Csv -Path .\sample.csv
foreach($singleContact in $allContacts) {
    $proxyAddresses = $singleContact.proxyAddresses.Split(";")
}
```

The first thing I needed was to get the properties I actually needed as opposed to just the ones PowerShell felt like including due to the object type. The solution was to:

1. Specify all of the attributes I wanted in **-Properties** to ensure they would be available.
    
2. Pipe the AD object to **Select-Object** and then re-specify each of the properties I wanted.
    

This works because PowerShell is selecting the attributes to include in the .csv file based on the type of object. **Select-Object** takes whatever comes through the pipeline, though, and makes a brand new, generic object from it. That object will include any properties from that original object that you specify, hence why I tell it the exact listing of properties that I need again.

This leads to the solution to the second problem which is what to do for **proxyAddresses**. PowerShell allows for the use of an expression to parse together a new property. In this instance the expression is to take the existing **proxyAddresses** array and **-join** it into a string. Each of the items in the array are separated by a semicolon. If a contact has the following proxy addresses, for example:

SMTP:pretty@unusually.pink
smtp:awesome@unusually.pink

Then the property in the .csv file will be stored as:

```
SMTP:pretty@unusually.pink;smtp:awesome@unusually.pink
```

You could pick any character for the delimiter that you want as long as it isn’t used in other data that you’re storing.

In this instance my goal was to take the exported data, re-import it, and then create contacts in a different AD forest. When creating a new object AD is expecting an array for the **proxyAddresses** attribute. A string with a random delimiter won’t do it. That’s where the second part of the example comes into play. If I’m looping through all of the data stored in the .csv file I can recreate the array of proxy addresses by splitting the string on my delimiter (in this case a semicolon) and assigning the result to a variable. That variable will be an array which can then be used to popular **proxyAddresses** when running **New-ADObject**
