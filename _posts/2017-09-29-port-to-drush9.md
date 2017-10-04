---
title: Porting Commands to Drush 9
image: https://github.com/weitzman/blog/raw/master/assets/drush-port/core-requirements.png 
---
Drush 9 features a deep rewrite of our app, both user facing and internals. We created and open sourced [AnnotatedCommand](https://github.com/consolidation/annotated-command) ([example]((https://github.com/drush-ops/drush/blob/3ada88d72d654c20c57de7f1f5cbac396e033b2e/src/Drupal/Commands/core/DrupalCommands.php#L95-L119))), [OutputFormatters](https://github.com/consolidation/output-formatters), and [Config](https://github.com/consolidation/config). We leveraged Symfony Console for our CLI fundamentals. For details on Drush9, see the [video](https://youtu.be/FNzXI_VRF48) or [slides](https://docs.google.com/presentation/d/1syT5-fv4nc6yct3q4G9YWvvGsU3sFJSxUrtXHoQjvPo/edit?usp=sharing) from our [Drupalcon Vienna presentation](https://events.drupal.org/vienna2017/sessions/drush-9-lean-and-modern).

![Annotated Command example]({{ site.url }}/assets/drush-port/core-requirements.png "An example Annotated Command")

Unfortunately, old commandfiles such as example.drush.inc no longer load in Drush 9 (since beta5). We've made it relatively painless to port this code to Drush 9. The video below shows the drush generate command porting the [migrate commands](http://cgit.drupalcode.org/migrate_tools/tree/migrate_tools.drush.inc). Detailed instructions are below the video.

<script type="text/javascript" src="https://asciinema.org/a/epC3MGuQwvMiTskfkhBIlKmDL.js" id="asciicast-epC3MGuQwvMiTskfkhBIlKmDL" async></script>

1. Using Drush 9 on a working site, run `drush generate drush-command-file`. `generate` is a wrapper for the [Drupal Code Generator](https://github.com/Chi-teck/drupal-code-generator) library. 
1. You will be prompted for 2 pieces of information:
	1. Module  name: example
	2. Absolute path to legacy Drush command file: `/path/to/example.drush.inc`
1. Drush writes 2 files to the Example module:
	1. drush.services.yml. No edits are needed unless you want to inject Drupal dependencies into your class (e.g. [yml](http://cgit.drupalcode.org/devel/tree/drush.services.yml), [class](http://cgit.drupalcode.org/devel/tree/src/Commands/DevelCommands.php).
	1. ExampleCommands.php: Each item in `example_drush_command()` in your old commandfile has been transformed into an Annotated method in a new ExampleCommands file. Copy the body of your command callback functions into the corresponding method in ExampleCommands. Then modernize the code as below. Compare the  [Drush9 commands](https://github.com/drush-ops/drush/tree/master/src/Drupal/Commands) versus the [Drush8 commands](https://github.com/drush-ops/drush/tree/8.x/commands) for guidance.
		1. Replace `drush_log()` with `$this->logger()->info()` or `$this->logger()->warning()` or similar.
		2. Replace `drush_set_error()` with `throw new \Exception()`
		3. Replace `drush_print()` with `$this->output()->writeln()`
		4. Replace `drush_sitealias_get_record()` as with `$this->siteAliasManager()->getSelf()`. From there you can call getRoot(), getUri(), legacyRecord(), and other handy methods. In order for this to work, edit your class to implement SiteAliasManagerAwareInterface ([LoginCommands](https://github.com/drush-ops/drush/blob/master/src/Commands/core/LoginCommands.php) is an example). 
		5. Optional - move user interaction to a [@hook interact](https://github.com/consolidation/annotated-command/blob/2.7.0/README.md#hooks). [Also, note the new `$this-io()->confirm()` and `$this->io()->ask()` methods](https://github.com/drush-ops/drush/blob/master/src/Style/DrushStyle.php#L8).
1. Run `drush cr` to add your commandfile to the Drupal container.
1. Congrats - your commands are now runnable in Drush9! 
	1. Please post a patch if Example is a Contrib module. 
	1. Please leave example.drush.inc in your module so that Drush 8 users may still use it.
	

I'm in #drush on [IRC](https://www.drupal.org/irc) and [Slack](https://www.drupal.org/slack) in case anyone wants help with porting.

Note that `drush generate` knows how to generate controllers, plugins, config, services, libraries, etc. A blog post about generate is coming soon.


<p style="background-color: #008A3C; color: white; padding: 10px;">Moshe is available for hire as a solo consultant or as a duo with the inimatable Campbell Vertesi. Moshe has worked on Drupal core since 2001, and loves mentoring Drupal teams. Moshe loves to set up CI and developer workflows for PHP/JS teams.</p>