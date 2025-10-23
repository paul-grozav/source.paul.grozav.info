---
layout: page
title: SysLog
---

C++ application:
{% highlight cpp %}
//----------------------------------------------------------------------------//
// Author: Tancredi-Paul Grozav <paul@grozav.info>
// See: ftp://ftp.gnu.org/old-gnu/Manuals/glibc-2.2.3/html_chapter/libc_18.html
// See: https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/
//   7/html/system_administrators_guide/s1-basic_configuration_of_rsyslog
//----------------------------------------------------------------------------//
#include <syslog.h> // Only on linux and part of glibc GNU C Library
//----------------------------------------------------------------------------//
int main()
{
  // LOG_UPTO - generates a mask with the bits on for a certain priority and all
  // priorities above it
  setlogmask( LOG_UPTO(LOG_NOTICE) );

  // Optional.Open connection to syslog in preparation for submitting messages.
/*  openlog(
    // Identification string that will be prefixed to each message
    "exampleprog"
    // bit string: see documentation
    , LOG_CONS | LOG_PID | LOG_NDELAY
    // The default facility code for this connection
    , LOG_LOCAL1
  );
*/

  // Log messages
  // The emergency level will produce a BEEP, only used for PANIC!
//  syslog(LOG_EMERG, "Emergency - The message says the system is unusable.");
  syslog(LOG_ALERT, "Alert - Action on the message must be taken immediately.");
  syslog(LOG_CRIT, "Critical - The message states a critical condition.");
  syslog(LOG_ERR, "Error - The message describes an error.");
  syslog(LOG_WARNING, "Warning - The message is a warning.");
  syslog(LOG_NOTICE, "Notice - The message describes a normal but important"
    " event.");
  syslog(LOG_INFO, "Info - The message is purely informational.");
  syslog(LOG_DEBUG, "Debug - The message is only for debugging purposes.");
  // Results are undefined if the priority code is anything else.

  // Close connection to Syslog, if there is one. No real reason to do it.
  //closelog();

  return 0;
}
//----------------------------------------------------------------------------//
{% endhighlight %}

syslog configuration file:
{% highlight cpp %}
# ============================================================================ #
# Author: Tancredi-Paul Grozav <paul@grozav.info>
# ============================================================================ #
# This is a configuration file for rsyslogd. Place it in /etc/rsyslog.d
# ============================================================================ #
# Set verbose logging format
$template precise,"%syslogpriority%,%syslogfacility%,%timegenerated%,%HOSTNAME%,%syslogtag%,%msg%\n"
$ActionFileDefaultTemplate precise

# Simply redirect to file by program name, then stop processing this message:
#:syslogtag, isequal, "main.exe:"    /var/log/pgrozav_main.exe.log
#& stop

# using the new format you could do:
:syslogtag, isequal, "main.exe:" action(type="omfile"
  file="/var/log/pgrozav_main.exe.log"
) & stop

# Return to default logging format
$ActionFileDefaultTemplate RSYSLOG_TraditionalFileFormat


# To support logging on remote servers you can use this OBSOLETE format:
# 1. This server receives log messages, so set rsyslog like this:
# provides TCP syslog reception
module(load="imtcp")
input(type="imtcp" port="514")
# 2. This server sends log messages, so set rsyslog like this:
# *.* @@172.17.0.1:514
# @@<IP>:<PORT> means the messages will be sent over TCP to that IP

# To support logging on remote servers you can use this NEW format:
# 1. Enable module imtcp as above
# 2. Configure client to forward messages, like this:
# *.* action(type="omfwd"
#      action.resumeRetryCount="-1"
#      queue.size="10000"
#      queue.type="LinkedList"
#      queue.filename="pgrozav_fwdQ"
#      queue.saveonshutdown="on"
#      target="172.17.0.1"
#      port="514"
#      protocol="tcp"
#    )
# ============================================================================ #
{% endhighlight %}

