---
title: Improve drupal.org code workflow with git merge
---
Drupal.org uses a patch based workflow in order to improve its software. Today I present a concrete instance where this significantly slowed us down. Views performance work is blocked when it didn't have to be. 

Today, Dries committed a fine patch by Sun which [moved a lot of test files into a happier place](http://drupal.org/node/2260121). After that commit, Dries reviewed [Daniel's Views patch](https://www.drupal.org/node/1849822#comment-9101469) which changed a couple of those same test files. Daniel's patch failed to apply so Dries [changed the issue to Needs Work](https://www.drupal.org/node/1849822#comment-9124083). **This doesn't need to happen anymore.** Git knows about file moves and can overcome this problem when we use it fully. If Daniel's change was a _merge_ instead of a _patch_, git uses its ancestry to apply changes to the moved test files. Try for yourself - the commands I ran are below.

We could all increase our velocity by using a merge workflow. Think of all the patches that need reroll currently which would not need reroll. Think of all the patches considered _disruptive_ when they don't need to be. I know that we can't avoid all 'Needs reroll' events, but we can avoid a lot.

For now, I encourage folks to use d.o. sandboxes or Github to host their forks, and to post `git merge` commands into the issue queue instead of uploaded patches. Discussion of these changes should remain in the main issue queue. I think we should document this workflow at [Drupal.org Git tutorials](https://www.drupal.org/node/1054594).

Feedback welcome.


---------------------
Start from a clean 8.0.x:
<pre>
# Create  branch whose tip is the commit before Sun's
$ git checkout -b daniel ebace9e
#Apply Daniel's patch
$ wget -O tmp.patch https://www.drupal.org/files/issues/1849822-view_render-19.patch; git apply tmp.patch;
$git commit -am "Fix #1849822. Convert (HTML) view rendering to a render array"
#Switch to main branch
$ git co 8.0.x
#Merge Daniel's patch and watch git's work its magic
$ git merge daniel
Auto-merging core/modules/views/tests/src/Unit/Routing/ViewPageControllerTest.php
Auto-merging core/modules/views/tests/src/Unit/Plugin/Block/ViewsBlockTest.php
Merge made by the 'recursive' strategy.
 core/modules/rest/src/Plugin/views/display/RestExport.php 
 core/modules/user/src/Tests/UserBlocksTest.php
 core/modules/views/src/Annotation/ViewsDisplay.php
 core/modules/views/src/Plugin/Block/ViewsBlock.php
 core/modules/views/src/Plugin/views/display/Attachment.php
 core/modules/views/src/Plugin/views/display/DisplayPluginBase.php
 core/modules/views/src/Plugin/views/display/Feed.php
 core/modules/views/src/Routing/ViewPageController.php
 core/modules/views/src/ViewExecutable.php
 core/modules/views/tests/src/Unit/Plugin/Block/ViewsBlockTest.php
 core/modules/views/tests/src/Unit/Routing/ViewPageControllerTest.php
 core/modules/views/views.module
 core/modules/views/views.theme.inc
 13 files changed, 147 insertions(+), 32 deletions(-)
</pre>
