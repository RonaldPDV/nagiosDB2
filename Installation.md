This is the procedure to install this set of scripts to monitor DB2.

# Requirements

The requirements are:

* DB2 (These scripts have been tested in b9.7 and v10.5, and many scripts could work in v9.5)
* Nagios (In v3 has been tested)
* Linux / Unix with **Bash**
* Optional: Check_MK

# Retrieving scripts

The only way to get the binaries is via this forge.

* You can download a generated zip containing all files.
* Cloning the git repository.

The first option allows you have a snapshot copy of all scripts. This is useful if you cannot access GitHub from the Nagios or DB2 server, and you have to download to an stage area (i.e. your own PC) and then copy to the target server. With this option you have to repeat the process in order to get the new versions.

The second option is the easiest and it is recommended. As scripts are sources, the best is to provide a mechanism designed to manage source files. When you clon this repository, you will get the most recent version of all scripts. If you want to update to a newest version, you just have to pull the new sources `git pull`. If you modified the scripts for your own requirements, you can create a branch for edit purposes, and other for the original scripts.

# Configuration

Each script has to be configured individually. Each script is autonomous, and they do not require configuration files, of similar thing.

In order to add this set of scripts to your Nagios installation, you can open each script and see the documentation. Each script contain examples of the parameters to put in the configuration file.

The default configuration probably will not work with your requirements, and for this reason you have to tune the warning and critical parameters, in order to not receive "false positives" (You do not want to be waked up at midnight for an alarm that does not really exists.)