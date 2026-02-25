---
title: Drush's Final Act
image: https://github.com/weitzman/blog/raw/master/assets/drush-is-dead/dead.png 
---

I propose a new Drupal core initiative which aims to *rapidly* introduce a full featured CLI. I see two initial steps: 

1. Reroll, discuss, and merge [CLI entry point in Drupal Core](https://www.drupal.org/project/drupal/issues/3453474). This wonderful PR by [dpi](https://www.drupal.org/u/dpi) brings a minimal Symfony Console application into Drupal. It introduces a `drupal` application whose inital commands are those which currently ship with core - InstallCommand, RecipeCommand, etc. It also provides way for extensions and vendor packages to add commands.
2. Add a dependency on Drush and bring in as many commands as the [product managers](https://project.pages.drupalcode.org/governance/core/drupal-core/#product-manager) deem appropriate. See https://git.drupalcode.org/issue/drupal-3453474/-/merge_requests/1 which extends the above MR. Note how simple it is to bring in a Drush command - a tagged service in core.services.yml is all it takes. Drush recently converted all its commands to vanilla Console format so that makes it trivial to incorporate them into `drupal`.

### Followups

1. Incorporate the more exotic commands which bootstrap Drupal or execute subcommands. Those are updatedb, config:import, deploy:\*, sql:\*, etc. I think these commands should be rewritten for Drupal core.
2. Start copying commands from Drush to Drupal core so they can be maintained using the core process. My reasoning for not doing this from the start is that this adds a lot of code review and tests. I think its vital for Drupal from a competitive perspective to get to a full featured CLI quickly. Initially keeping the commands in Drush helps this goal.

### Generous Review

We've had a few attempts at a CLI in Drupal core over the years. These attempts stalled due to debates about command name or bootstrap details or numberous other bikeshed details. Lets please not to do that again. Drupal is fighting to stay relevant. Its just not helpful to hold up the whole feature on trivial details. Please review generously and if you call for a change, create/update an MR as well.

### Drush's Final Act

My end goal here is the death of Drush. It has served an incredibly long and useful life. Its death will be its most impactful and generous act.

Along the way, we may lose some niceties like Drush config and Site aliases. Platforms already have solutions for replacements aliases such as Lagoon (Amazee), Terminus (Pantheon), acli (Acquia), etc.

Until the death is complete, it is trivial to run Drush commands along with Drupal commands. Sites will have years to convert their scripts to use `drupal` instead. Drush support will continue through this transition.

