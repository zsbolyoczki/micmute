# How to mute/unmute the microphone on Windows 10 with a hotkey

## Tools needed

### Autohotkey

- Download and install from https://www.autohotkey.com
- Create the hotkey script: ``%APPDATA%\micmute.ahk``
```
^F3::
Run, powershell -Command %APPDATA%\micmute.ps1, , Hide
#NoTrayIcon
```


### AudioDeviceCmdlets

- Download the DLL from https://github.com/frgnca/AudioDeviceCmdlets
- open Powershell ISE from Start menu as Administrator
- Run this script
```
New-Item "$($profile | split-path)\Modules\AudioDeviceCmdlets" -Type directory -Force
Copy-Item "C:\blah\blah\Downloads\AudioDeviceCmdlets.dll" "$($profile | split-path)\Modules\AudioDeviceCmdlets\AudioDeviceCmdlets.dll"
```

### Mute script

- Create the script that mutes/unmutes the mic (%APPDATA%\micmute.ps1)
```
Set-Location "$($profile | Split-Path)\Modules\AudioDeviceCmdlets"
Get-ChildItem | Unblock-File
Import-Module AudioDeviceCmdlets

$ismuted = Get-AudioDevice -RecordingMute

if ($ismuted) {
  Set-AudioDevice -RecordingMuteToggle
	Set-ItemProperty -Path HKCU:\SOFTWARE\Microsoft\Windows\CurrentVersion\Themes\Personalize -Name AppsUseLightTheme -Value 1
}
else {
  Set-AudioDevice -RecordingMuteToggle
	Set-ItemProperty -Path HKCU:\SOFTWARE\Microsoft\Windows\CurrentVersion\Themes\Personalize -Name AppsUseLightTheme -Value 0	
}
```

## Using the script

- Double click on ``%APPDATA%\micmute.ahk`` after logged in to Windows to start Autohotkey
- Press ``CTRL - F3`` to mute/unmute the microphone
- The script will switch Windows to **"Dark"** theme when the microphone is **muted** and switches back to **"Light"** mode when it's **unmuted** to have visual feedback

