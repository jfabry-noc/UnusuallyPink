+++
title = "Updating PowerShellGet"
date = "2020-10-11"
author = "John Fabry"
authorTwitter = "UnusuallyPink"
showFullContent = false
keywords = ["nuget", "powershell", "powershellget", "windows"]
tags = ["nuget", "powershell", "powershellget", "windows"]
+++

It's not too often these days that I find myself needing to update the underpinnings of PowerShell. The majority of the PowerShell work I do now is based on [PowerShell Core](https://github.com/PowerShell/PowerShell), the current version of which is 7.0.3, and frequently just comes with newer versions of the supporting modules. PowerShell Core began with PowerShell 6 and is created with .NET Core, which is Microsoft's open source and cross-platform flavor of .NET. PowerShell version 5 and before, known as Windows PowerShell, is the original, Windows-specific variant of PowerShell. Microsoft doesn't really do any new development work on Windows PowerShell, instead opting to work on PowerShell Core and slowly make the full set of functionality available on all platforms.

This is awesome, but some systems very specifically target Windows PowerShell. This can easily happen since the interpreter even has a different name; Windows PowerShell calls `powershell.exe` while PowerShell Core calls `pwsh.exe` in an effort to allow the two versions to co-exist on the same Windows host. As a result, systems which proxy PowerShell commands or scripts on your behalf down to a target machine that have not been updated to expect PowerShell Core will generally target Windows PowerShell instead. This was the situation I found myself in last week.

I was attempting to load a script that I had written into a monitoring platform which will then send my script down to any number of "collector" machines in order to for it to execute and do the actual data aggregation. In this case, my script failed because it was calling the `MSAL.PS` module. [MSAL](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet) is the Microsoft Authentication Library, and as the name indicates it facilitates authentication to Azure AD. It replaces the older Azure AD Authentication Library (ADAL), and is honestly _much_ nicer to use. The module needs to be installed first, though, and while I had previously installed it on the target system under PowerShell Core, Windows PowerShell is a completely separate entity with a separate space for modules. I remoted to the system and ran the following to handle the installation from an administrative Windows PowerShell session:

`Install-Module -Name MSAL.PS`

Instead of joy, I got the following error message:

> WARNING: The specified module 'MSAL.PS' with PowerShellGetFormatVersion '2.0' is not supported by the current version of PowerShellGet. Get the latest version of the PowerShellGet module to install this module, 'MSAL.PS'.

Ick... some things were a bit old in the Windows PowerShell installation. This was one of the rare instances where the error message didn't tell me _exactly_ how to fix the issue, though, so I did a few searches on this exact error. The trick is that updating `PowerShellGet` involves not one but two steps.

While `PowerShellGet` is a module specific for discovering and installing PowerShell packages from the [PowerShell Gallery](https://www.powershellgallery.com/), it leverages Microsoft's much more generic [NuGet package manager](https://www.nuget.org/). To get the latest version of PowerShellGet, I [first had to make sure I was using the latest version of NuGet](https://docs.microsoft.com/en-us/powershell/scripting/gallery/installing-psget?view=powershell-7) by running:

`Install-PackageProvider Nuget -Force`

Once that completed, _then_ I was able to successfully update PowerShellGet via:

`Install-Module -Name PowerShellGet -Force`

Once the update completes, the current PowerShell session will still be running the old version. I just closed PowerShell, re-launched a new administrator instance, and then successfully installed the module via the same cmdlet from earlier:

`Install-Module -Name MSAL.PS`
