###############################################################################
 Linux Tools
###############################################################################

- `Search`_

    - `File timestamps`_
    - `Find`_
    - `Grep`_

- `Network`_

    - `Deprecated commands`_
    - `SS`_
    - `IPtables`_
    - `Fail2Ban`_
    - `Other network utils`_

- `System`_

    - `Manage Users`_
    - `Manage Packages`_
    - `Aptitude`_
    - `Hold package versions`_
    - `Systemctl & Service`_
    - `SSH Keys`_
    - `Enable Hibernate`_
    - `Snap`_
    - `Kernel parameters`_

- `Utils`_

    - `Logrotate`_
    - `TAR`_
    - `Other utils`_

- `Drives`_

    - `GRUB`_
    - `RAID`_
    - `SMART`_
    - `Mount partitions`_
    - `Fstab`_
    - `Swap File`_



===============================================================================
 Search
===============================================================================

File timestamps
-------------------------------------------------------------------------------

atime
    File Access Time - the last time the data from a file was accessed - read
    by one of the Unix processes directly or through commands and scripts.

ctime
    File Change Time - changes when file's ownership or access permissions is
    changed. It will also naturally highlight the last time file had its
    contents updated.

mtime
    File Modify Time - shows time of the last change to file's contents. It
    does not change with owner or permission changes, and is therefore used for
    tracking the actual changes to data of the file itself.


Find
-------------------------------------------------------------------------------

- Security considerations:
  https://www.gnu.org/software/findutils/manual/html_node/find_html/Security-Considerations-for-find.html
- Deleting files:
  https://www.gnu.org/software/findutils/manual/html_node/find_html/Deleting-Files.html

Numeric arguments can be specified as:

- ``+n`` for greater than n
- ``-n`` for less than n
- ``n`` for exactly n

``-not <expr>``, ``! <expr>``
    True if ``expr`` is false.

``<expr1> -and <expr2>``, ``<expr1> -a <expr2>``, ``<expr1> <expr2>``
    AND - two expressions in a row are taken to be joined; ``expr2`` is not
    evaluated if ``expr1`` is false.

``<expr1> -or <expr2>``, ``<expr1> -o <expr2>``
    OR - ``expr2`` is not evaluated if ``expr1`` is true.

``-delete``
    Delete files; true if removal succeeded. If the removal failed, an error
    message is issued. If ``-delete`` fails, find's exit status will be nonzero
    (when it eventually exits). Use of ``-delete`` automatically turns on the
    ``-depth`` option.

``-depth``
    Process each directory's contents before the directory itself.

``-maxdepth <levels>``
    Descend at most levels (a non-negative integer) levels of directories below
    the starting-points. ``-maxdepth 0`` means only apply the tests and actions
    to the starting-points themselves.

``-print0``
    print the full file name on the standard output, followed by a null
    character (instead of the newline character that ``-print``  uses). This
    allows file names that contain newlines or other types of white space to be
    correctly interpreted by programs that process the find output.  This
    option corresponds to the ``-0`` option of ``xargs``.

``-amin <n>``
    File was last accessed ``n`` minutes ago.

``-atime <n>``
    File was last accessed ``n*24`` hours ago.  When ``find`` figures out how
    many 24-hour periods ago the file was last accessed, any fractional part is
    ignored, so to match ``-atime +1``, a file has to have been accessed at
    least two days ago.

``-cmin <n>``
    File's status was last changed n minutes ago.

``-ctime <n>``
    File's status was last changed ``n*24`` hours ago.  When ``find`` figures out
    how many 24-hour periods ago the file was last changed, any fractional part
    is ignored, so to match ``-ctime +1``, a file has to have been changed at
    least two days ago.

``-mmin <n>``
    File's data was last modified ``n`` minutes ago.

``-mtime <n>``
    File's data was last modified ``n*24`` hours ago. The time since each file
    was last modified is divided by 24 hours and any remainder is discarded. To
    match ``-mtime 0``, a file will have to have a modification in the past
    which is less than 24 hours ago.

    .. code-block:: shell

        find . -mtime +0    # Find files modified greater than 24 hours ago.
        find . -mtime 0     # Files modified between now and 1 day ago,
                            # in the past 24 hours only.
        find . -mtime -1    # Modified less than 1 day ago (same as "-mtime 0").
        find . -mtime 1     # Modified between 24 and 48 hours ago.
        find . -mtime +1    # Modified more than 48 hours ago.

