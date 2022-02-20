# Running script on login

Files with the .sh extension in the /etc/profile.d directory get executed whenever a bash login shell is entered (e.g. when logging in from the console or over ssh), as well as by the DisplayManager when the desktop session loads.

You can for instance create the file /etc/profile.d/myenvvars.sh and set variables like this:

```
export JAVA_HOME=/usr/lib/jvm/jdk1.7.0
export PATH=$PATH:$JAVA_HOME/bin
```

Other files

While /etc/profile is often suggested for setting environment variables system-wide, it is a configuration file of the base-files package, so it's not appropriate to edit that file directly. Use a file in /etc/profile.d instead as shown above. (Files in /etc/profile.d are sourced by /etc/profile.)

/etc/default/locale is specifically meant for system-wide locale environment variable settings. It's written to by the installer and when you use Language Support to set the language or regional formats system-wide. On a desktop system there is normally no reason to edit this file manually.

The shell config file /etc/bash.bashrc is sometimes suggested for setting environment variables system-wide. While this may work on Bash shells for programs started from the shell, variables set in that file are not available by default to programs started from the graphical environment in a desktop session. 

More info [here](https://help.ubuntu.com/community/EnvironmentVariables#A.2Fetc.2Fprofile.d.2F.2A.sh)
