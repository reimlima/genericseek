genericseek
--------------------------------------------------------------------------------

Counts  number  of occurrences of a stretch to the end of the log and stores the
position  to  tell  from  where  it  stopped.  Great   for  creating  monitoring
processes in real time logs that are being increased.

Changelog
--------------------------------------------------------------------------------

2015-01-09 - v1   - First Stable Release

2015-01-23 - v1.5 - A lot of improvements in config file read, write and recover

2015-12-10 - v2   - No more config file, only commandline options

Dependencies
--------------------------------------------------------------------------------

As a Perl script you will need to install via CPAN the Modules below:

	JSON
	XML::Simple
	Getopt::Std
	Sys::Hostname
	File::Basename

How to Use
--------------------------------------------------------------------------------

COMMANDLINE OPTIONS

	Usage: genericseek -l <log> -k <key> -o '<log occurrence>' -f <outputformat> -w <warning> -c <critical>

        -l: logfile, Ex.: /var/log/message
        -k: key, something to be used as output message Ex.: "error" or "File Monitoring from Application Log"
        -o: occurrence, whatever you're looking for in the log file Ex.: "error"
        -f: outputformat, what kind of output you want? Ex.: json, xml, icinga
        -w: warning, threshold for Warning Message Ex.: 30
        -c: critical, threshold for Critical Message Ex.: 50 (must be bigger than warning number)

DB FILE:

For  the  very  first time running the script this file must be created, so just
execute a "touch command" and leave it empty, the script will do the rest.

	# touch genericseek.db

Output Format
--------------------------------------------------------------------------------

This  is how the output will look depending of your choise (this is just a dummy example OK?).

XML Output:

	<?xml version="1.0" encoding="UTF-8"?>
		<filematch>
		<key>test</key>
		<code>0</code>
		<hostname>myhost</hostname>
		<outputMsg>OK</outputMsg>
		<timestamp>1449756463</timestamp>
		<value>0</value>
	</filematch>

JSON OUTPUT:

	{"filematch": [{
		"hostname": "c019531"
	}, {
		"timestamp": 1449756524
	}, {
		"key": "test"
	}, {
		"value": "0"
	}, {
		"code": 0
	}, {
		"outputMsg": "OK"
	}]}

ICINGA OUTPUT:

	[1449756590] PROCESS_SERVICE_CHECK_RESULT;c019531;test;0;OK
