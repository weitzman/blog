---
title: DrupalSpoons FAQ
image: https://github.com/weitzman/blog/raw/master/assets/drupalspoons/devel_issues.png 
---

Hi friends. I'd like to share my answers to frequently asked questions about [DrupalSpoons](https://gitlab.com/drupalspoons/webmasters/-/blob/master/README.md). Comments are welcome in [Webmasters](https://gitlab.com/drupalspoons/webmasters/-/issues/23) or [on Slack](https://drupal.slack.com/archives/C013MP4UKC0).

#### What are some pain points at Drupal.org?

- **Horrible UX for adding images**. Sorry, I just don't know what adjective to use. Below is what we show after clicking the image button in the toolbar. I guess you are just supposed to know to attach your image file and then copy attachment URL and put the path substring into the dialog?!. No client I know would accept this solution in 2020. DrupalSpoons allows you to drag images into the textarea, paste from clipboard, etc. Remember that images vastly increase the quality of bug reports, support requests, and feature proposals.
  *![Image upload](https://github.com/weitzman/blog/raw/master/assets/drupalspoons/comment_image.png)
- **Frequent patch rerolls and CLI conflict resolution**. Because git knows when a branch diverged from its base, it can often merge the branch even when the parent has diverged significantly. Patches lose this ability and thus require more frequent re-rolls. [Here is an example from drupal.org](https://weitzman.github.io/blog/code-syntax). Furthermore, Gitlab has [web based conflict resolution](https://docs.gitlab.com/ee/user/project/merge_requests/resolve_conflicts.html), for those times when Git can no longer work out how to apply a change.
- **Multiple tools required to edit code/docs**. In order to propose a change on rupal.org, even a docs change, one must clone the repo, open an editor, generate a patch file, and attach it to a comment. This is all one flow _within your web browser_ when using the excellent [Gitlab Web IDE](https://about.gitlab.com/blog/2020/05/28/using-gitlab-web-ide-gitlab-ci-cd/). Browser based editing welcomes even novice users into the Git workflow.
- **Nitpicks** It is common practice at drupal.org to post comments that suggest trivial changes to code style, code comments, etc. [Suggest Changes](https://docs.gitlab.com/ee/user/discussions/#suggest-changes) tranforms that comment from an annoyance to an actionable improvement. The merge request author can easily accept the suggestion from her web browser.
![Suggest changes](https://github.com/weitzman/blog/raw/master/assets/drupalspoons/suggest3.png)
- **Notifications** Drupal.org has email notifications b ut they are so ugly that I doubt many but I use them :). Gitlab offers in-browser notifications, [a personal todo list](https://docs.gitlab.com/ee/user/todos.html),[granular email notifications](https://docs.gitlab.com/ee/user/profile/notifications.html), and reply via email. Good notifications keep people engaged and thus improve velocity.
- **CI inflexibility**. Unlike DrupalCI, projects can override any part of the template. For example, projects can add new services like Solr/Elasticsearch, or Redis/Memcache. Projects can use a different DB backend (e.g. Oracle) or custom PHP extensions.
- **Lack of batch editing**. Issue metadata must be edited on an issue by issue. There is no batch editing. Right now many projects are adding a new semantic major version. They all face the daunting task of editing dozens or hundreds of open issues to move them to the new version. [Drupal sysadmins doe this today via CLI scripting](https://www.drupal.org/project/infrastructure/issues/3132269).
- **Lack of writable web services API**.  Chat bots, batch editing, triage policies, etc. are all possible after we move to vanilla Gitlab and enjoy its [API](https://docs.gitlab.com/ee/api/).

###### Hey, but what about ...
- **Two URLs**. Although the issue remains the focal point for discussing a change, code review moves to its Linked merge request. So there are two URLs to consider when reviewing a code change. This is sub-optimal but the top product teams BitBucket, Github, and Gitlab have not found a way to harmonize these URLs either. Also note that the proposed [Issue Workspaces](https://www.drupal.org/project/drupalorg/issues/2488266) solution has same behavior (with jarring UX due to mismatched UIs).
- **Issue credits**. [DrupalSpoons continues the issue credit system with minor changes](https://gitlab.com/drupalspoons/webmasters/-/issues/2). The nice feature where a contributor specifies on a per-comment basis which org he is working for is removed. The cockpit where a maintainer decides who gets credit based on all prior comments is removed. In its place a maintainer can _Assign_ credit to one or more accounts (including Orgs). As a demo, [DrupalSpoons updates a Google Sheets with all earned credits](https://docs.google.com/spreadsheets/d/1m14yI71qwWpOS1WNXmF5GG6TbcuQlIlCUKT9tIob5F4/edit?usp=sharing). The DA can enhance this approach as needed.
- **Required Fields**. One regression (IMO) when using Gitlab issues is that Version, Component, et. become optional fields instead of required. This is mitigated via [Description Templates](https://docs.gitlab.com/ee/user/project/description_templates.html). These templates guide users toward filing complete and helpful issues. [Devel uses drupal.org's issue template](https://www.drupal.org/node/1155816).

#### What does the Drupal Association think about DrupalSpoons?
It is my impression from emails and [tweets](https://twitter.com/TimLehnen/status/1262816185353031680) that the DA is supportive of DrupalSpoons and is hoping for the community to embrace it. That makes intuitive sense; the DA has a long todo list and not enough time or money to achieve it. I'll bet that they don't relish paying once again to upgrade the project_* modules. With a DrupalSpoons approach, they don't have to. Similarly, DrupalSpoons proposes using Gitlab CI instead of DrupalCI. This will free up person-hours for other priorities. Disclaimers:

 -  The DA is a group of individuals and they likely differ in opinions.
 -  I'm no longer privy to internal discussions at the DA.

#### What keeps the DA from adopting vanilla Gitlab?
Allow me to introduce our opponent - the status quo. The DA has many passionate stakeholders, and they don't all agree on how to proceed. No product has every feature we want (if we could even agree on a set), so debate is inevitable. This a winning recipe for the status quo.

Its hard to defeat the status quo, but we have done it before. In 2010 we decided to move from CVS to Git. @webchick led a spirited discussion, [she chose Git](https://groups.drupal.org/node/48818#comment-133893), and we've been enjoying a better workflow ever since. We can win again.

#### How can you help move beyond the status quo?
Let your community know that you would be thrilled to develop Drupal on vanilla Gitlab:

1. [Join DrupalSpoons](https://gitlab.com/drupalspoons/webmasters/-/blob/master/docs/onboarding_user.md)
1. [Move contrib modules to DrupalSpoons](https://gitlab.com/drupalspoons/webmasters/-/blob/master/docs/onboarding_project.md).
1. Retweet on social media @TODO Paste link.
1. Open issues for your favortite modules kindly suggesting that they move to DrupalSpoons.

#### Is anyone using DrupalSpoons today?
Adoption is going well. I see abuot [80 issues](https://gitlab.com/drupalspoons/webmasters/-/issues?page=2&scope=all&state=closed) so far where people joined DrupalSpoons, or requested to migrate their project. You will likely recognize many members as they are influential in Drupal.