``-newerXY <reference>``
    Succeeds if timestamp X of the file being considered is newer than
    timestamp Y of the file reference. The letters X and Y can be any of the
    following letters:

    === =======================================================================
    a   The access time of the file reference.
    B   The birth time of the file reference.
    c   The inode status change time of reference.
    m   The modification time of the file reference.
    t   reference is interpreted directly as a time.
    === =======================================================================

    Some combinations are invalid; for example, it is invalid for X to be ``t``.
    Some combinations are not implemented on all systems; for example
    ``B`` is not supported on all systems. If an invalid or unsupported
    combination of XY is specified, a fatal error results.

    Time specifications are interpreted as for the argument to the ``-d``
    option of GNU ``date``. If you try to use the birth time of a reference
    file, and the birth time cannot be determined, a fatal error message
    results. If you specify a test which refers to the birth time of files
    being examined, this test will fail for any files where the birth time is
    unknown.

``-empty``
    File is empty and is either a regular file or a directory.

``-user <uname>``
    File is owned by user ``uname`` (numeric user ID allowed).

``-group <gname>``
    File belongs to group ``gname`` (numeric group ID allowed).

``-perm <mode>``
    File's permission bits are exactly mode (octal or symbolic). Since an exact
    match is required, if you want to use this form for symbolic modes, you may
    have to specify a rather complex mode string. For example ``-perm g=w``
    will only match files which have mode 0020. It is more likely that you
    will want to use the ``/`` or ``-`` forms, for example ``-perm -g=w``,
    which matches any file with group write permission.

``-perm -<mode>``
    All of the permission bits mode are set for the file. Symbolic modes are
    accepted in this form, and this is usually the way in which you would want
    to use them. You must specify ``u``, ``g`` or ``o`` if you use a symbolic
    mode.

``-perm /<mode>``
    Any of the permission bits mode are set for the file. Symbolic modes are
    accepted in this form. You must specify ``u``, ``g`` or ``o`` if you use
    a symbolic mode. If no permission bits in mode are set, this test matches
    any file (the idea here is to be consistent with the behaviour
    of ``-perm -000``).


``-exec command ;``
    The specified command is **run once for each** matched file.

    **Note:** There are unavoidable security problems surrounding use of the
    ``-exec`` action; you should use the ``-execdir`` option instead.

    All following arguments to ``find`` are taken to be arguments to the command
    until an argument consisting of ``;`` is encountered. The string ``{}`` is
    replaced by the current file name being processed everywhere it occurs in
    the arguments to the command, not just in arguments where it is alone, as
    in some versions of ``find``. Both of these constructions might need to be
    escaped (``\;``) or quoted (``"{}"``) to protect them from expansion by the
    shell.

    The command is executed in the starting directory.

    true if 0 status is returned.

``-exec command {} +``
    This variant of the exec action runs the specified command on the
    selected files, but the command line is built **by appending each**
    selected file name at the end; the total number of invocations of the
    command will be much less than the number of matched files.

    The command line is built in much the same way that ``xargs`` builds its
    command lines. Only one instance of ``{}`` is allowed within the command.

    The command is executed in the starting directory.

    If ``find`` encounters an error, this can sometimes cause an immediate exit, so
    some pending commands may not be run at all. This variant of ``-exec``
    always returns true.

``-execdir command ;`` (``-execdir command {} +``)
    Like ``-exec``, but the specified command is **run from the subdirectory
    containing the matched file**, which is not normally the directory in which
    you started ``find``.

    **Note:** This a much more secure method for invoking commands, as it
    avoids race conditions during resolution of the paths to the matched files.

    As with the ``-exec`` action, the ``+`` form of ``-execdir`` will build
    a command line to process more than one matched file, but any given
    invocation of command will only list files that exist in the same
    subdirectory.

    If you use this option, you must ensure that your ``$PATH`` environment
    variable does not reference ``.``; otherwise, an attacker can run any
    commands they like by leaving an appropriately-named file in a directory in
    which you will run ``-execdir``. The same applies to having entries in
    ``$PATH`` which are empty or which are not absolute directory names.

    If ``find`` encounters an error, this can sometimes cause an immediate exit, so
    some pending commands may not be run at all. The result of the action
    depends on whether the ``+`` or the ``;`` variant is being used; ``-execdir
    command {} +`` always returns true, while ``-execdir command {} ;`` returns
    true only if command returns 0.


**Examples**

Remove all cache dirs::

    $ find . -type d -name "__pycache__" -exec rm -rv "{}" +

Set permissions on all dirs/files::

    $ find . -type d -print0 | xargs -0 chmod 755
    $ find . -type f -print0 | xargs -0 chmod 644

