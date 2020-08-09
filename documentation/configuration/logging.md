Spectrum 2 uses [log4cxx](http://logging.apache.org/log4cxx/) for logging. In the main config file, there are two options to set full path to log4cxx configuration files which are then used for backends and Spectrum 2 main instance:

	[logging]
	# Full path to config file used for main Spectrum 2 instance logging
	config=/etc/spectrum2/logging.cfg

	# Full path to config file used for backends logging
	backend_config=/etc/spectrum2/backend-logging.cfg

## Log4cxx config files

There is full [documentation of log4cxx on log4cxx homepage](http://logging.apache.org/log4cxx/index.html).

### Logging everything to stdout

For logging to stdout, we have to use ConsoleAppender appender like this:

	# We create two rootLoggers:
	#   - "debug" is internal logger used by log4cxx
	#   - "stdout" is name of our ConsoleAppender logger
	log4j.rootLogger=debug, stdout

	# Create new ConsoleAppender logger with custom PatternLayout
	log4j.appender.stdout=org.apache.log4j.ConsoleAppender

	# Define the output pattern. Characters are mentioned here: http://logging.apache.org/log4cxx/apidocs/classlog4cxx_1_1_pattern_layout.html
	log4j.appender.stdout.layout=org.apache.log4j.PatternLayout
	log4j.appender.stdout.layout.ConversionPattern=%d %-5p %c: %m%n

Configuration options for ConversationPattern are described [here](http://logging.apache.org/log4cxx/apidocs/classlog4cxx_1_1_pattern_layout.html).

### Logging everything to file

	# We create two rootLoggers
	#   - "debug" is internal logger used by log4cxx
	#   - "R" is name of our RollingFileAppender logger
	log4j.rootLogger=debug, R

	# Create new RollingFileAppender logger
	log4j.appender.R=org.apache.log4j.RollingFileAppender
	# Set the filename
	log4j.appender.R.File=/var/log/spectrum2/${jid}/spectrum2.log

	# Set MaxFileSize. Log will be rotated automatically when this limit is reached
	log4j.appender.R.MaxFileSize=10000KB
	# Keep one backup file
	log4j.appender.R.MaxBackupIndex=1

	# Define the output pattern. Characters are mentioned here: http://logging.apache.org/log4cxx/apidocs/classlog4cxx_1_1_pattern_layout.html
	log4j.appender.R.layout=org.apache.log4j.PatternLayout
	log4j.appender.R.layout.ConversionPattern=%d %-5p %c: %m%n

### Logging XML to different file

We have to create another RollingFileAppender to achive this:

	# We create two rootLoggers
	#   - "debug" is internal logger used by log4cxx
	#   - "R" is name of our RollingFileAppender logger for everything except XML
	log4j.rootLogger=debug, R

	# ---- spectrum2.log

	# Create new RollingFileAppender logger
	log4j.appender.R=org.apache.log4j.RollingFileAppender
	# Set the filename
	log4j.appender.R.File=/var/log/spectrum2/${jid}/spectrum2.log

	# Set MaxFileSize. Log will be rotated automatically when this limit is reached
	log4j.appender.R.MaxFileSize=10000KB
	# Keep one backup file
	log4j.appender.R.MaxBackupIndex=1

	# Define the output pattern. Characters are mentioned here: http://logging.apache.org/log4cxx/apidocs/classlog4cxx_1_1_pattern_layout.html
	log4j.appender.R.layout=org.apache.log4j.PatternLayout
	log4j.appender.R.layout.ConversionPattern=%d %-5p %c: %m%n

	# ---- spectrum2_xml.log

	# Define new logger for category Component.XML:
	#   - "debug" is internal logger used by log4cxx
	#   - "XML" is the name of our RollingFileAppender logger for XML category
	log4j.category.Component.XML = debug, XML

	# Do not add XML category into "R" logger, so XML category will be logged only to spectrum2_xml.log, but not to spectrum2.log.
	# If you want to have XML category also in spectrum2.log, set this value to "true"
	log4j.additivity.Component.XML=false

	# Create new RollingFileAppender logger and set the file name
	log4j.appender.XML=org.apache.log4j.RollingFileAppender
	log4j.appender.XML.File=/var/log/spectrum2/${jid}/spectrum2_xml.log

	# Set MaxFileSize. Log will be rotated automatically when this limit is reached
	log4j.appender.XML.MaxFileSize=100000KB
	# Keep one backup file
	log4j.appender.XML.MaxBackupIndex=4

	# Define the output pattern. Characters are mentioned here: http://logging.apache.org/log4cxx/apidocs/classlog4cxx_1_1_pattern_layout.html
	log4j.appender.XML.layout=org.apache.log4j.PatternLayout
	log4j.appender.XML.layout.ConversionPattern=%d %-5p %c: %m%n

### Disable XML logging

	# We create two rootLoggers:
	#   - "debug" is internal logger used by log4cxx
	#   - "stdout" is name of our ConsoleAppender logger
	log4j.rootLogger=debug, stdout

	# Create new ConsoleAppender logger with custom PatternLayout
	log4j.appender.stdout=org.apache.log4j.ConsoleAppender

	# Define the output pattern. Characters are mentioned here: http://logging.apache.org/log4cxx/apidocs/classlog4cxx_1_1_pattern_layout.html
	log4j.appender.stdout.layout=org.apache.log4j.PatternLayout
	log4j.appender.stdout.layout.ConversionPattern=%d %-5p %c: %m%n

	# Disable XML category
	log4j.category.Component.XML = OFF

### Disable logging

To disable logging, you still *must have* one logger created (probably the ConsoleAppender), but you can set log4j.threshold = OFF to not log everything later:

	# We create two rootLoggers:
	#   - "debug" is internal logger used by log4cxx
	#   - "stdout" is name of our ConsoleAppender logger
	log4j.rootLogger=debug, stdout

	# Create new ConsoleAppender logger with custom PatternLayout
	log4j.appender.stdout=org.apache.log4j.ConsoleAppender

	# Define the output pattern. Characters are mentioned here: http://logging.apache.org/log4cxx/apidocs/classlog4cxx_1_1_pattern_layout.html
	log4j.appender.stdout.layout=org.apache.log4j.PatternLayout
	log4j.appender.stdout.layout.ConversionPattern=%d %-5p %c: %m%n

	# Disable logging of everything:
	log4j.threshold = OFF


