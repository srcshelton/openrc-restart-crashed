# openrc-restart-crashed

```
Usage: openrc-restart-crashed [--quiet] [-- service_to_restart[,...]]
```

Report on any services which have crashed, and restart them automatically if
they are specified on the command-line, or match those pre-defined at the top
of the script.  These services which will be restarted automatically by default
are:

* apache
* dovecot
* mysql
* opendkim
* postfix
* postgres

... and further services may be trivially inserted.

`openrc-restart-crashed` is intended to be run regularly from cron - a typical
`/etc/cron.d/openrc-restart-crashed` might read:

```
# Global variables
SHELL=/bin/sh
PATH=/sbin:/bin:/usr/sbin:/usr/bin
MAILTO=root
HOME=/

# Local variables
services="monit"

#  Fields:
#   minute              (0-59)
#   hour                (0-23)
#   day of month        (1-31)
#   month of year       (1-12)
#   day of week         (0-6, Sunday == 0)

* * * * *       root    test -x /usr/local/sbin/openrc-restart-crashed && /usr/local/sbin/openrc-restart-crashed --quiet -- $services

# vi: set nowrap:
```

... and would cause any of the services above and also `monit` to be restarted
within a minute if they were marked as crashed by OpenRC.
