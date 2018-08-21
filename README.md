# Valhalla

Valhalla is the tools repository to be used with Valkyrie.  It contains the automated actions that Valkyrie can take.  For example, the clear_mailq binary stored in ./live 
will delete all files in /var/spool/clientmqueue.  The 'src' folder contains any pre-compiled source code, with the resulting binary's target destination being the 'live' folder.
Scripts should not use 'src', but should be placed in 'live', as interpreted languages are *NOT* compiled, but run directly.  The listing of tools so far, is as follow:

### clear_mailq
The clear_mailq tool is used to delete built up mails in /var/spool/clientmqueue.  If this tool gets run, it's likely indicitive of an issue with sendmail being misconfigured,
or the sendmail daemon not running.  In our environment, sendmail should not be running on the servers, and submit.cf should be configured to send it's mta to our mailserver.
Currently, Puppet manages this configuration, but this tool could be updated to also fix /etc/mail/submit.cf, or to turn on sendmail if it's meant to be running.

### free_wasted_space
This tool is used mainly to truncate log files.  It's also configured to clear yum cache should any exist.  It is meant to clean up /var/logs and currently is coded to prevent
deletion of anything that is *NOT* a .log file (this means it will exclude /var/log/messages, etc). This should probably be updated to have a --flag passed that can choose
to bypass that protection, but as of yet, it does not have this feature.

### find_big_file 
Related to the free_wasted_space tool, this will search for a big file on the passed in directory. It does a file-walk, and looks for the largest 3 files on the directory and 
all of it's child directories.  This tool is a reporting only tool, and won't take any delete or clean up actions.

### find_offensive_pid
The find_offensive_pid tool is used when CPU Usage spikes.  It logs in and basically does a top and ps to figure out which pid is wreaking havok on the system.  This is also
a reporting only tool, and will take no clean up actions.  It will report the top 3 most offensive pids.

### delete_nagios_host
The delete_nagios_host utility is provided as a way for a system to remove nagios hosts from a nagios server.  We found some pretty nasty stability and inconsistency issues
when using the web API for Nagios, so we changed this tool to use the .php files provided by Nagios.  This tool is currently hard-coded to use /usr/local/nagiosxi/script (the
default install location of nagiosxi).
