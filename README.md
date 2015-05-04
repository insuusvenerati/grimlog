# grimlog
Simple, lightweight log aggregator and web viewer

Grimlog is a simple log aggregator that collects application logs over the network and saves them in sqlite database. Collected logs 
can be browsed and searched via web gui which is also provided by the application. The tool is implemented using nodejs and has everything
that's necessary to collect and browse logs.

# Features
  * Log messages are sent to grimlog over UDP, every message is a JSON object. Simple JSON and GELF format are supported. More transport options/formats to come (hopefully).
  * Many popular logging frameworks can be configured to send logs over UDP/GELF so no additional libraries are necessary on the application side.
  * Single process. Glog-view runs as a single nodejs application and doesn't have any dependencies on other running services. 
  * Low footprint - single nodejs application, consumes 30-130 MB of RAM, compact log file format, efficient and lightweight implementation.  
  * Simple configuration, or no configuration at all. Just install and run, the same procedure on Linux, Windows or Mac OS. 
  * Log rotation. By default Glog-view rotates the log files on a daily basis and allows you to browse all collected files.
  * Performance: sqlite database is able to store 20-50 thousand records per second so this is the theoretical maximum of what grimlog can handle. With full-text search enabled we're looking at about 10K events/second, maybe more if the machine has fast SSD disk + enough CPU. Database performance is the key limit here and better perf tests are necessary. Anyway, this is much more than logstash + elasticsearch can handle.
  

# Status
Currently in alpha, some important parts need to be completed

# Message fields
List of fields that are handled/understood by grimlog 
* message - message text (short_message and full_message in GELF) - mandatory
* level - log level (syslog severity, same as in GELF) - optional
* ts - timestamp (num of milliseconds since epoch) - optional 
* source - log source (application name for example) - optional
* logname - log name (_logname custom field in GELF message) - optional
* pid - process ID (_pid in GELF) - optional
* threadid - thread ID (_threadid in GELF) - optional
* correlation - correlation id that joins group of messages (_correlation in GELF) - optional

# TODO List - ideas, plans    
  * ~~Full-text search (currently only normal sql queries are used)~~ - DONE
  * More transport options (ZeroMQ, TCP)
  * Event parsing/pre-processing so we can handle other message formats as well
  * Event filtering so we can decide whether events will be logged or not
  * Setup instructions for most popular logging libraries (NLog, Log4J, Log4net, ...)
  * Statistics display (histogram etc)
  * 'Global search' - parallel search in all log files at once
  * Data export
  * Configurable log views
  * Online event processing (configurable rules for handling incoming events and detecting some patterns/anomalies or deciding what to do with an event)
  * Performance metrics (e.g. forwarding some data to performance monitoring tool, time series database, rrdtool)
  * Alerts (nagios integration?)
  * User authentication - via some external auth mechanism
  * Reading logs from text files, tailing text log files and parsing events from them
  * Health check (basic website with some stats/grimlog health information)
 
  
# Installation
 
## Ubuntu

  * sudo apt-get install sqlite3
  * make sure you have 'node' link configured in addition to /usr/bin/nodejs that's installed by default. Required to install sqlite3 package correctly. `ln -s /usr/bin/nodejs /usr/local/bin/node` should be sufficient.
  * clone grimlog repository (git clone git://....)
  * npm install 
  * `nodejs server.js` to run the app (or `npm start`)

    
## Windows

  * install nodejs
  * clone grimlog git repository
  * npm install
  * `node server.js` - to run from command line (`npm start` should work too)
  * in order to run grimlog as a windows service please install it with `npm run-script install-windows-service` (you need to run the command as administrator). Then 'grimlog' windows service will be created and you should be able to start it.
