Wordpress Nightly Builds on OpenShift
=====================================

[![nightly builds dashboard](http://i.imgur.com/vNckYmZ.png)

[![choose scheme dashboard themes](http://i.imgur.com/1m2cIun.png)

This git repository helps you get up and running quickly w/ a Wordpress installation
on OpenShift.  The backend database is MySQL and the database name is the 
same as your application name (using getenv('OPENSHIFT_APP_NAME')).  You can name
your application whatever you want.  However, the name of the database will always
match the application so you might have to update .openshift/action_hooks/build.


Running on OpenShift
--------------------

Create an account at http://openshift.redhat.com/ and install the client tools (run 'rhc setup' first)

Create a php-5.3 application (you can call your application whatever you want)

```bash
$ rhc app create wordpress php-5 mysql-5 --from-code=https://github.com/tigefa4u/wordpress-nightly-openshift.git
```

That's it, you can now checkout your application at:

```bash
http://wordpress-$yournamespace.rhcloud.com
```
 
You'll be prompted to set an admin password and name your WordPress site the first time you visit this 
page.  

Note: When you upload plugins and themes, they'll get put into your OpenShift data directory
on the gear ($OPENSHIFT_DATA_DIR).  If you'd like to check these into source control, download the 
plugins and themes directories and then check them directly into php/wp-content/themes, php/wp-content/plugins.

Notes
=====

GIT_ROOT/.openshift/action_hooks/deploy:
    This script is executed with every 'git push'.  Feel free to modify this script
    to learn how to use it to your advantage.  By default, this script will create
    the database tables that this example uses.

    If you need to modify the schema, you could create a file 
    GIT_ROOT/.openshift/action_hooks/alter.sql and then use
    GIT_ROOT/.openshift/action_hooks/deploy to execute that script (make sure to
    back up your application + database w/ 'rhc app snapshot save' first :) )

Security Considerations
-----------------------
Consult the WordPress documentation for best practices regarding securing your wordpress installation.  OpenShift 
automatically generates unique secret keys for your deployment into wp-config.php, but you may feel more
comfortable following the WordPress documentation directly.
