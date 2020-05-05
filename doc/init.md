Sample init scripts and service configuration for securusd
==========================================================

Sample scripts and configuration files for systemd, Upstart and OpenRC
can be found in the contrib/init folder.

    contrib/init/securusd.service:    systemd service unit configuration
    contrib/init/securusd.openrc:     OpenRC compatible SysV style init script
    contrib/init/securusd.openrcconf: OpenRC conf.d file
    contrib/init/securusd.conf:       Upstart service configuration file
    contrib/init/securusd.init:       CentOS compatible SysV style init script

1. Service User
---------------------------------

All three startup configurations assume the existence of a "securus" user
and group.  They must be created before attempting to use these scripts.

2. Configuration
---------------------------------

At a bare minimum, securusd requires that the rpcpassword setting be set
when running as a daemon.  If the configuration file does not exist or this
setting is not set, securusd will shutdown promptly after startup.

This password does not have to be remembered or typed as it is mostly used
as a fixed token that securusd and client programs read from the configuration
file, however it is recommended that a strong and secure password be used
as this password is security critical to securing the wallet should the
wallet be enabled.

If securusd is run with "-daemon" flag, and no rpcpassword is set, it will
print a randomly generated suitable password to stderr.  You can also
generate one from the shell yourself like this:

bash -c 'tr -dc a-zA-Z0-9 < /dev/urandom | head -c32 && echo'

Once you have a password in hand, set rpcpassword= in /etc/securus/securus.conf

For an example configuration file that describes the configuration settings,
see contrib/debian/examples/securus.conf.

3. Paths
---------------------------------

All three configurations assume several paths that might need to be adjusted.

Binary:              /usr/bin/securusd
Configuration file:  /etc/securus/securus.conf
Data directory:      /var/lib/securusd
PID file:            /var/run/securusd/securusd.pid (OpenRC and Upstart)
                     /var/lib/securusd/securusd.pid (systemd)

The configuration file, PID directory (if applicable) and data directory
should all be owned by the securus user and group.  It is advised for security
reasons to make the configuration file and data directory only readable by the
securus user and group.  Access to securus-cli and other securusd rpc clients
can then be controlled by group membership.

4. Installing Service Configuration
-----------------------------------

4a) systemd

Installing this .service file consists on just copying it to
/usr/lib/systemd/system directory, followed by the command
"systemctl daemon-reload" in order to update running systemd configuration.

To test, run "systemctl start securusd" and to enable for system startup run
"systemctl enable securusd"

4b) OpenRC

Rename securusd.openrc to securusd and drop it in /etc/init.d.  Double
check ownership and permissions and make it executable.  Test it with
"/etc/init.d/securusd start" and configure it to run on startup with
"rc-update add securusd"

4c) Upstart (for Debian/Ubuntu based distributions)

Drop securusd.conf in /etc/init.  Test by running "service securusd start"
it will automatically start on reboot.

NOTE: This script is incompatible with CentOS 5 and Amazon Linux 2014 as they
use old versions of Upstart and do not supply the start-stop-daemon uitility.

4d) CentOS

Copy securusd.init to /etc/init.d/securusd. Test by running "service securusd start".

Using this script, you can adjust the path and flags to the securusd program by
setting the SecurusD and FLAGS environment variables in the file
/etc/sysconfig/securusd. You can also use the DAEMONOPTS environment variable here.

5. Auto-respawn
-----------------------------------

Auto respawning is currently only configured for Upstart and systemd.
Reasonable defaults have been chosen but YMMV.
