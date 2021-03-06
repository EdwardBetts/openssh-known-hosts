% UPDATE-OPENSSH-KNOWN-HOSTS(8)
% Timo Weingärtner <timo@tiwe.de>
% 2014-02-03

# NAME

update-openssh-knwon-hosts - download, filter and merge known_hosts for OpenSSH

# SYNOPSIS

*update-openssh-known-hosts* [*-f*]

# DESCRIPTION

update-openssh-known-hosts manages downloading, filtering and mergeing of ssh_known_hosts files from anywhere into one local file for use by ssh(1).

# OPTIONS

-f
:   treat every non-zero exit from download plugin as an error, see EXIT_IGNORE below.

# RETURN VALUES

Returns zero on success and anything else on error.

# ENVIRONMENT

CONFDIR
:   Configuration directory, defaults to /etc/openssh-known-hosts. Currently there is only a sources subdirectory in it.

PLUGIN_PATH
:   Plugin search path, defaults to /usr/local/share/openssh-known-hosts/plugins:/usr/share/openssh-known-hosts/plugins.

CACHEDIR
:   Cache directory, defaults to /var/cache/openssh-known-hosts.

LOCK
:   Lockfile path, defaults to /var/lock/openssh-known-hosts.

OUTFILE
:   Output file name, defaults to /var/lib/openssh-known-hosts/ssh_known_hosts

# SOURCE DEFINITIONS

A source definition is shell snippet dropped into CONFDIR/sources/ with a run-parts(8) compliant name.
There are two variables not specific to a download plugin:

PLUGIN
:   name of the download plugin to use, searched for in PLUGIN_PATH.

EXIT_IGNORE
:   optional space-seperated list of exitcodes which should be ignored. Upon such exit code the previously downloaded version is used.

# DOWNLOAD PLUGINS

Download plugins are executables dropped into PLUGIN_PATH and referenced via the PLUGIN variable in the source definition.
A plugin gets the variables set in the source definition in its environment.
The working directory will be set to the source's cache directory.
Everything a plugin has to do is to create a file named "new".
"current" must not be touched but can be used as a hint to skip downloading the same file again.
stdout and stderr will be connected to "log", which will be output on error.
Plugins needn't create "new" if it would be identical to "current".

# HOSTNAME FILTERS

Place a file foo.filter next to your source definition foo.
Each line shall contain a rule consisting of an action, a space and a pattern.
The first rule with a matching pattern decides: If the action starts with a, o, p or y (for accept, admit, allow, ok, pass, permit, print, yes, ...) the hostname will be used, otherwise it is discarded.
If a key has no hostnames left it is discarded as a whole.

# SEE ALSO

ssh(1), sshd(8), ssh_config(5), curl(1), rsync(1), psql(1), run-parts(8)
