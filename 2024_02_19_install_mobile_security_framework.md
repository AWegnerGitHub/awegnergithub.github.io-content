Title: Installing the Mobile Security Framework (MobSF) on Windows using Docker
Date: 2024-02-19 15:00
Tags: technical
Category: Technical Solutions
Slug: install-mobsf-on-windows-docker
Summary: A short article on installing the Mobile Security Framework on Windows utilizing Docker and then using it to quickly analyze an APK.
Status: published

[TOC]

## Introduction

I recently needed to poke around an APK. I'd done this sort of thing before, but it's been years so the process of setting up an Android environment was unfamilar. Instead of spending a day reimmersing myself for what was (hopefully) going to be a 10-15 minute thing, I turned to another tool: [Mobile Security Framework][1] (also known as MobSF)

## Setting up Docker

There are countless blog posts, articles, and YouTube videos on how to set up the perfect docker environment. This was a quick task. I didn't need "perfect", I needed good enough. At the time, I was on a Windows 11 machine, so that's what I had to work with. The goal was to [get Docker][2] operational as quickly as possible.

Why? MobSF has a quick setup with two lines of Docker code. 

Using the [Install Docker for Windows][3] documentation, I downloaded the installable and started the process. The installation took a couple minutes on the device I was on and required me to log out and back in. Total install time was less than 5 minutes.


## Installing MobSF

Once logged back in, I fired up the command line. The [MobSF README][1] provides two lines to execute on the command line to pull the latest image. I didn't go through the process of setting up a dymanic analyzer. I didn't think I'd need it (and as luck would have it in this case, I was correct).

Run the two commands:

```
docker pull opensecurity/mobile-security-framework-mobsf:latest
docker run -it --rm -p 8000:8000 opensecurity/mobile-security-framework-mobsf:latest
```

This will pull the latest version of MobSF and then run it on port 8000 on the local machine. Once it's running you can access it in your browser by going to `http://127.0.0.1:8000`

## Find your APK

Theorectically, you'll be running this against an APK you've built and developed. In that case, you'll have the APK handy and can load it into the UI. I, unfortunately, didn't have that luxuary. Fortunately, getting the APK is relatively simple.

1. Go to the [Google Playstore][4] and search for the application
2. Navigate into the details page of the application (you should see the option to install it)
3. Copy the URL to this page
4. Go to [APKCombo][5]
5. Paste the URL you copied in step 3 instal the appropriate text box and click "Generate Download Link"
6. After a few seconds, click the download icon next to the link that was generated

You now have the APK to use.

## Using MobSF

With the APK in hand, and MobSF running, navigate to `http://127.0.0.1:8000`. Upload the APK and wait a couple of minutes. If you are watching the logs, you'll see it performing multiple steps behind the scenes. Once it's done analyzing the APK, it'll drop you to the dashboard.

![Analyzing an APK][6]

![Analyzing an APK with logs][7]

The dashboard has a _lot_ of detail about the APK. Scroll through to see the permissions it's utilizing, certificates attached to it, potential security vulnerabilities, code analysis, detected URLs in the code, hard coded secrets, and strings. All of these would help a team build a better and more secure mobile application. 

## Conclusion

For my purpose, this 15 minute excersice not only answered the question I was seeking it also raised a few items that will need to be corrected before the next release. Introducing security practices with a tool like MobSF would be incredibly benefitial to a team tasked with building an Android application. Further, adding in the dynamic analysis capabilities can provide more details on areas to improve. 

While I didn't need those to answer my question, I'd enourage someone looking to introduce this to a team, to look at the [capabilities of MobSF][8]




 [1]: https://github.com/MobSF/Mobile-Security-Framework-MobSF
 [2]: https://docs.docker.com/get-docker/
 [3]: https://docs.docker.com/desktop/install/windows-install/
 [4]: https://play.google.com/store/apps
 [5]: https://apkcombo.com/downloader/
 [6]: {attach}images/mobsf/analyze.png
 [7]: {attach}images/mobsf/analyze-logs.png
 [8]: https://mobsf.github.io/docs/#/dynamic_analyzer