MonitorMate-Modbus
===========

A monitoring system for the Outback Power MATE3. It processes the data stream, reformats it for logging, charting, and display on a web server.

Charts and graphs are creating using Highcharts JS for free under the Creative Commons "Attribution-NonCommercial 3.0 License" regulations. http://www.highcharts.com

This is a fork of jepefe/monitormate, created by Jesús Miguel Pérez and originally distributed on his website: http://www.jeperez.com/monitor-solar-outback/ 

How does this software work?
===========
The software is divided in three parts:

- PHP-CLI script for receive and relay MATE3 datastream to a webserver. (typically an off-site webserver.)
- PHP scripts for registering the data in the database and query’s.
- HTML/JavaScript webpage for visual representation.


monitormate.php
===========
Gets and minimally processes datastream to send current status to a webserver. Use the following options:

php monitormate.php -h

	Usage: monitormate.php [options]
	Options:

		-a IP_ADDRESS		IP address on which to listen for data stream. (optional, defaults to all)
		-p UDP_PORT			Port Mate3 is configured to use for Data Stream.
		-u URL				The URL to your MonitorMate web server installation.
		-t TOKEN			Token configured in config.php on your webserver. (optional, but recommended)
		-d					Debug output
	
post_datastream.php
===========
Gets a json string with devices status and record it in the database.


getstatus.php
===========
Gets data from database an returns a json string.


monitormate.html / monitormate.js
===========
Visual representation of devices status and history. Works with getstatus.php for history and with matelog for “real time” status.


config.php
===========
Configuration file. Contains database connection parameters, record interval, security token, time zone, and power system configuration.


monitormate.sh
===========
Init-style script for running monitormate automaticaly as a daemon. See your specific OS/distributions documentaiton for setting up daemons.


Installation and Execution
===========

1. Download monitormate and extract it.
2. Rename the config file to config.php (remove .sample)
2. Edit the config file to your liking.
3. Create the database manually, and a new database user if necessary.
4. Use database.sql to create tables in your MySQL database. (I suggest phpAdmin to import)
5. Copy the “Web Server” directory content to your web server.
6. copy the "Data Stream Host" directory content to your host computer (if it's not the one you're using)
7. Run monitormate.php on the host with the correct parameters.

Optionally, if you want it to run as a daemon on Linux (assumes systemd usage, modify as necessary):

1. Create /usr/local/bin/monitormate/
2. Copy monitormate.php to /usr/local/bin/monitormate/monitormate.php
3. Edit monitormate.service for correct paths, port, URL, and token.
4. Copy monitormate.service to /lib/systemd/system/monitormate.service
5. Run "sudo systemctrl enable monitormate.service" to have monitormate automatically run in the background at boot time.

Go to  http://www.YOURSERVER.com/index.php (maybe you have to wait a little depending your record interval.)

The relay script sends the data up to the server as soon as it has all available information. Because of the way FNDC works this is about 14 seconds. Interval for database record is in config.php.
