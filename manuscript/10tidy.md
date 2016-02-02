# Keeping Systems Tidy

## Scripting Pattern

You want cron to be silent for the normal, expected cases. For the unexpected,
it should receive output and non-zero exits. So the common pattern is to do
your checks and tests, and output _nothing_ if all is well.

{title="Example 1: A simple local mail-checking script", lang=bash}
~~~~~~~~
mail_exists=

for u in myuser root; do 
  size=$(stat -c %s /var/spool/mail/$u)
  if (( size > 0 )); then
    mail_exists=true
    echo "Uh-oh, user $u on $(hostname) has mail." 2>&1
  fi 
done

[[ -n $mail_exists ]] && exit 1  # otherwise exit 0 implied
~~~~~~~~

Things to observe in this simple script:

1. Output is printed to `stdout` (via `2>&1`)
2. A boolean tracking flag variable `mail_exists` is off until triggered
2. Exit status is `1` for alerting case
4. Clean fall-through exit otherwise

This script can be conveniently run and tested outside of cron.




    #! /usr/bin/env zsh

    # reboot-needed — check different installed vs running kernel version
    # http://serverfault.com/questions/122178/how-can-i-check-from-the-command-line-if-a-reboot-is-required-on-rhel-or-centos

    # Check for updates
    yum --quiet check-update  # bad exit 100 if update needed
    es=$?

    LAST_KERNEL=$(rpm -q --last kernel | perl -pe 's/^kernel-(\S+).*/$1/' | head -1)
    CURRENT_KERNEL=$(uname -r)

    [[ $LAST_KERNEL != $CURRENT_KERNEL ]] && echo REBOOT

    if (( $es != 0 )) || [[ $LAST_KERNEL != $CURRENT_KERNEL ]]; then exit 1; fi



Check RAID failure:

    #! /usr/bin/env zsh

    # check-raid — check for megaraid failure; call from cron every ~10m

    alarm() {
      msg=$1
      print $msg
      mailx -s $msg redalert@example.opsgenie.net <<< $msg
      exit 1
    }

    # Check to see if MegaCli will do anything sensible. If not, probably not a RAID system.
    wc=$(/opt/megaraid/MegaRAID/MegaCli/MegaCli64 -AdpAllInfo -aALL |wc -l)

    (( wc < 10 )) && exit 0

    oks=$(/opt/megaraid/MegaRAID/MegaCli/MegaCli64 -AdpAllInfo -aALL |grep -P '(Critical|Failed) Disks\s+\: 0' |wc -l)

    (( $oks == 2 )) || alarm "RAID failure on $(hostname)"


shoot-mtr

cache-warm

## Retry Pattern

resque-pressure

Run once. If problem, write a temp file with a couple numbers in it. Next run
will check numbers and either clear numbers if improved, or alert if worsened.
