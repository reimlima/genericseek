# genericseek
--------------------------------------------------------------------------------

Counts  number  of occurrences of a stretch to the end of the log and stores the
position  to  tell  from  where  it  stopped.  Great   for  creating  monitoring
processes in real time logs that are being increased.

# Changelog
--------------------------------------------------------------------------------

* **09/01/2015 - v1   - First Stable Release**

* **23/01/2015 - v1.5 - A lot of improvements in config file read, write and recover**

* **10/12/2015 - v2   - No more config file, only commandline options**

* **12/09/2017 - v3   - Zabbix is now an option of output, Some improvements under the hood**

* **06/02/2019 - v4   - Now you can send your monitoring stuff to Slack**

# Dependencies
--------------------------------------------------------------------------------

As a Perl script you will need to install via CPAN the Modules below:

	JSON
	XML::Simple
	Getopt::Std
	Sys::Hostname
	File::Basename
	CPAN::Meta
	CPAN::Meta::YAML
	Module::Build
	WebService::Slack::IncomingWebHook

# How to Use
--------------------------------------------------------------------------------

COMMANDLINE OPTIONS

	Usage: genericseek -l <log> -k <key> -o '<log occurrence>' -f <outputformat> -w <warning> -c <critical>

        -l: logfile, Ex.: /var/log/message
        -k: key, something to be used as output message Ex.: "error" or "File Monitoring from Application Log"
        -o: occurrence, whatever you're looking for in the log file Ex.: "error"
        -f: outputformat, what kind of output you want? Ex.: json, xml, icinga, zabbix
        -w: warning, threshold for Warning Message Ex.: 30
        -c: critical, threshold for Critical Message Ex.: 50 (must be bigger than warning number)

DB FILE:

For  the  very  first time running the script this file must be created, so just
execute a "touch command" and leave it empty, the script will do the rest.

	# touch genericseek.db

# Prepare Slack to receive data

First things first, you have to read and follow the instructions in this [Documentation] before send something to slack.

Next, when you finally have the webhook URL you want to send your notifications change the parameter 'webhook_url' (line 92 in the script)

Ensure that the module 'WebService::Slack::IncomingWebHook' is properly installed.

# Output Format
--------------------------------------------------------------------------------

This  is how the output will look depending of your choise (this is just a dummy example OK?).

XML Output:
```xml
	<?xml version="1.0" encoding="UTF-8"?>
		<filematch>
		<key>test</key>
		<code>0</code>
		<hostname>myhost</hostname>
		<outputMsg>OK</outputMsg>
		<timestamp>1449756463</timestamp>
		<value>0</value>
	</filematch>
```
JSON OUTPUT:

```json
	{"filematch": [{
		"hostname": "myhost"
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
```

ICINGA OUTPUT:

```
	[1449756590] PROCESS_SERVICE_CHECK_RESULT;myhost;test;0;OK
```

ZABBIX OUTPUT:

```json
	{
		"data": [
		{
			"host": "myhost",
			"value": "0",
			"key": "test"
		}
		],
	"request": "sender data",
	"clock": 1505235023
}
```

SLACK OUTPUT:

```
	10.10.10.10 -> slack: error.log => [CRITICAL] Number of events "PHP Fatal error" in logfile is greater than 50
```

[//]: # (Links bellow)

   [Documentation]: <https://api.slack.com/tutorials/slack-apps-hello-world>
