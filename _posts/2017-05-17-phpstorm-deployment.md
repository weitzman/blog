---
title: PHPStorm Remote Deployment and Debugging
---
I've long preferred doing web development on my local laptop. Unfortunately, my current consulting gig has unusual requirements. Specifically, I'm migrating content from an Oracle database into Drupal. The Oracle database is behind an IP based VPN - I can't connect to it via a particular AWS instance that's running our Drupal 8 site. My PHP has to run at AWS, and can't run locally. Happily, PHPStorm thrives in this situation. PHPStorm Deployments sync code from my laptop to AWS via SSH. Further, I'll show how to configure the Debugger to step through remote execution of PHP CLI scripts. Remember kids, the debugger is your friend.


This blog is a terse "how-to". Refer to [Sync changes and automatic upload to a deployment server in PhpStorm](https://confluence.jetbrains.com/display/PhpStorm/Sync+changes+and+automatic+upload+to+a+deployment+server+in+PhpStorm) and [Remote Debugging in PHPStorm via SSH Tunnel](https://confluence.jetbrains.com/display/PhpStorm/Remote+debugging+in+PhpStorm+via+SSH+tunnel) for more detail.

### Remote Deployment

1. Start creating a new Deployment configuration: _Tools => Deployment => Configuration_
1. Enter a name and pick SFTP for the type.
1. On the connection tab, enter the SSH details for remote server.
1. On the Mappings tab, tell PHP storm how the remote path maps to the local path. In my case:
 ```
Local path: /Users/moshe.weitzman/reps/b4
Remote path: /var/www/drupal
```
1. Test by browsing the remote host: _Tools => Deployment => Browse Remote Hosts_
1. You may now upload from local to remote via _Tools => Deployment => Upload To [name]_. Explore other options in this menu that may better suit your use case.

### Remote CLI Debugging over an SSH Tunnel

##### On your local machine:

1. _Run => Start listening for PHP Debug Connections_
1. _Run => Break at the first line in PHP scripts_
1. In a terminal on local machine, run a command like `ssh -R 9000:localhost:9000 -A username@host`. The -R arguments creates a SSH tunnel such that port 9000 on the remote server forwards all traffic to port 9000 on your local machine. This neatly bypasses any networking challenges between you and the remote machine. Customize _username@host_ for your own needs.

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
1. If prompted about _Path Mappings_, choose to pull them from the Deployment specified above. 

### Troubleshooting

If you get stuck, I refer you again to these longer documents:  [Sync changes and automatic upload to a deployment server in PhpStorm](https://confluence.jetbrains.com/display/PhpStorm/Sync+changes+and+automatic+upload+to+a+deployment+server+in+PhpStorm) and [Remote Debugging in PHPStorm via SSH Tunnel](https://confluence.jetbrains.com/display/PhpStorm/Remote+debugging+in+PhpStorm+via+SSH+tunnel).