Delete files which have been modified in the last 24h::

    $ find . -type f -name "*.sql.gz" -mtime 0 -delete
    $ find . -type f -name "*.sql.gz" -mtime -1 -delete

Find modified files in date range::

    $ find . -type f -newermt 2013-07-27 -not -newermt 2013-07-28
    $ find . -type f -newermt 2013-07-27 ! -newermt 2013-07-28

Search for files which have read/write permission by owner **and** group
**and** read by others, without regard to the presence of any extra permission
bits (for example the executable bit). This will match a file which has mode
0777::

    $ find . -perm -664

Same as above example, but **exact match** only. Files which meet these
criteria but have other permissions bits set (for example if someone can
execute the file) will not be matched::

    $ find . -perm 664

Search for files which are writable **by both** - their owner **and** their
group::

    $ find . -perm -220
    $ find . -perm -g+w,u+w

Search for files which are writable **by either** - their owner **or** their
group.  The files don't have to be writable by both to be matched, either will
do::

    $ find . -perm /220
    $ find . -perm /u+w,g+w
    $ find . -perm /u=w,g=w

To run a single command for each file found::

    $ find some/path/ ... -exec some_command "{}" \;

To run a single command on multiple files at once::

    $ find some/path/ ... -exec some_command "{}" +

To run multiple commands in sequence for each file found, where the second
command should only be run if the first command succeeds::

    $ find some/path/ ... -exec some_cmd "{}" \; -exec other_cmd "{}" \;


Grep
-------------------------------------------------------------------------------

``egrep``, ``fgrep`` and ``rgrep`` are **deprecated** and are the same as
``grep -E``,  ``grep -F``,  and  ``grep -r``.

Syntax::

    $ grep [OPTIONS] PATTERN [FILE...]

Matcher Selection:

-E, --extended-regexp
    Interpret pattern as an extended regular expression (ERE, see below).

-F, --fixed-strings
    Interpret pattern as a list of fixed strings (instead of regular
    expressions), separated by newlines, any of which is to be matched.

Matching Control:

-r, --recursive
    Read all files under each directory, recursively,  following  symbolic  links
    only if they are on the command line.  Note that if no file operand is given,
    grep searches the working directory.  This is equivalent to  the  -d  recurse
    option.
-i, --ignore-case
    Ignore case distinctions in both the PATTERN and the input files.
-o, --only-matching
    Print only the matched (non-empty) parts of a matching line, with  each  such
    part on a separate output line.
-s, --no-messages
    Suppress error messages about nonexistent or unreadable files.
-I  Process a binary file as if it did not contain matching data; this is equivalent
    to the --binary-files=without-match option.
-l, --files-with-matches
    Suppress normal output; instead print the name of each input file from  which
    output would normally have been printed. The scanning will stop on the first
    match.
-n, --line-number
    Prefix each line of output with the 1-based line number within its input file.

Basic vs Extended regular expressions:

    In basic regular expressions the meta-characters ``?``, ``+``, ``{}``,
    ``|``, ``()`` lose their special meaning; instead use the backslashed
    versions ``\?``, ``\+``, ``\{\}``, ``\|``, ``\(\)``.

Example of some differnce between basic, extended (``-E``) regexps & ``-e``
flag::

    $ grep 'aaa\|bbb\|ccc' some_file.txt
    $ grep -E 'aaa|bbb|ccc' some_file.txt
    $ grep -e 'aaa' -e 'bbb' -e 'ccc' some_file.txt


Find pattern in files content::

    $ grep -EriIns "some patten" /some/path
    $ grep -FriIns "some string" /some/path



===============================================================================
 Network
===============================================================================

Deprecated commands
-------------------------------------------------------------------------------

https://dougvitale.wordpress.com/2011/12/21/deprecated-linux-networking-commands-and-their-replacements/

==================== ==========================================================
Deprecated command   Replacement command(s)
-------------------- ----------------------------------------------------------
arp                  ip n (ip neighbor)
ifconfig             ip a (ip addr), ip link, ip -s (ip -stats)
iptunnel             ip tunnel
iwconfig             iw
nameif               ip link, ifrename
netstat              ss, ip route (for netstat-r), ip -s link (for netstat -i),
                     ip maddr (for netstat-g)
route                ip r (ip route)
==================== ==========================================================

Show / manipulate routing, devices, policy routing and tunnels. Show host’s
network stack on the host::

    $ ip addr show
    $ ip a



SS
-------------------------------------------------------------------------------
Some examples: http://www.binarytides.com/linux-ss-command/

Show all TCP connection::

    $ ss -tnp | column -t

