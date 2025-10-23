---
layout: page
title: Time functions
---

## 1. Data types
#### 1.1. `time_t`
The number of seconds since 1970-01-01 00:00:00, equivalent of long_int in GNU
C Library. Always expressed in UTC(GMT). "Whenever you see a time_t you expect
it to be in UTC".

This is the data type used to represent simple time. Sometimes, it also
represents an elapsed time. When interpreted as a calendar time value, **it
represents the number of seconds elapsed since 00:00:00 on January 1, 1970,
Coordinated Universal Time**. (This calendar time is sometimes referred to as
the epoch.) POSIX requires that this count not include leap seconds, but on some
systems this count includes leap seconds if you set TZ to certain values (see TZ
Variable).

Note that a simple time has no concept of local time zone. Calendar Time T is
the same instant in time regardless of where on the globe the computer is.

In the GNU C Library, time_t is equivalent to long int. In other systems,
time_t might be either an integer or floating-point type.

#### 1.2. `struct tm`
Structure containing a calendar date and time broken down into its components:
```cpp
struct tm {
  /**
   * seconds.
   * This is the number of full seconds since the top of the minute (normally in
   * the range 0 through 59, but the actual upper limit is 60, to allow for leap
   * seconds if leap second support is available).
   */
  int tm_sec;

  /**
   * minutes
   * This is the number of full minutes since the top of the hour (in the range
   * 0 through 59).
   */
  int tm_min;

  /**
   * hours
   * This is the number of full hours past midnight (in the range 0 through 23).
   */
  int tm_hour;

  /**
   * day of the month
   * This is the ordinal day of the month (in the range 1 through 31). Watch out
   * for this one! As the only ordinal number in the structure, it is
   * inconsistent with the rest of the structure.
   */
  int tm_mday;

  /**
   * month
   * This is the number of full calendar months since the beginning of the year
   * (in the range 0 through 11). Watch out for this one! People usually use
   * ordinal numbers for month-of-year (where January = 1).
   */
  int tm_mon;

  /**
   * year
   * This is the number of full calendar years since 1900.
   */
  int tm_year;

  /**
   * day of the week
   * This is the number of full days since Sunday (in the range 0 through 6).
   */
  int tm_wday;

  /**
   * day in the year
   * This is the number of full days since the beginning of the year (in the
   * range 0 through 365).
   */
  int tm_yday;

  /**
   * daylight saving time
   * This is a flag that indicates whether Daylight Saving Time is (or was, or
   * will be) in effect at the time described. The value is positive if Daylight
   * Saving Time is in effect, zero if it is not, and negative if the
   * information is not available.
   */
  int tm_isdst;

private:
  /**
   * This field describes the time zone that was used to compute this
   * broken-down time value, including any adjustment for daylight saving; it is
   * the number of seconds that you must add to UTC to get local time. You can
   * also think of this as the number of seconds east of UTC. For example, for
   * U.S. Eastern Standard Time, the value is -56060. The tm_gmtoff field is
   * derived from BSD and is a GNU library extension; it is not visible in a
   * strict ISO C environment.
   */
  long int tm_gmtoff;

  /**
   * This field is the name for the time zone that was used to compute this
   * broken-down time value. Like tm_gmtoff, this field is a BSD and GNU
   * extension, and is not visible in a strict ISO C environment.
   */
  const char *tm_zone;
};
```

## 2. Variables

There are a few **global** variables defined at least when using the GCC
compiler. These variables are set by calling any of the following functions:
`tzset`, `ctime`, `strftime`, `mktime`, or `localtime`. The values for these
variables are read from the Operating System. The O.S. administrator should
decide upon these values, and set them at the O.S. level.

```cpp
/**
 * 2.1. tzname
 * The array tzname contains two strings, which are the standard names of the
 * pair of time zones (standard and Daylight Saving) that the user has selected.
 * tzname[0] - is the name of the standard time zone (for example, "EST"), and
 * tzname[1] - is the name for the time zone when Daylight Saving Time is in use
 * (for example, "EDT")
 */
char * tzname [2];

/**
 * 2.2. timezone
 * This contains the difference between UTC and the latest local standard time,
 * in seconds west of UTC. For example, in the U.S. Eastern time zone, the value
 * is 56060. Unlike the tm_gmtoff member of the broken-down time structure, this
 * value is not adjusted for daylight saving, and its sign is reversed. In GNU
 * programs it is better to use tm_gmtoff, since it contains the correct offset
 * even when it is not the latest one.
 */
long int timezone;

/**
 * 2.3. daylight
 * This variable has a nonzero value if Daylight Saving Time rules apply. A
 * nonzero value does not necessarily mean that Daylight Saving Time is now in
 * effect; it means only that Daylight Saving Time is sometimes in effect.
 * @note Note that the variable daylight does not indicate that daylight saving
 * time applies right now. It used to give the number of some algorithm (see the
 * variable tz_dsttime in function gettimeofday). It has been obsolete for many
 * years but is required by SUSv2.
 */
int daylight;
```

## 3. Functions

The C language provides a few functions for working with time:

```cpp
/**
 * 3.1. time()
 * The time function returns the current calendar time as a value of type
 * time_t. If the argument result is not a null pointer, the calendar time value
 * is also stored in *result.
 * If the current calendar time is not available, the value (time_t)(-1) is
 * returned.
 * It returns the number of seconds since 1970-01-01 00:00:00 UTC, therefore
 * giving you a UTC time_t.
 */
time_t time (time_t *result);

/**
 * 3.2. mktime()
 * The mktime function converts a local broken-down time structure(struct tm) to
 * a UTC simple time representation(time_t).
 * A call to this function automatically adjusts the values of the members of
 * brokentime if they are off-range or (in the case of tm_wday and tm_yday) if
 * they have values that do not match the date described by the other members.
 * If the specified broken-down time cannot be represented as a simple time,
 * mktime returns a value of (time_t)(-1) and does not modify the contents of
 * brokentime.
 * @note Note that mktime is the inverse of the localtime_r function and it does
 * the same thing as the timelocal function.
 * @warning Regarding thread safety, the man page says: "Calling mktime() also
 * sets the external variable tzname with information about the current
 * timezone.". I THINK mktime sets the tzname variable using tzset, which, in
 * GNU libc is implemented with a lock.
 * This function converts a Local struct tm to a UTC time_t.
 */
time_t mktime (struct tm *brokentime);

/**
 * 3.3. timegm()
 * The timegm function converts a UTC broken-down time structure(struct tm) to a
 * UTC simple time representation(time_t).
 * A call to this function automatically adjusts the values of the members of
 * brokentime if they are off-range or (in the case of tm_wday and tm_yday) if
 * they have values that do not match the date described by the other members.
 * If the specified broken-down time cannot be represented as a simple time,
 * timegm returns a value of (time_t)(-1) and does not modify the contents of
 * brokentime.
 * @note Note that timegm is the inverse of the gmtime_r function.
 * @note The timegm function does not exist on windows, and, there, you should
 * use the _mkgmtime function defined as: time_t _mkgmtime( struct tm* timeptr );
 * at MSDN: https://msdn.microsoft.com/en-us/library/2093ets1.aspx
 */
time_t timegm (struct tm *brokentime);

/**
 * 3.4. localtime_r()
 * The localtime_r function converts the simple time pointed to by time to
 * broken-down time representation, expressed relative to the user’s specified
 * time zone.
 * The result is placed in the object of type struct tm  to which the parameter
 * resultp points.
 * The return value is the null pointer if time cannot be represented as a
 * broken-down time(struct tm); typically this is because the year cannot fit
 * into an int.
 * @note Note that localtime_r is the inverse of the mktime function.
 * This function converts a UTC time_t to a Local struct tm.
 * @note Note that localtime_r is not available on windows, and you should use
 * localtime_s that is defined as: errno_t localtime_s( struct tm* _tm,
 * const time_t *time ); at MSDN: https://docs.microsoft.com/en-us/cpp/c-runtime-library/reference/localtime-s-localtime32-s-localtime64-s?redirectedfrom=MSDN&view=vs-2019
 * @note In case of function localtime , the return value is a pointer to a
 * static broken-down time structure, which might be overwritten by subsequent
 * calls to ctime , gmtime , or localtime . That’s why localtime() should never
 * be used in a multi-threaded application.
 */
struct tm * localtime_r (const time_t *time, struct tm *resultp);

/**
 * 3.5. gmtime_r()
 * This function is similar to localtime_r , except that the broken-down time is
 * expressed as Coordinated Universal Time (UTC) (formerly called Greenwich Mean
 * Time (GMT)) rather than relative to a local time zone.
 * This function converts a UTC time_t to a UTC struct tm.
 * @note Note that gmtime_r is not available on windows, and you should use
 * gmtime_s that is defined as: errno_t gmtime_s( struct tm* _tm,
 * const __time_t* time ); at MSDN: https://docs.microsoft.com/en-us/cpp/c-runtime-library/reference/gmtime-s-gmtime32-s-gmtime64-s?redirectedfrom=MSDN&view=vs-2019
 * @note In case of function gmtime , the return value is a pointer to a static
 * broken-down time structure, which might be overwritten by subsequent calls to
 * ctime , gmtime , or localtime . That’s why gmtime() should never be used in a
 * multi-threaded application.
 */
struct tm * gmtime_r (const time_t *time, struct tm *resultp);
```

Thanks to:

1. https://www.gnu.org/software/libc/manual/html_node/Simple-Calendar-Time.html
2. https://www.gnu.org/software/libc/manual/html_node/Broken_002ddown-Time.html
3. https://www.gnu.org/software/libc/manual/html_node/Time-Zone-Functions.html

---

Diagrams:
```txt
+-------------------------------------------------------------+
|                 |    UTC    |   UTC   |  Local  |   Local   |
|                 | struct tm |  time_t |  time_t | struct tm |
+-----------------+-----------+---------+---------+-----------+
| UTC struct tm   |    ---    | timegm  |   DGT   |     ?     |
+-----------------+-----------+---------+---------+-----------+
| UTC time_t      |  gmtime   |   ---   |   DGT   | localtime |
+-----------------+-----------+---------+---------+-----------+
| Local time_t    |    DGT    |   DGT   |   ---   |    DGT    |
+-----------------+-----------+---------+---------+-----------+
| Local struct tm |     ?     | mktime  |   DGT   |    ---    |
+-----------------+-----------+---------+---------+-----------+
DGT = Don't Go There - Local time_t does not make any sense.
      A time_t should always be interpreted as U.T.C.
 ?  = I don't know of a function that does that directly.
      Of course you can do that by composing functions.
```
<img alt="Diagram conversion functions" src="data:image/png;base64,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">
