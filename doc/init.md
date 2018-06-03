Sample init scripts and service configuration for aeriumxd
==========================================================

Sample scripts and configuration files for systemd, Upstart and OpenRC
can be found in the contrib/init folder.

    contrib/init/aeriumxd.service:    systemd service unit configuration
    contrib/init/aeriumxd.openrc:     OpenRC compatible SysV style init script
    contrib/init/aeriumxd.openrcconf: OpenRC conf.d file
    contrib/init/aeriumxd.conf:       Upstart service configuration file
    contrib/init/aeriumxd.init:       CentOS compatible SysV style init script

1. Service User
---------------------------------

All three startup configurations assume the existence of a "aeriumx" user
and group.  They must be created before attempting to use these scripts.

2. Configuration
---------------------------------

At a bare minimum, aeriumxd requires that the rpcpassword setting be set
when running as a daemon.  If the configuration file does not exist or this
setting is not set, aeriumxd will shutdown promptly after startup.

This password does not have to be remembered or typed as it is mostly used
as a fixed token that aeriumxd and client programs read from the configuration
file, however it is recommended that a strong and secure password be used
as this password is security critical to securing the wallet should the
wallet be enabled.

If aeriumxd is run with "-daemon" flag, and no rpcpassword is set, it will
print a randomly generated suitable password to stderr.  You can also
generate one from the shell yourself like this:

bash -c 'tr -dc a-zA-Z0-9 < /dev/urandom | head -c32 && echo'

Once you have a password in hand, set rpcpassword= in /etc/aeriumx/aeriumx.conf

For an example configuration file that describes the configuration settings,
see contrib/debian/examples/aeriumx.conf.

3. Paths
---------------------------------

All three configurations assume several paths that might need to be adjusted.

Binary:              /usr/bin/aeriumxd
Configuration file:  /etc/aeriumx/aeriumx.conf
Data directory:      /var/lib/aeriumxd
PID file:            /var/run/aeriumxd/aeriumxd.pid (OpenRC and Upstart)
                     /var/lib/aeriumxd/aeriumxd.pid (systemd)

The configuration file, PID directory (if applicable) and data directory
should all be owned by the aeriumx user and group.  It is advised for security
reasons to make the configuration file and data directory only readable by the
aeriumx user and group.  Access to aeriumx-cli and other aeriumxd rpc clients
can then be controlled by group membership.

4. Installing Service Configuration
-----------------------------------

4a) systemd

Installing this .service file consists on just copying it to
/usr/lib/systemd/system directory, followed by the command
"systemctl daemon-reload" in order to update running systemd configuration.

To test, run "systemctl start aeriumxd" and to enable for system startup run
"systemctl enable aeriumxd"

4b) OpenRC

Rename aeriumxd.openrc to aeriumxd and drop it in /etc/init.d.  Double
check ownership and permissions and make it executable.  Test it with
"/etc/init.d/aeriumxd start" and configure it to run on startup with
"rc-update add aeriumxd"

4c) Upstart (for Debian/Ubuntu based distributions)

Drop aeriumxd.conf in /etc/init.  Test by running "service aeriumxd start"
it will automatically start on reboot.

NOTE: This script is incompatible with CentOS 5 and Amazon Linux 2014 as they
use old versions of Upstart and do not supply the start-stop-daemon uitility.

4d) CentOS

Copy aeriumxd.init to /etc/init.d/aeriumxd. Test by running "service aeriumxd start".

Using this script, you can adjust the path and flags to the aeriumxd program by
setting the AeriumXD and FLAGS environment variables in the file
/etc/sysconfig/aeriumxd. You can also use the DAEMONOPTS environment variable here.

5. Auto-respawn
-----------------------------------

Auto respawning is currently only configured for Upstart and systemd.
Reasonable defaults have been chosen but YMMV.