TCP listen::

    $ ss -tnpel | column -t
    $ watch -n 1 "ss -tnpel | column -t"

UDP all::

    $ ss -unpea | column -t

TCP/UDP all::

    $ ss -tunpea | column -t

All 80/443 connections::

    $ ss -tnp | egrep 'ESTAB.*:(80|443)' | column -t
    $ watch -n 1 "ss -tnp | egrep 'ESTAB.*:(80|443)' | column -t"



IPtables
-------------------------------------------------------------------------------

List all rules in all chains::

    $ sudo iptables -L -n

To attempt to delete every non-builtin (a user-defined) chain::

    $ sudo iptables -X

Delete all rules in all chains::

    $ sudo iptables -F

Show rule by number::

    $ sudo iptables -L -n --line-numbers
    $ sudo iptables -S INPUT <RULE_NUM>

Replce rule by number::

    $ sudo iptables -R INPUT <RULE_NUM> -p tcp --syn --dport 80 -m connlimit --connlimit-above 50 -j REJECT

Restrict the number of parallel connections to a server per client IP address
or client address block::

    $ sudo iptables -A INPUT -p tcp --syn --dport 25 -m connlimit --connlimit-above 5 -j REJECT

Drop all incoming from IP::

    $ sudo iptables -A INPUT -s 11.22.33.1 -j DROP

or from host::

    $ sudo iptables -A INPUT -s test.host.jp -j DROP

or from subnet::

    $ sudo iptables -A INPUT -s 11.22.33.0/24 -j DROP

Drop all incoming to 25 port & allow from only one IP::

    $ sudo iptables -A INPUT -p tcp --dport 25 -j DROP
    $ sudo iptables -A INPUT -s 11.22.33.1 -p tcp --dport 25 -j ACCEPT

Save rules::

    $ sudo iptables-save > /etc/iptables-rules.conf

Apply and ask if all ok::

    $ sudo iptables-apply /etc/iptables-rules.conf

Flush & restore::

    $ sudo iptables-restore < /etc/iptables-rules.conf

Add ``-n`` to not overwrite the previously written rules in the tables::

    $ sudo iptables-restore -n < /etc/iptables-rules.conf

Create script ``/etc/network/if-pre-up.d/iptables-rules-restore``:

.. code-block:: shell

    #!/bin/sh
    # -n tells to not flush previously written rules in the tables
    iptables-restore -n < /etc/iptables-rules.conf
    exit 0

Give script execute permissions::

    $ sudo chmod +x /etc/network/if-pre-up.d/iptables-rules-restore

List all available modules of iptables::

    $ ls /lib/modules/`uname -r`/kernel/net/netfilter/

List of extensions in the standard iptables distribution:

- http://ipset.netfilter.org/iptables-extensions.man.html
- http://manpages.ubuntu.com/manpages/xenial/man8/iptables-extensions.8.html



Fail2Ban
-------------------------------------------------------------------------------

Test regular expressions for fail2ban::

    $ fail2ban-regex [OPTIONS] <LOG> <REGEX> [IGNOREREGEX]
    $ fail2ban-regex /var/log/auth.log /etc/fail2ban/filter.d/sshd.conf
    $ fail2ban-regex /var/log/auth.log "Failed [-/\w]+ for .* from <HOST>"

Use ``-v`` for verbose output::

    $ fail2ban-regex -v /var/log/mail.log /etc/fail2ban/filter.d/smtp.conf



Other network utils
-------------------------------------------------------------------------------

iptraf
    Interactive Colorful IP LAN Monitor.

ifstat
    Report InterFace STATistics. ::

        $ ifstat -zntS

iftop
    Display bandwidth usage on an interface by host.

Test network::

    $ wget cachefly.cachefly.net/100mb.test -O /dev/null




===============================================================================
 System
===============================================================================

Manage Users
-------------------------------------------------------------------------------

Show current user::

    $ whoami

Show all users in system::

    $ getent passwd
    $ compgen -u

Show all groups in system::

    $ getent group
    $ compgen -g

Show what groups user is in::

    $ groups <user_name>

Add user to group::

    $ usermod -aG sudo <user_name>
    $ usermod -aG docker $(whoami)

Remove user from named group::

    $ sudo gpasswd -d <user_name> <group_name>



Manage Packages
-------------------------------------------------------------------------------

Install `.deb` package::

    $ sudo apt install /tmp/docker.deb

    # or alternative method:
    sudo dpkg --install /tmp/docker.deb
    sudo apt-get install -f

List all files installed to your system by some package::

    $ dpkg --listfiles <package_name>

