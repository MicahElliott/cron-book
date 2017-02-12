# The Little Cron Book
# Cron, in Small Batches
# Doing Cron Right
# Effective Batching with Cron
# Idiosyncratic Cron
# Five-Star Cron Czar
# Timeless Cron

## Icons
{icon=fire-extinguisher}
G> ## Vroom!
compass
cogs
diamond
film
birthday-cake
bomb
bullhorn
lightbulb-o
database
globe
money
signal (chart increasing)
thumbs-up-o
tree
trophy
umbrella
shield (security)
send-o (email)
life-ring
map(-signs)
road
sliders
wrench
tty
quote-left
puzzle-piece
rocket
link
paperclip
hand-o-right
github
linux
wikipedia-w
fort-awesome
stack-overflow

Add book here at some point:
https://www.goodreads.com/book/new?book[title]=cron

Introduction
  Debian-based (vixie) or RedHat/Arch (cronie)
  what version of cron do you have?
  assume systemd
  cron is a daemon (what’s that?)
  batches vs Streaming
  how does it actually work (fork and sleep)
  http://stackoverflow.com/questions/3982957/how-does-cron-internally-schedule-jobs
  running on your laptop
  running on servers
  history

Syntax and Variables
  custom vim syntax/highlighting
  spaces in vars (not like shell)
  no var substitution: bad: PATH=/usr/local/bin:$PATH
  multiple email addresses
  debugging/printing variables

Shells (sh, dash, bash, zsh)
  zsh for everything

In-memory crontabs (/var/spool, EDITOR=vi) vs files
  multi-user vs system-wide
  listing, editing, piping (both)
  loading from file on stdin

Execution Environment
  custom shell
  shell quoting
  what is an interactive shell? (tty)
  not a login/interactive shell
  no .bashrc
  use a ~/.cronsh (?)
  zsh
  wish
  rubysh
  chruby
  virtualenv

Lifecycle of a Job
  state machine diagram

One-offs
  && beep
  && mailx
  at, atq, atrm
  batch

Time Table of jobs on various servers
  (calendar-like view for a day?)
  common: logrotate, locatedb
  crunchers: upacs, fidelity (report generation)
  crm: sugar’s jobs?
  monitor
  db server: compress tables
  app server: compile logs
  queue server (redis/resque/rabbit)

Recipes
  2-minute freshdesk updates

Security
  allow/deny
  limited PATH
  SELinux
  ssh-agent

Error-proofing Email Setup
  all output goes to mail
  clients
    tutanota
    gmail
    mutt
  checking mail: mailx, mutt
  opening firewalld ports
  tls secure mail sending
  dig/host: PTR, DKIM, SPF
  single server mail relay(?)
  watching logs with `journalctl`
  configuring postfix
    postmap canonical
    mydomain
    ipv4
  [CheckMX](https://toolbox.googleapps.com/apps/checkmx/check?from=support.google.com&origin=checkmx-widget&domain=)
  [Kitterman SPF testing](http://www.kitterman.com/spf/validate.html?)
  [MXToolbox](http://mxtoolbox.com/SuperTool.aspx)
  viewing headers
    use “show original” in gmail

Documentation
  Navigating man pages: cron(8), crontab(5), crontab(1)
  Linux books
  StackOverflow tags: cron, crontab

Managing Time
  UTF time
  no PM!
  NTP setup
  daylight savings

Testing
  docker setup?
  lunch-time test: email working, time set, ntp in sync
  watching log files (/var/log/cron vs journalctl)
  checking for mail that stuck locally

Keeping Systems Tidy
  check-mail (check for local unwanted system mail)
  check-reboot
  check-pkgs-update (yum-cron or your own)

Performance
  running minutely?
  detecting overlapping runs

Sounding alarms
  opsgenie/pagerduty

The Venerable and Ubiquitous 5-star syntax
  The 6th field: user
  days of the week: mon - sun
  keywords: @daily, @weekly

More Syntax
  newlines
  multiple commands

Output
  silence for good cases (fd-teacher-school-high)
  exit statuses (non-zero does not trigger email alone)
  directing stderr to stdout

Locaation of crontabs

How to name a crontab

Editor support
  extension `.crontab`

Anacron
  yum install at
  sc start atd
  run-parts
  offline mode
  reducing all-at-once cluster load

Random Start Times

Dir Tree of crontabs

Common use cases/examples (see pinok.io landing)
  birthday calendar
  reminders
  desktop wallpaper rotator (see flickpapr hack)
  rss2email
  nightly database backups
  thermostat
  xmas lights

Starting/stopping/reloading
  uses inotify

Alternatives
  systemd as cron replacement

Other cron Advancements
  chronos
  [Amazon Simple Workflow](https://www.youtube.com/watch?v=3PsY7_ttcKk&feature=youtu.be)
  [Heroku Scheduler](https://devcenter.heroku.com/articles/scheduler)

Wrapping in Scripts
  executable
  nice, wish
  chruby

Other uses in the wild
  tools that use/manipulate crontabs (sugarcrm, whenever, jenkins)

Cron with Config Mgmt/Automation Tools
  Ansible: http://docs.ansible.com/ansible/cron_module.html

Interlude: Intro to related tools
  postfix
  monit
  logrotate
  systemd

Wrapping inside cron (simple run-mailer how-to, wish)
  move complex/multi-commands into little called scripts
  writing your own wrapper
  using a wrapper service

Cron for Monitoring
  how monit works
  when to use monit

System-specific cron setups

Deploying crontabs
  Editing crontabs across systems (git/ansible)

Common Mistakes
  no sudo for non-tty
  swapping hours and minutes
  star in wrong column

List of Quirks
  doing something with X11

List of Best Current Practices (BCPs)
  use two digits
  use sun instead of 0
  align columns

Cron FAQs (top interesting questions)
  viewing crontabs across all users
  http://stackoverflow.com/questions/610839/how-can-i-programmatically-create-a-new-cron-job
  http://stackoverflow.com/questions/2366693/run-cron-job-only-if-it-isnt-already-running
  http://stackoverflow.com/questions/8132355/test-run-cron-entry
  [DST](http://stackoverflow.com/questions/13195999/daylight-savings-and-cron)
  http://stackoverflow.com/questions/2229825/where-can-i-set-environment-variables-that-crontab-will-use
  http://stackoverflow.com/questions/2893954/how-to-pass-in-password-to-pg-dump
  http://stackoverflow.com/questions/8899737/crontab-run-in-directory
  http://stackoverflow.com/questions/4811738/cron-job-log-how-to-log
  [windows](http://stackoverflow.com/questions/707184/how-do-you-run-a-crontab-in-cygwin-on-windows)
  http://stackoverflow.com/questions/10129381/crontab-path-and-user
  http://stackoverflow.com/questions/6971595/how-to-set-a-cron-job-to-open-a-web-page-in-the-browser-using-crontab
  http://stackoverflow.com/questions/10193788/restarting-cron-after-changing-crontab-file
