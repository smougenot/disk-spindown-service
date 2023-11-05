Role Name
=========

[![Cotinuous Integration](https://github.com/smougenot/disk-spindown-service/actions/workflows/ci.yml/badge.svg)](https://github.com/smougenot/disk-spindown-service/actions/workflows/ci.yml)

Goal is to spindown disk drives after a certain duration of standby. This saves energy.

Requirements
------------

None

Role Variables
--------------

See defaults/main.yml for details.

Most important is `script_options`. For available options see script usage below.

Dependencies
------------

Base on the work in
project [truenas-spindown-timer](https://github.com/ngandrass/truenas-spindown-timer/blob/master/README.md)

### Script usage

Please refer to original project for details/changes.

```bash
Usage:
  spindown_timer.sh [-h] [-q] [-v] [-d] [-m] [-u <MODE>] [-t <TIMEOUT>]
                    [-p <POLL_TIME>] [-i <DRIVE>] [-s <TIMEOUT>]

Monitors drive I/O and forces HDD spindown after a given idle period.
Resistant to S.M.A.R.T. reads.

Operation is supported on either drive level (MODE = disk) with plain device
identifiers or zpool level (MODE = zpool) with zfs pool names. See -u for more
information. A drive is considered idle and gets spun down if there has been no
I/O operations on it for at least TIMEOUT seconds. I/O requests are detected
within multiple intervals with a length of POLL_TIME seconds. Detected reads or
writes reset the drives timer back to TIMEOUT.

Options:
  -t TIMEOUT    : Total spindown delay. Number of seconds a drive has to
                  experience no I/O activity before it is spun down (default: 3600).
  -p POLL_TIME  : I/O poll interval. Number of seconds to wait for I/O during a
                  single monitoring period (default: 600).
  -s TIMEOUT    : Shutdown timeout. If given and no drive is active for TIMEOUT
                  seconds, the system will be shut down.
  -u MODE       : Operation mode (default: disk).
                  If set to 'disk', the script operates with disk identifiers
                  (e.g. ada0) for all CLI arguments and monitors I/O using
                  iostat directly.
                  If set to 'zpool' the script operates with ZFS pool names
                  (e.g. zfsdata) for all CLI arguments and monitors I/O using
                  the iostat of zpool.
  -i DRIVE      : In automatic drive detection mode (default):
                    Ignores the given drive or zfs pool.
                  In manual mode [-m]:
                    Only monitor the specified drives or zfs pools. Multiple
                    drives or zfs pools can be given by repeating the -i option.
  -m            : Manual drive detection mode. If set, automatic drive detection
                  is disabled.
                  CAUTION: This inverts the -i option, which can then be used to
                  manually supply drives or zfs pools to monitor. All other drives
                  or zfs pools will be ignored.
  -q            : Quiet mode. Outputs are suppressed set.
  -v            : Verbose mode. Prints additional information during execution.
  -d            : Dry run. No actual spindown is performed.
  -h            : Print this help message.

Example usage:
spindown_timer.sh
spindown_timer.sh -q -t 3600 -p 600 -i ada0 -i ada1
spindown_timer.sh -q -m -i ada6 -i ada7 -i da0
spindown_timer.sh -u zpool -i freenas-boot
```

Example Playbook
----------------

```yaml
    - hosts: servers
      roles:
      - { role: disks-spindown, tags: spindown }
```

License
-------

MIT

Author Information
------------------
