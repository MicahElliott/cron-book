# Preface

Every job I’ve had in IT in the last 20 years has found me spending
significant time with cron. I’ve picked it up and learned to use it well
partly by reading some terse man pages, but much more by experimenting and
struggling to get it to behave well in the face of undeliverable mail, limited
environment, hidden output, strange version differences, and other surprises.
My goal in the book is to get you quickly aware of its quirks and cross-system
heterogeneity so that you’ll be productive in designing a scheduling system
across most or all of the computers you use or manage.

To my knowledge, there are no other books devoted entirely to cron, despite it
being 40 years old, and one of the most pervasive programs running in the
world. People seem to hack their way through cron setups. There are more than
10,000 cron-related questions on StackOverflow! On the outset, cron doesn’t
seem like a very exciting topic, but of the systems administrators I have
known, I’ve found that much of our time is spent plugging things into cron and
monitoring their behavior. It’s one of our most critical tool for keeping
machines functioning well, and I hope you’ll take the time to join me here in
mastering the art of _keeping things running_.

Some of cron is mundane: building filesystem indexes, rotating log files,
initiating backups.
But there are so many interesting use cases for cron! It’s used by Netflix to
... and ... You’ll get a taste of more of those cases in these pages, plus
you’ll get ideas for where you should (and shouldn’t!) be leveraging cron.

Cron is also an integral part of Big Data[^bigdata]. In that light, we’ll
compare its “batch” model to that of some “streaming”[^streaming] frameworks.

Cron has become so ubiquitous that you’ll even find its syntax in other tools,
like Jenkins, SugarCRM, and more. Maybe you’ll want to consider supporting
such syntax in a scheduling tool you’re working on!

[^bigdata]: massive amounts of usually high-performance data processing

[^streaming]: real-time processing of small chunks of data


## Who Should Read this Book

Anyone who considers themself a power user, developer, or sysadmin needs to be
able to work with cron. Anyone who works with cron needs to know most of
what’s in this book. So unless you’ve already mastered the ins and outs of
cron and its dependent tools, read this book!

“But cron is simple,” you say. No it’s not. Here’s are things you might not
know about the jobs you’re running:

- How long does it take to complete?
- Was last night’s run similar to last week’s?
- How are you filtering all its output?
- Is it starting at the same time across all systems? Is that okay?
- When did that job not run?
- How do you track its run history?
- Are you still getting mails from jobs that have failed?
- Are spam filters allowing all systems to get to your inbox?
- Are your crontabs organized into a sensible hierarchy across machines?

I won’t assume you’ve ever used cron before. I’m surprised how many developers
I meet who avoid cron because they didn’t get over the short but steep
learning hump. Cron’s syntax is intuitive and easy to get _something_ working
with, but I’ll move fast so as not to bore you with too much pedantry. Most
of cron is covered, but I’ve omitted a few things you’re unlikely to ever use
for the sake of honoring time -- cron’s currency.

Although you can find much of what’s in this book online, herein I’m uniquely
offering an organized way to ingest the most important pieces. I’ll also
provide many ways to actually _remember_ how and why things work the way they
do.


## Which Cron?

Cron is available and used (in some form) on all modern operating systems.
It’s not de facto on non-UNIXes, though, so we’ll focus on those OSs where
it’s already installed and set up. These include Linux family distributions
such as Red Hat (fedora, CentOS), Debian (Ubuntu), and Arch, plus the
(non-Apple) BSDs. I’ll briefly touch on making it work on Mac and Windows, but
I’ll assume your bent is toward making cron work well on Linux or BSD servers.
Most of the audience will have a Linux bias, so I’ll leverage that and dive
into Systemd specifics, since it’s here to stay (or coming) on all almost all
Linuxes.

The two mainstream cron versions are known as Vixie and Cronie. Their
differences are sprinkled around the pages when they matter.


## Organization of this Book

This is a short book, and you can read through it cover to cover. But it’s
divided into sections that should also work as a reference when you’re stuck
on something.

The end goal is to be able to wrangle cron into behaving well for your many
systems and tasks. We’ll even brainstorm what tasks are useful to be
implementing. A necessary side-effect of that is understanding some things
about operating systems and their “daemons”, mail transfer (postfix), log
rotating (logrotate), service management (systemd), monitoring (monit),
firewalls (firewalld), security models, and shells and their
environments. So we’ll go off on these brief tangents, but in the end, I
suspect you’ll be glad we did.

We’ll touch on the history of cron, but after that we’ll be keeping things
very modern. So modern, that we’ll be expecting things like systemd and
firewalld to be active.


## Author Contact Info

If you have any feedback or questions about the content of this book, please
email me at <mde@MicahElliott.com>.
