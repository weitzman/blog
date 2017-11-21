---
title: Debugging Drush commands at Acquia
image: https://confluence.jetbrains.com/download/attachments/54345259/zend_server_tutorial_18.png?version=1&modificationDate=1410292135000&api=v2
---
It is a repeating need for me to debug Drush commands that run at [Acquia Cloud](https://www.acquia.com/products-services/acquia-cloud). When I say "debug", I refer to step-wise debugging via Xdebug. This is the best and only way to slay hard bugs. Down with print_r() debugging! 

This blog is a terse "how-to". Refer to [Acquia - Configuring Xdebug with PhpStorm](https://docs.acquia.com/article/configuring-xdebug-phpstorm) for more detail.

1. SSH with a tunnel: `drush @massgov.prod -v --ssh-options="-R 9000:localhost:9000" ssh`
    1. Replace `@massgov.prod` with your own site alias
    1. This command connects to Acquia via SSH *AND* opens a tunnel from Acquia's port 9000 to your laptop's port 9000. This allows PHP on Acquia to connect to your PHPStorm no matter what firewall separates you.
    1. To verify that the tunnel is working, you can run `netstat -nlt | grep 9000` in same terminal window. If you see output, you are good.
1. In your local PHPStorm project, _Run => Start listening for PHP Debug Connections_
1. In your local PHPStorm project, _Run => Break at the first line in PHP scripts_
1. Back in your terminal window that is ssh-ed into Acquia, run `XDEBUG_CONFIG= php -dzend_extension=xdebug.so -dxdebug.remote_enable=1  ../vendor/bin/drush core-status`
    1. The environment variable `XDEBUG_CONFIG=` has no value because none is needed. This nudges XDebug to connect back to PHPStorm.
    1. The two -d options enable and configure XDebug extension for the current request.
    1. The `/vendor/bin/drush` path points to the site-local Drush. Older Drupal sites that are not built with Composer can just use `drush` instead.
    1. `core-status` is just an example command. Replace with your own command and arguments/options.
1. If all went well, your PhpStorm should be debugging at line 1 of `drush`. If PHPStorm has a red warning about Path Mappings, click that link to set them up.   
    
![Debugging Drush commands at Acquia](https://confluence.jetbrains.com/download/attachments/54345259/zend_server_tutorial_18.png?version=1&modificationDate=1410292135000&api=v2) 
 
