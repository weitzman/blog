---
title: PHPStorm Remote Deployment.
---
I've long preferred doing web development on my local laptop. Now, I'm consulting for a client with unusual requirements. Specifically, this client is migrating content from an Oracle database into Drupal. The Oracle database is behind an IP based VPN so I can't connect to it except a particular  AWS instance that's running our Drupal 8 site. My PHP has to run at AWS, and can't run locally.

To my joy, PHPStorm thrives in this situation. In this blog I describe how to use PHPStorm Remote Deployments to sync code from my laptop to AWS via SSH. Further, I show how to configure the Debugger so I can step through remote execution of the code and fix those pesky bugs. Remember kids, debuggers are your friend.

This blog is a terse "how-to". Refer to [Sync changes and automatic upload to a deployment server in PhpStorm](https://confluence.jetbrains.com/display/PhpStorm/Sync+changes+and+automatic+upload+to+a+deployment+server+in+PhpStorm) and [Remote Debugging in PHPStorm via SSH Tunnel](https://confluence.jetbrains.com/display/PhpStorm/Remote+debugging+in+PhpStorm+via+SSH+tunnel) for more detail.

### Remote Deployment

Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.

### Remote Debugging

Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.

