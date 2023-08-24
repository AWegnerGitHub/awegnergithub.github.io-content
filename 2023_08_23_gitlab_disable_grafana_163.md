Title: How to disable Grafana in Gitlab 16.3 Omnibus
Date: 2023-08-23 22:15
Tags: technical
Category: Technical Solutions
Slug: disable-grafana-in-gitlab-16
Summary: GitLab 16.3 deprecated and disabled the bundled Grafana, but didn't provide complete instructions for how to disable it. Fortunately, it's easy to do. I've documented the few steps needed.
Status: published

[TOC]

## Introduction

I continue to love my [self hosted instance of GitLab][gitlab]. I look forward to the 22nd of every month to pull down the 
newest dot update and enjoy the new features. As with any piece of software, things get deprecated and eventually disabled over
time. 

In 16.3 - released in August 2023 - the version of [Grafana][grafana] bundled with the self hosted version of GitLab reached the 
end of it's depreciation window and was disabled. GitLab added a check to prevent an update if it was still enabled. 

## Symptom

When performing an `apt` update, GitLab 16.3 will error out if Grafana is still enabled on your instance. The error looks like this:


    Preparing to unpack .../gitlab-ee_16.3.0-ee.0_amd64.deb ...
    * grafana[enable] has been deprecated since 16.0 and was removed in 16.3. The bundled Grafana is deprecated and no longer available. We recommond following
    https://docs.gitlab.com/ee/administration/monitoring/performance/grafana_configuration.html#deprecation.
    Deprecations found. Please correct them and try again.

## The Problem

The [documentation linked][1] provides four steps to follow

> To switch away from bundled Grafana to a newer version of Grafana from Grafana Labs:
>
>  1. Set up a version of Grafana from Grafana Labs.
>  2. Export the existing dashboards from bundled Grafana.
>  3. Import the existing dashboards in the new Grafana instance.
>  4. [Configure GitLab][2] to use the new Grafana instance.

I've removed the links in steps 2 and 3, because they really don't matter for this write up. Step 1 does not have a link, so it's left as 
and excercise to the user to install Grafana in their environment. Step 4, links to another article that provides a few more steps to follow.


>  1. On the left sidebar, expand the top-most chevron.
>  2. Select Admin Area.
>  3. On the left sidebar, select Settings > Metrics and profiling and expand Metrics - Grafana.
>  4. Select the Add a link to Grafana checkbox.
>  5. Configure the Grafana URL. Enter the full URL of the Grafana instance.
>  6. Select Save changes.

And that's it. That's the set of instructions provided by GitLab. Follow those, rerun your `apt` update and you'll run into the same symptom.

## Solution

The missing step is hinted at in the error message itself: `grafana[enable] has been deprecated`

This is a setting in the GitLab configuration file. You will need to edit the config file and set the value to false.

>  1. Edit `/etc/gitlab/gitlab.rb` (or appropriate path) 
>  2. Edit the `grafana[enable]` value to be `false`. On my config this was on line 1689, but I recommend you search for the string `grafana` to find it. 
>
>      `grafana['enable'] = false`
>
>  3. Reconfigure GitLab by running `gitlab-ctl reconfigure`. Let this run and it should end with `gitlab Reconfigured!`
>  4. You can now resume the `apt` update that failed due to having Grafana enabled.

Once done, you will get the standard GitLab upgrade complete message and you'll be good to go.

 [gitlab]: {filename}2018_04_12_setting_up_gitlab.md
 [grafana]: https://grafana.com/
 [1]: https://docs.gitlab.com/ee/administration/monitoring/performance/grafana_configuration.html#deprecation
 [2]: https://docs.gitlab.com/ee/administration/monitoring/performance/grafana_configuration.html#integrate-with-gitlab-ui