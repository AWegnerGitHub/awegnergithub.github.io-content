Title: How to fix GitLab 18 error regarding git_data_dirs
Date: 2025-07-06 08:00
Tags: technical
Category: Technical Solutions
Slug: gitlab_18_git_data_dirs_resolution
Summary: GitLab 18 removes `git_data_dirs` and if you have been using it and didn't notice the deprecation warnings, an update to GitLab 18 will fail. This is a simple fix.
Status: published

[TOC]

## Introduction

Several years ago I [set up GitLab in my home environment][1]. I apprecate past me documenting the basic steps I took to do this, because I'll need them again at some point when I make upgrades to my home lab. I've documented somethings I've done (like [setting up GitLab runners][3], or dealing with [Grafana being deprecated within GitLab][4] or utilzing [GitLab to automatically backup my Obsidian notes][5]) Unfortunately, I didn't document everything. One of those things that I didn't document was changing where my repositories are stored on disk by default.

In GitLab 17.8 (January 2025), a new deprecation warning started appearing.

> git_data_dirs has been deprecated since 17.8 and will be removed in 18.0. See https://docs.gitlab.com/omnibus/settings/configuration.html#migrating-from-git_data_dirs for migration instructions.

I'm a little annoyed that there were only 5 months of notice on this because major versions are released in May. In any case, if you attempt to update to version 18 or beyond and are utilizing `git_data_dirs`, the upgrade will fail.

## Solution

As usual, GitLab is good at providing [documentation on resoluting and migrating][2] through the deprecations. However, on my first attempt I encountered an error. I believe it's because I missed a note buried in the text - not the code blocks - the first time.

> Note that the `/repositories` suffix must be appended to the path because it was previously appended internally.

The solution is to open `/etc/gitlab/gitlab.rb` (you probably should make a backup first). Find the `git_data_dirs` line. For me this was around line 455. Then comment out this entire block.

Then find (or add) the `gitaly['configuration']` block. This was immediately after the `git_data_dirs` section for me. Uncomment it, and add the appropriate path (from your `git_data_dirs` block) and add `/repositories` to the end of it.

Once you are done, run

    gitlab-ctl reconfigure
    gitlab-ctl restart

Give it a minute to fire up all the GitLab subcomponents, and then you should be able to update GitLab beyond version 18.

### New code section

My `gitlab.rb` now has this:

    #git_data_dirs({
    #  "default" => {
    #    "path" => "/previous/path/to/repos"
    #   }
    #})

    ### Gitaly settings
    gitaly['configuration'] = {
    storage: [
        {
        name: 'default',
        path: '/previous/path/to/repos/repositories',
        },
    ],
    }

## Conclusion

With this quick change, I can continue to hold the repositories at a location of my choosing. The biggest thing is to add `/repositories` to the `path` in the new Gitly configuration. With this change, I can continue to utilizing the current version of GitLab - a tool that I still find invaluable.



 [1]: {filename}2018_04_12_setting_up_gitlab.md
 [2]: https://docs.gitlab.com/omnibus/settings/configuration/#migrating-from-git_data_dirs
 [3]: {filename}2019_10_22_gitlab_runners.md
 [4]: {filename}2023_08_23_gitlab_disable_grafana_163.md
 [5]: {filename}2023_09_26_obsidian_gitlab_setup.md