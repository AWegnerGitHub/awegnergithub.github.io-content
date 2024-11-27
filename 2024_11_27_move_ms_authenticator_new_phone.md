Title: Moving MS Authenticator to a new phone without a personal account
Date: 2024-11-27 12:15
Tags: technical
Category: Technical Solutions
Slug: ms-authenticator-without-personal-account
Summary: The Microsoft Authenticator application allows you to transfer tokens to a new device if you have a personal Microsoft account. How do you do it if you don't have a personal account? This is a quick walk through of what worked for me.

[TOC]

## Introduction

Work utilizes the Microsoft Authenticator app as their 2FA application of choice. It's not my favorite, but it does what it needs to do, and since I only use the application for work things, it's nice to keep work accounts out of personal accounts in my other 2FA application. Recently, I got a new phone, and needed to migrate everything over to that device. As an aside, Android continues to make this easier and easier. 

Standard advice on how to migrate your token to a new device is to enable cloud backup, sign in to the new device and import the backup. The problem is when you go to enable Cloud Backup from the settings, you are hit with this message:

> "You need a personal Microsoft account to use cloud backup"

Well, I don't have a personal Microsoft account. 

_Aside: I can't provide screenshots of the Android application for this post because the application prevents screenshots from being taken. I appreciate this feature as part of the application for security purposes, but it's annoying when trying to write about it. Oh well..._

## Alternative to Cloud Backup

I have set up MFA on my work account - obviously - and can access settings through the [Microsoft MFA setup page][mfa]. From here, you'll be on the Security Info page. It will show the current authenticator device being utilized. You'll need the old device during this process to authenticate one last time.

![Microsoft security information page showing my current MS Authenticator device][sec_info]

Click on "Add sign-in method" and then "Microsoft Authenticator"

![Options to pick from for adding a new signin method. For this, select Microsoft Authenticator][new_method]

On your phone, ensure you have the MS Authenticator application installed. Then follow the on screen prompts between your computer and your new phone. You'll select "Work or School" and then scan the QR code that is common with 2FA/MFA applications.

The last step in the process will be to send an authentication request to the new device. Do so, enter the verification code, and your new device will be added.

![Device is approved after entering your verification code][approved]

I recommend you wrap this up by removing your old device from the Security Info section now that the new device is approved. 

## Conclusion

The Microsoft authenticator application doesn't make moving to a new device as easy as some of its competitors. The requirement for a personal Microsoft account to go through the most commonly recommended way is also a non-starter if you don't have such an account already. Fortunately, the alternative posted above ended up working just fine. I assume I'll need a new phone again, and this should help at least me, remember that there is an easy enough way to migrate.


 [mfa]: https://aka.ms/mfasetup
 [sec_info]: {attach}images/ms_authenticator/security_info.png
 [new_method]: {attach}images/ms_authenticator/new_method.png
 [approved]: {attach}images/ms_authenticator/approved_device.png