# 7. Adding Users with PowerShell Script

## Project Overview

This guide demonstrates how to automate user account creation in Active Directory using PowerShell scripting. Instead of manually adding users one by one through the Active Directory GUI, this project shows how to create multiple user accounts simultaneously using a simple text file containing names and a PowerShell script that processes this data. 

This automation technique is especially useful for system administrators who need to provision accounts for new employees, test environments, or classroom setups quickly and efficiently.

## Prerequisites

- Create a Notepad/Text file with your list of names, ensuring they are all in the same format
- No middle initials
- Save that file to a folder on your desktop

## Running PowerShell as Administrator

1. Click Start then Windows PowerShell
2. Right click Windows PowerShell ISE
3. Select More > Run as administrator > Yes

<img src=https://ik.imagekit.io/typeai/tr:w-1200,c-at_max/img_UulbPi6ezOoxaDCVQw.png width=400px>

1. Select the 'Open' Folder and double click on the saved script

<img src=https://ik.imagekit.io/typeai/tr:w-1200,c-at_max/img_nweo1bkE4H2hxWfpMT.png width=400px>

## Script Breakdown

<img src=https://ik.imagekit.io/typeai/tr:w-1200,c-at_max/img_iiOR34bC6pPqXJd2XN.png width=1000px>

## Example: Adding The Office US Characters

Here is the script and the text file structure:

### PowerShell Script:

```powershell
# ----- Edit these Variables for your own Use Case ----- #
$PASSWORD_FOR_USERS = "Pass1234"
$USER_FIRST_LAST_LIST = Get-Content .\TheOffice.txt
# ------------------------------------------------------ #

$password = ConvertTo-SecureString $PASSWORD_FOR_USERS -AsPlainText -Force

New-ADOrganizationalUnit -Name _THEOFFICE -ProtectedFromAccidentalDeletion $false

foreach ($n in $USER_FIRST_LAST_LIST) {
    $first = $n.Split(" ")[0].ToLower()
    $last = $n.Split(" ")[1].ToLower()
    $username = "$($first.Substring(0,1))$($last)".ToLower()
&nbsp;
    Write-Host "Creating user: $($username)" -BackgroundColor Black -ForegroundColor Cyan
&nbsp;
    New-AdUser -AccountPassword $password `
               -GivenName $first `
               -Surname $last `
               -DisplayName $username `
               -Name $username `
               -EmployeeID $username `
               -PasswordNeverExpires $true `
               -Path "ou=_THEOFFICE,$(([ADSI]"").distinguishedName)" `
               -Enabled $true
}
```

### Text File Format:

**TheOffice.txt**: [Click here to access names](https://github.com/paigeholl/BulkAdd/blob/main/TheOffice.txt)

> **NOTE**: Be mindful of Lines 7 and 22 if you want to create a new Organizational Unit with a different name.

## Enabling Script Execution

The script won't work right away - you need to enable the execution of scripts first:

1. For the lab environment, enter:
2. ```powershell
   Set-ExecutionPolicy Unrestricted
   ```
3. When prompted if you are sure, click "Yes to All"

<img src=https://ik.imagekit.io/typeai/tr:w-1200,c-at_max/img_6hGKnVzOOxcvJPT8kO.png width=400px>

## Running the Script

1. Navigate to the directory where the text file is located with the 'cd' command:

<img src=https://ik.imagekit.io/typeai/tr:w-1200,c-at_max/img_UfpUpMvaAfn2qxGTzg.png width=400px>

1. Click the Play icon to start the script:

<img src=https://ik.imagekit.io/typeai/tr:w-1200,c-at_max/img_NK2ayiMTyOyG2gcQ9E.png width=400px>

## Results

### Active Directory Before Running the Script:

<img src=https://ik.imagekit.io/typeai/tr:w-1200,c-at_max/img_BfG0Dha1FOBldJMbo3.png width=400px>

### Active Directory After Running the Script:

![](https://ik.imagekit.io/typeai/tr:w-1200,c-at_max/img_OCwaG2LzB3uOvOKumI.png){ width=400px }

> **REMINDER**: Be mindful of Lines 7 and 22 if you want to create your own new Organizational Unit.