Search installed package::

    $ dpkg --get-selections | grep <package_name>

List packages matching given pattern. If no package-name-pattern is given, list
all packages in /var/lib/dpkg/status, excluding the ones marked as
not-installed (i.e. those which have been previously purged). Normal shell
wildcard characters are allowed::

    $ dpkg --list
    $ dpkg --list "package-name-pattern"

The first three columns of the output show the desired **action**, the package
**status**, and **errors**, in that order.

Desired action:

======= ===================
u       Unknown
i       Install
h       Hold
r       Remove
p       Purge
======= ===================

Package status:

======= ===================
n       Not-installed
c       Config-files
H       Half-installed
U       Unpacked
F       Half-configured
W       Triggers-awaiting
t       Triggers-pending
i       Installed
======= ===================

Error flags:

======= ===================
<empty> (none)
R       Reinst-required
======= ===================

Frorce to remove package, but leave dependencies::

    $ sudo dpkg --remove --force-all <package_name>

Reconfigure an already installed package, It will ask configuration questions,
much like when the package was first installed::

    $ sudo dpkg-reconfigure <PACKAGE_NAME>

Upgrade one package::

    $ apt-get -sV --only-upgrade install <package_name>

Show dependencies of package::

    $ apt-cache depends <package_name>

Add an external APT repository to either ``/etc/apt/sources.list`` or a file in
``/etc/apt/sources.list.d/`` or removes an already existing repository::

    $ sudo add-apt-repository ppa:some_ppa/ppa
    $ sudo add-apt-repository --remove ppa:some_ppa/ppa

Removing a PPA means not only to disable the PPA, but also to downgrade any
packages you've upgraded from that PPA, to the version available in the
official Ubuntu repositories.

To find out the PPA to which a package belongs to::

    $ apt-cache policy <package_name>



Aptitude
-------------------------------------------------------------------------------

Options:

-P, --prompt          Always prompt for confirmation or actions
-D, --show-deps       Show the dependencies of automatically changed packages.
-V, --show-versions   Show which versions of packages will be installed.
-v, --verbose         Display extra information.
-s, --simulate        Simulate actions, but do not actually perform them.

List Legend:

=== ===========================================================================
i   Installed package
c   Package not installed, but package configuration remains on system
p   Purged from system
v   Virtual package
B   Broken package
u   Unpacked files, but package not yet configured
C   Half-configured - Configuration failed and requires fix
H   Half-installed - Removal failed and requires fix
=== ===========================================================================

Simulate install::

    $ sudo aptitude install -sPDVv <package_name>

Upgrade installed packages to their most recent version. Installed packages
will not be removed unless they are unused. Packages which are not currently
installed may be installed to resolve dependencies unless the
``--no-new-installs`` command-line option is supplied::

    $ sudo aptitude update
    $ sudo aptitude safe-upgrade -PDV

Remove old kernels::

    $ dpkg --list "*linux-*"
    $ sudo aptitude purge -PDVv linux-image-4.4.0-{31,34,36..38}-generic
    $ sudo aptitude purge -PDVv linux-image-extra-4.4.0-{31,34,36..38}-generic



Hold package versions
-------------------------------------------------------------------------------

Using dpkg, put a package on hold::

    $ echo "package hold" | sudo dpkg --set-selections

Remove the hold::

    $ echo "package install" | sudo dpkg --set-selections

Show the status of packages::

    $ dpkg --get-selections
    $ dpkg --get-selections | grep <package_name>

Using apt::

    $ sudo apt-mark hold <package_name>
    $ sudo apt-mark unhold <package_name>

Using aptitude::

    $ sudo aptitude hold <package_name>
    $ sudo aptitude unhold <package_name>



Systemctl & Service
-------------------------------------------------------------------------------

The ``service`` command is a wrapper script that allows system administrators
to start, stop and check the status of services without worrying too much
about the actual init system being used::

    $ service --status-all  # All services
    $ service nginx         # Show usage keywords

``systemctl`` control the systemd system and service manager::

    $ systemctl stop dovecot.socket
    $ systemctl mask dovecot.socket
    $ systemctl enable dovecot.service
    $ systemctl start dovecot.service
    $ systemctl status dovecot.service

Restarts a service only if it is running::

    $ systemctl try-restart name.service

Reloads configuration if it's possible::

    $ systemctl reload name.service

Try to reload but if it's not possible restarts the service::

    $ systemctl reload-or-restart name.service

To find out about a service status::

    $ systemctl status name.service
    $ systemctl is-active name.service      # running
    $ systemctl is-enabled name.service     # will be activated when booting
    $ systemctl is-failed name.service      # failed to load

