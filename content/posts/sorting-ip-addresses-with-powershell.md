+++
title = "Sorting IP Addresses With PowerShell"
date = "2019-06-15"
author = "John Fabry"
authorTwitter = "UnusuallyPink"
showFullContent = false
keywords = ["ip-address", "powershell", "scripting", "sorting"]
tags = ["ip-address", "powershell", "scripting", "sorting"]
+++

I was recently attempting to sort a list of IP addresses that I had in a text file. PowerShell is awesome at sorting, so I figured I would use it given that I had nearly 2000 of them. I initially just tried to pipe the contents of the file to the **Sort-Object** cmdlet:

`Get-Content -Path .\ipListFixed.txt | Sort-Object`

The results were, suffice to say, lackluster. I ended up with something like this:

```
10.1.69.1
10.1.69.12
10.1.69.148
10.1.69.149
10.1.69.17
```

Gross, right? Obviously 10.1.69.17 shouldn’t be after 10.1.69.148 or .149. The issue is that the whole octet isn’t being considered. It’s being sorted like they’re just strings rather than IP addresses; 7 is bigger than 4 so they’re sorted accordingly. PowerShell isn’t really comparing the numbers of 17 to 148, for example, to realize that 17 is actually smaller.

It makes sense; PowerShell can’t assume that the value is an IP address so it’s treating the value like a string instead. This means that the key is to _tell_ PowerShell it’s an IP address so that PowerShell can adjust the way it does sorting. This is possible by casting the values as the [.NET Version class](https://docs.microsoft.com/en-us/dotnet/api/system.version?view=netcore-2.2). Since I happened to be pulling the IPs from a file, I did the casting within the **\-Property** parameter that I added to the **Sort-Object** cmdlet:

`Get-Content -Path .\ipListFixed.txt | Sort-Object -Property { [System.Version]$_ }`

This yields _significantly_ better results:

```
10.1.69.1
10.1.69.2
10.1.69.12
10.1.69.17
10.1.69.29
```

Stay pink!
