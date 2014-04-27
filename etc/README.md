## /etc Scripts

These scripts primarily interact with the system as a whole and will likely be kept in `/etc`.

The update-motd routines are documented on the creators website at [Sail2Software.com](http://www.sail2software.com/2014/03/a-better-dynamic-motd.html).  Yes, this was all blatantly ~~stolen~~ forked from there.

#### Quick Install Instructions 


 I use toilet, and not figlet, so install required packages:

~~~
$ sudo apt-get install toilet update-notifier-common
~~~

Copy the relevant directories to /etc

~~~
$ sudo cp -Rp etc/* /etc
~~~

In `update-motd.d` there are two `10-sysinfo*` scripts, one for a single network interface system, and one for a dual, remove the one you do not need.

##### Configure Scripts to Run

Add the contents of `configs/crontab-root` to the root user's crontab, and then add the relevant portion of that same command to the `/etc/rc.local` so it will be run when the system reboots (so you don't have to wait the 30 minutes until the cron job runs).

Probably wouldn't hurt to run it by hand here as well.  This will create the necessary scripts the first time, i.e. since neither the cronjob nor the `rc.local` commands have run yet.

~~~
$ sudo /bin/run-parts --arg=/var/run/motd_local- /etc/update-motd_local.d
~~~

#####  Link the motd file to our new dynamic motd

This simply removes the default `/etc/motd` file and creates a symbolic link from that location to our new dynamic files:

~~~
$ sudo rm /etc/motd
$ sudo ln -s /var/run/motd /etc/motd
~~~

##### Enjoy

That should do it.  If everything has gone according to plan, you should now have a nice dynamic motd, a la the Ubuntu one, but which is significantly less resource intensive.

As always, the author's website and github repo contain the complete documentation and the latest updates:

1.  [Website](http://www.sail2software.com/2014/03/a-better-dynamic-motd.html)
2.  [Github Repo](https://github.com/scotte/linux-configs)