Mask or unmask a service::

    $ systemctl mask name.service
    $ systemctl unmask name.service

Wen you mask a service it will be linked to /dev/null, so manually or
automatically other services can't active/enable it. (you should unmask it
first).

List timer units currently in memory, ``NEXT`` shows the next time the timer
will run, ``ACTIVATES`` shows the name the service the timer activates when it
runs::

    $ systemctl list-timers



SSH Keys
-------------------------------------------------------------------------------

Generate keys::

    $ ssh-keygen -t rsa -b 4096 -f ~/.ssh/id_rsa

Add key to ``~/.ssh/config``::

    Host short-alias-name
        HostName                    full.server.name
        Port                        22
        User                        username-on-remote-machine
        PreferredAuthentications    publickey
        IdentityFile                ~/.ssh/id_rsa

Transfer ``key_file.pub`` to target remote host.

Specify the identity file for connection::

    $ ssh -i ~/.ssh/private-key-file some-user@some.server.name

To use only the authentication identity and certificate files explicitly
configured in the ssh config files or passed on the ssh command-line set option
in ``/etc/ssh/ssh_config``::

    IdentitiesOnly yes



Enable Hibernate
-------------------------------------------------------------------------------

Enable hibernate in menu::

    $ sudo vim /var/lib/polkit-1/localauthority/10-vendor.d/com.ubuntu.desktop.pkla

    [Disable hibernate by default in upower]
    ResultActive=yes

    [Disable hibernate by default in logind]
    ResultActive=yes

Append ``resume=`` with swap partition UUID to the grub (``/etc/default/grub``)
and update grub::

    $ sudo vim /etc/default/grub
    # Edit line to:
    GRUB_CMDLINE_LINUX_DEFAULT="quiet splash resume=UUID=97e78c77-5ba9-7207-9fe1-c7f585d3efd7"
    $ sudo update-grub



Snap
-------------------------------------------------------------------------------

$ snap info pycharm-professional
$ sudo snap refresh pycharm-professional --channel=latest/stable --classic
$ sudo snap refresh pycharm-professional --channel=2019.2/stable --classic



Kernel parameters
-------------------------------------------------------------------------------

``sysctl`` is used to modify kernel parameters at runtime. The parameters available
are those listed under ``/proc/sys/``. We can use ``sysctl`` to both read
and write sysctl data.

At boot, ``systemd-sysctl.service`` reads configuration files from the ``/etc/sysctl.d``
to configure ``sysctl`` kernel parameters.

To see current values of params::

    $ sudo sysctl fs.inotify.max_user_watches

To apply changes::

    $ sudo sysctl -p --system



===============================================================================
 Utils
===============================================================================

Logrotate
-------------------------------------------------------------------------------

-v  Turn on verbose mode.
-d  Turns on debug mode and implies ``-v``. In debug mode, no changes will
    be made to the logs or to the logrotate state file.
-f  Tells logrotate to force the rotation, even if it doesn’t think
    this is necessary.

Test in debug mode::

    $ logrotate -fd /etc/logrotate.conf

Force rotate::

    $ logrotate -vf /etc/logrotate.conf



TAR
-------------------------------------------------------------------------------

-t, --list                      List the contents of an archive
-c, --create                    Create a new archive
-x, --extract, --get            Extract files from an archive
-C, --directory                 Change to directory DIR
-f, --file ARCHIVE              Use archive file or device ARCHIVE
-p, --same-permissions, --preserve-permissions
    Extract information about file permissions
-P, --absolute-names            Don't strip leading ``/`` from file names
--exclude=PATTERN               Exclude files, given as a PATTERN
--strip-components=NUMBER
    Strip NUMBER leading components from file names on extraction
-d, --diff, --compare           Find differences between archive and file system
-r, --append                    Append files to the end of an archive
-u, --update                    Only append files newer than copy in archive
-z, --gzip, --gunzip, --ungzip  Filter the archive through gzip
-v, --verbose                   Verbosely list files processed

List all files in archive verbosely::

    $ tar tvf /path/to/archive.tar

Add to archive::

    $ tar cpf archive.tar some/path/dir
    $ tar cpfz archive.tar.gz some/path/dir

Add files to archive without all path dirs::

    $ tar cpfz archive.tar.gz -C some/path/ dir
    $ tar cpfz archive.tar.gz -C some/path/dir .

Add files excpet some specific dirs::

    $ tar cpf arch.tar --exclude='dir/one/*' --exclude='dir/two/*' -C some/path/ dir

