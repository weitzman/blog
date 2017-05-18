---
title: PHPStorm Remote Deployment and Debugging.
---
I've long preferred doing web development on my local laptop. Today, I'm consulting for a client with unusual requirements. Specifically, this client is migrating content from an Oracle database into Drupal. The Oracle database is behind an IP based VPN - I can connect to it via a particular AWS instance that's running our Drupal 8 site. My PHP has to run at AWS, and can't run locally.

To my joy, PHPStorm thrives in this situation. In this blog I describe how to use PHPStorm Remote Deployments to sync code from my laptop to AWS via SSH. Further, I show how to configure the Debugger so I can step through remote execution of PHP CLI scripts and fix those pesky bugs. Remember kids, the debugger is your friend.

This blog is a terse "how-to". Refer to [Sync changes and automatic upload to a deployment server in PhpStorm](https://confluence.jetbrains.com/display/PhpStorm/Sync+changes+and+automatic+upload+to+a+deployment+server+in+PhpStorm) and [Remote Debugging in PHPStorm via SSH Tunnel](https://confluence.jetbrains.com/display/PhpStorm/Remote+debugging+in+PhpStorm+via+SSH+tunnel) for more detail.

### Remote Deployment

Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.

### Remote CLI Debugging over an SSH Tunnel

##### On your local machine:

1. Run => Start listening for PHP Debug Connections
1. Run => Break at the first line in PHP scripts
1. In a terminal on local machine, run a command like `ssh -R 9000:localhost:9000 -A username@host`. The -R arguments creates a SSH tunnel such that port 9000 on the remote server forwards all traffic to port 9000 on your local machine. This neatly bypasses any networking challenges between you and the remote machine. Customize username@host for your own needs.

##### On the remote server:

1. Enable and configure the Xdebug PHP extension on the remote server. Use `php --ini` to figure out which ini file to edit. Then add:
```
zend_extension=xdebug.so
xdebug.remote_enable=1
xdebug.remote_host=127.0.0.1
xdebug.remote_port=9000

```
1. Use `php -i | grep -i xdebug` to verify that your edits are working (i.e. remote_enable is _enabled_).
1. In a terminal on the remote server, run a PHP CLI script such as `XDEBUG_CONFIG=  vendor/bin/drush st`. The first clause sets a required environment variable to null value. We only need this variable to be defined. It triggers XDebug to connect back to PHPStorm.
1. @todo ***Path Mappings*** 

### Troubleshooting

If you get stuck, I refer you again to these longer documents:  [Sync changes and automatic upload to a deployment server in PhpStorm](https://confluence.jetbrains.com/display/PhpStorm/Sync+changes+and+automatic+upload+to+a+deployment+server+in+PhpStorm) and [Remote Debugging in PHPStorm via SSH Tunnel](https://confluence.jetbrains.com/display/PhpStorm/Remote+debugging+in+PhpStorm+via+SSH+tunnel).