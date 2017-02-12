# Email Setup

Cron and email go together like clowns and crying babies (mac & cheese, pb&j,
horse & carriage).
It’s hard to have one without the other.

Cron is inherently an email-based system, so it’s important to ensure that its
messages can get through to you and maybe even other systems you use to
track your batch jobs.

You may choose to have all jobs send some form of completion status. By
default, an email from cron looks something like:

    From: (Cron Daemon)
    Subject: Cron <bob@crunchie> the-job plus args
    Message: <anything your jobs printed to stdout/stderr>

If there was no output from `the-job`, then the email will not be generated.
(See the [silent success pattern] for utilizing this.)


## Postfix

You should already have _postfix_ and its prerequisites installed. Ensure that
it’s running on every system:

    systemctl status postfix
    systemctl start  postfix
    systemctl enable postfix