Extract files to a specific directory::

    $ tar xpf archive.tar -C some/path/
    $ tar xpf archive.tar.gz -C some/path/

Extact without first two levels of structure::

    $ tar --strip-components 2 -xpf archive.tar.gz -C some/path/



Other utils
-------------------------------------------------------------------------------

System info::

    $ inxi -F




===============================================================================
 Drives
===============================================================================

Filesystem space & inodes usage::

    $ df -h
    $ df -i

Lists information about all available or the specified block devices::

    $ lsblk

Show plugged disks info::

    $ sudo lshw -C disk

Show disks soft info::

    $ dmesg | grep sd

Show devices UUID::

    $ sudo blkid

``Parted`` is a partition manipulation program. To lists partition layout on
all block devices::

    $ sudo parted -l

``sfdisk`` display or manipulate a disk partition table. List the partitions of
all devices::

    $ sudo sfdisk -l

``gdisk`` (GPT fdisk) provides complete diagnostic of partition table type::

    $ sudo gdisk -l /dev/sda

Disk benchmark::

    $ sudo hdparm -tT /dev/sda

Wipe disk::

    $ sudo dd if=/dev/zero of=/dev/sda1 bs=1M
    $ sudo dd if=/dev/urandom of=/dev/sdX bs=512



GRUB
-------------------------------------------------------------------------------

The bootloader goes to the MBR of the disks, not to partitions. And since you
are running software raid which the OS creates, the bootloader would go to both
disks to the MBR (``/dev/sda`` and ``/dev/sdb``). If it was fakeraid or
hardware RAID, it would go onto the RAID device because the RAID device is the
whole disk anyway.

With software RAID, the parts of the RAID are partitions, and the bootloader
doesn't go to partitions (with some special exceptions).

If the system partitions are on a software RAID install GRUB2 on all disks
in the RAID::

    $ grub-install /dev/sda
    $ grub-install /dev/sdb

Show where grub installed::

    $ grub-probe -t device /boot/grub

Update grub after changes in ``/etc/default/grub``::

    $ sudo update-grub



RAID
-------------------------------------------------------------------------------

View the status of an array::

    $ sudo mdadm --detail /dev/md0

View the status of a disk in an array::

    $ sudo mdadm --examine /dev/sda1

If a disk fails and needs to be replaced:

.. code-block:: shell

    # mark subsequent devices a faulty
    $ sudo mdadm /dev/md0 --fail /dev/sda1

    # remove subsequent devices, which must not be active
    $ sudo mdadm /dev/md0 --remove /dev/sda1

    # hotadd subsequent devices to the array
    $ sudo mdadm /dev/md0 --add /dev/sda1

After the drive has been replaced and synced, grub will need to be installed::

    $ sudo grub-install /dev/md1

Sometimes a disk can change to a faulty state even though there is nothing
physically wrong with the drive. It is usually worthwhile to remove the drive
from the array then re-add it. This will cause the drive to re-sync with the
array. If the drive will not sync with the array, it is a good indication of
hardware failure.

The ``/proc/mdstat`` file contains useful information about the system's RAID
devices::

    $ cat /proc/mdstat

Watch the status of a syncing drive (Ctrl+c to stop)::

    $ watch -n1 cat /proc/mdstat



SMART
-------------------------------------------------------------------------------

Show device SMART health status::

    $ sudo smartctl -H /dev/sda

Display detailed SMART information for drive::

    $ sudo smartctl -a /dev/sda

View a drive's info::

    $ sudo smartctl -i /dev/sda

Run tests (the most useful is "long")::

    $ sudo smartctl -t short /dev/sda
    $ sudo smartctl -t conveyance /dev/sda
    $ sudo smartctl -t long /dev/sda

Test HDD for bad sectors::

    $ badblocks -v /dev/sdc > /tmp/bad-sdc.txt



Mount partitions
-------------------------------------------------------------------------------

View the system's physical information::

    $ sudo fdisk -l
    $ sudo sfdisk -l
    $ sudo parted -l

Show UUIDs::

    $ sudo blkid

Show all mounts::

    mount

View configuration file ``/etc/fstab``::

    $ cat /etc/fstab



Fstab
-------------------------------------------------------------------------------

https://help.ubuntu.com/community/Fstab

The configuration file /etc/fstab contains the necessary information to
automate the process of mounting partitions.

- Options for 'mount' and 'fstab' are similar.
- Partitions listed in fstab can be configured to automatically mount during
  the boot process.
- If a device/partition is not listed in fstab **only root** may mount the
  device/partition.
- Users may mount a device/partition if the device is in fstab with the proper
  options.

