# Syntax and Variables

## Syntax

     ┌───────────── min (0 - 59)
     │ ┌────────────── hour (0 - 23)
     │ │ ┌─────────────── day of month (1 - 31)
     │ │ │ ┌──────────────── month (1 - 12)
     │ │ │ │ ┌───────────────── day of week (1 - 7, or mon - sun)
     │ │ │ │ │
     │ │ │ │ │
     * * * * *  command-to-execute arguments ...  # comment documentation

Note that times are 0-based, but dates are 1-based, just like the real world.

The five-star syntax employed by cron can seem intimidating at first, but is
now adopted so widely that it’s worth celebrating as a successful DSL. Some
have invented more verbose syntaxes, but I find five-star to be a beautiful
invention.

It as been adopted by SugarCRM, Jenkins, and others.

The `*` mean first-through-last values. For minutes, you could write `0-59`
instead of `*`.

### Some Examples

Every minute:

    * * * * *

Every hour (at top of hour):

    0 * * * *

Every day (at midnight):

    0 0 * * *

### Advanced 5-Star Spec

You can fine-tune your timespec. A `/` means _every so many_.

    */10 * * * *

A `-` indicates a range. This will run on the 42^nd^ minute of the work hours
in a typical business week.

    42 9-17 * * mon-fri


(version dependent?)

## The Modern Algorithm

## Comments

Just like in most other scripting syntaxes, the `#` character starts a
comment used for documentation (or for disabling code). Unlike other
languages, you can’t put them at the end of a line! This will cause problems:

    MAILTO = you@example.com  # that’s me!
    SHELL = /bin/zsh  # because I <3 Zsh

Quotes are not kept literally, but are used for preserving spaces.

    MY_NAME = 'Schmoe Hawk'

## Variables

As you just saw, cron supports a small set of variables that control its
behavior. There are two types to be aware of, and their difference trips up
many.

As with many tools, `SHELL` (or `USER` on BSDs), `LOGNAME`, and `HOME` are
taken from your entry in `/usr/passwd`. You can override `SHELL` and it can be
useful to do so.

... How/why to use zsh ...

It’s important to note that a crontab file is interpreted rather differently
than a shell script. It’s not evaluated wholesale by your shell, but by the
cron parser. So, while you’re used to shell variables requiring no spaces:

    export FAVE_COLOR=chartreuse HATED_COLOR=beige

In crontab, this would need to be on two lines, and can contain spaces. I even
like to add spacing just to remind myself (and others) it’s not shell.

    FAVE_COLOR  = chartreuse
    HATED_COLOR = beige

Yes, crontab variables are implicitly exported.

You can actually see all the variables in your cron with the following test:

    # Make up something
    FOO = whatever

    09 15 * * *  env

On my Arch Linux system, this is:

    SHELL=/bin/sh
    MAILTO=mde@micahelliott.com
    USER=mde
    LOGNAME=mde
    PATH=/usr/bin:/bin
    PWD=/home/mde
    HOME=/home/mde
    LANG=en_US.UTF-8
    SHLVL=1
    FOO=whatever
    _=/usr/bin/env

Wow, that’s not much! Note that the variables don’t need to be all upper-case,
and they can even be interspersed through a crontab.

W> ### Caution!
W>
W> There is no variable substitution! You cannot use variable substitution for
W> previously set variables.
W>
W>    FOO = whatever
W>    BAR = $FOO
W>    path2 = $PATH

Crontab variables are exported. If you set `FOO = whatever` in your crontab,
all scripts that run will see it.

### Other important variables treated by cron

The `PATH` variable determines what commands will be available. Note that this
is often much more restrictive than what you’re used to in your regular
interactive shell.

    PATH=/usr/bin:/bin

Note that you need to set the whole path from scratch; you can’t build it up
the way you do in shell environments, like

    PATH=$HOME/bin:$PATH  # no workie

Things to consider adding to `PATH`:

    Java -- /opt/java8
    Home scripts -- $HOME/bin

As mentioned above, a crontab is not evaluated by `SHELL`. However, certain
parts are passed directly through to the shell.

    07 * * * *  echo pretty, pretty good 2>&1
    ^^^^^^^^^^  ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
       cron                  shell

You can use `~` to represent `$HOME`.

MAILTO = $LOGNAME
MAILTO = you@example.com,admins@example.com

No spaces allowed between recipients!

MAILFROM

RANDOM_DELAY

## Limited Shell Environment

A typical default crontab 


## 
