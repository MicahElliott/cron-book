# Organizing Crontabs

The use of cron in this book is focused more heavily on multi-machine
deployments, rather than individual users controlling personal crontabs on
single machines. With that in mind, our use case benefits from separating our
scheduled tasks into multiple crontab files.

There is a nice pattern for organizing files that can be used for several
tools. Take a look in `etc`:

    # ls -d /etc/*.d

Depending on what you have installed, you’ll see something like:

    cron.d/
    monit.d/
    logrotate.d/
    yum.repos.d/

These directories serve as a nice area to store a set of files managed by
their correspondingly named tool.

For a database server, you may have files like:

    /etc/cron.d/database.crontab


## Top-Level System Crontab

There is a single entry-point file used by cron: `/etc/crontab`. This is a
good place to put anything you want all machines to do, since it’s easy to
deploy to all systems.

Then you can deploy machine-specific files to appropriate servers.



    # System-wide crontab

    SHELL = /usr/bin/zsh

    #PATH = /sbin:/bin:/usr/sbin:/usr/bin
    PATH = /sbin:/bin:/usr/sbin:/usr/bin:/home/bob/bin

    # This hopefully sets MAILTO for all sub-crontabs
    MAILTO = admin@example.com


    ### Jobs #############################################################

    00 08 * * * root reboot-needed.zsh

    00 07,19 * * * root check-raid.zsh

    00 08 * * * root check-mail.zsh

    00 12 * * * bob echo "$(hostname) says lunch time"