Syntax of a 'fstab' entry::

    [Device] [Mount Point] [File System Type] [Options] [Dump] [Pass]

[Device]
    The device/partition (by /dev location or UUID) that contain a file system.
    By default, Ubuntu now uses UUID to identify partitions::

        UUID=xxx.yyy.zzz

    Alternative ways to refer to partitions::

        LABEL=label
        Samba: //server/share
        NFS: server:/share
        SSHFS: sshfs#user@server:/share
        Device: /dev/sdxy (not recommended)

[Mount Point]
    The directory on your root file system (aka mount point) from which it will
    be possible to access the content of the device/partition (note: swap has
    no mount point). Mount points should not have spaces in the names.

    A mount point is a location on your directory tree to mount the partition.
    The default location is /media although you may use alternate locations
    such as /mnt or your home directory. You may use any name you wish for the
    mount point, but you must create the mount point before you mount the
    partition.

[File System Type]
    You may either use auto or specify a file system. Auto will attempt to
    automatically detect the file system of the target file system and in
    general works well. In general auto is used for removable devices and
    a specific file system or network protocol for network shares.

    - auto
    - vfat - used for FAT partitions.
    - ntfs, ntfs-3g - used for ntfs partitions.
    - ext4, ext3, ext2, jfs, reiserfs, etc.
    - udf,iso9660 - for CD/DVD.
    - swap

[Options]
    Mount options of access to the device/partition (see the man mount). You
    may use "defaults" here and some typical options may include
    ``defaults = rw, suid, dev, exec, auto, nouser, and async``.

    http://manpages.ubuntu.com/manpages/zesty/en/man8/mount.8.html

[Dump]
    Enable or disable backing up of the device/partition (the command dump).
    This field is usually set to ``0``, which disables it. This field sets
    whether the backup utility dump will backup file system. If set to ``0``
    file system ignored, ``1`` file system is backed up.

[Pass Num]
    Controls the order in which fsck checks the device/partition for errors at
    boot time. The root device should be ``1``. Other partitions should be
    ``2``, or ``0`` to disable checking.

    You may also "tune" or set the frequency of file checks (default is every
    30 mounts) but in general these checks are designed to maintain the
    integrity of your file system and thus you should strongly consider keeping
    the default settings.

Examples::

    # FAT16 and FAT32
    /dev/hda2 /media/data1 vfat defaults,user,exec,uid=1000,gid=100,umask=000 0 0
    /dev/sdb1 /media/data2 vfat defaults,user,dmask=027,fmask=137 0 0

    # NTFS, this example is perfect for a Windows partition.
    /dev/hda2 /media/windows ntfs-3g defaults,locale=en_US.utf8 0 0

    # For a list of locales available on your system, run
    locale -a



Swap File
-------------------------------------------------------------------------------

Create file in root, fast, but work only on ext4, xfs etc::

    $ fallocate -l 2G /swapfile

Manual, can exhaust memory, so set bs < free memory::

    $ sudo dd if=/dev/zero of=/swapfile bs=512M count=4

Check created file::

    ls -lh /swapfile

Set secure permissions::

    $ sudo chmod 600 /swapfile

Set up the swap space & enable swap::

    $ sudo mkswap /swapfile
    $ sudo swapon /swapfile

Check system reports swap::

    $ sudo swapon -s
    $ free -h

Make the swap file permanent add to ``/etc/fstab`` line::

    /swapfile none swap sw 0 0

Some tuning of swap can be made in ``/proc/sys/vm/swappiness``. Some guys is
recommended set it to «10». In case of ``swappiness=1`` - minimum swappiness
without disabling it entirely.  ``swappiness=100`` - tells the kernel to
aggressively swap processes out of physical memory and move them to swap cache.

To set the swappiness to a different value::

    $ sudo sysctl vm.swappiness=10

This setting will persist until the next reboot. To set this value
automatically at restart add the line directly to ``/etc/sysctl.conf`` file:

    vm.swappiness=10

or to the new end-user file ``60-*.conf`` at ``/etc/sysctl.d/``.

The RAM which is not occupied by running programs is used as disk cache, by
decreasing swappiness, you increase the chance of a program not to be swapped
out, but at the same time decrease the size of disk cache, which can make disk
access slower. **So the effects of this setting on the actual performance are not
that straightforward**.

Enable/disable devices and files for paging and swappin.
Use flags ``-v``, ``--verbose`` to be verbose. ``-a``, ``--all`` for all devices
marked as ‘‘swap’’ in ``/etc/fstab``::

    $ sudo swapoff -a
    $ sudo swapon -a
