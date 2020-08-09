Spectrum 2 consist of several separate parts which cooperates together. This page describes them.

## Spectrum 2 instance, Spectrum 2 main process and backends

This chapter describes differences between Spectrum 2 instance, Spectrum 2 main process and Spectrum 2 backends.

### Spectrum 2 instance

In server mode, Spectrum 2 instance is single XMPP server. In gateway mode, it is single XMPP gateway. We can say these statements about Spectrum 2 instance:

- One instance is defined by one configuration file (By default stored in /etc/spectrum2/transports/).
- Spectrum 2 instance is represented to end user by its Jabber ID (for example icq.domain.tld).
- Internally every Spectrum 2 instance consists of Spectrum 2 main process and Spectrum 2 backends.
- One instance can run just one type of Spectrum 2 backend.

### Spectrum 2 main process

Spectrum 2 main process is the main process of Spectrum 2 instance.

- Spectrum 2 main process is responsible for running Spectrum 2 backends and routing users' requests to proper backend.
- By default the logs of Spectrum 2 main process are stored in /var/log/spectrum2/<jid>/spectrum2.log.

### Spectrum 2 backend

Spectrum 2 backend is special application run by Spectrum 2 main process. The goal of Spectrum 2 backend is to handle users's sessions. Spectrum 2 main process can handle more Spectrum 2 backends, but all messages from single user are always handled by the same backend.

One Spectrum 2 instance can run only one type of Spectrum 2 backend.

## Where are all those things in git-tree?

Directory| Description
---------|------------
./src|Libtransport source codes
./include/transport|Libtransport header
./plugin/cpp|Libtransport-plugin source codes
./backends/|Various Spectrum 2 backends source codes
./spectrum/src|Spectrum 2 source codes
./spectrum/src/frontends| Spectrum 2 frontends source codes

## Libtransport

Libtransport is library providing the high-level interface for creating transports. It's used by the Spectrum 2 and by several backends and by all frontends.

Libtransport contains NetworkPluginServer class, which acts as server to which backends connect. Libtransport spawns the backend's processes when they are needed (for example when new user logs in) and destroys them when they are not needed anymore (for example when there are no active users on the backend).

Libtransport is used by:

Name| Reason
----|-------
Spectrum 2|It's the Spectrum 2 core
Some backends|Connect the Spectrum2, use of Spectrum 2 database, parsing the config file, ...
Frontends|Core library for frontends development

Libtransport uses:

 Name| Reason
-----|-------
Swiften library|Historical reasons. It's used as a utils library and basic data structures are represented using classes from Swiften library.
log4cxx|Logging
protobuf|Protocol for libtransport - backends communication
mysql-client|MySQL support
sqlite3|SQLite3 support
pqxx|PostgreSQL support
curl|HTTP requests for OAuth2 and REST frontends

## Libtransport-plugin

Libtransport-plugin is subset of Libtransport library and contains only basic things for backend development. The goal is to have smaller library with the less dependencies than Libtransport.

The Libtransport-plugin contains NetworkPlugin class, which is the base class for every C++ backend. Programmer has to create his own class inherited from this one and implement all the virtual methods to create new backend.

Libtransport-plugin is used by:

Name| Reason
----|-------
All Backends|Connect the Spectrum 2, parsing the config file

Libtransport-plugin uses:

 Name| Reason
-----|-------
log4cxx|Logging
protobuf|Protocol for libtransport - backends communication

## Spectrum 2

Main Spectrum 2 binary just uses Libtransport and it's core classes to create particular Spectrum 2 instance.

Spectrum2 uses:

Name| Reason
----|-------
Libtransport|Core library...

## Backends

Backends allow communication with particular legacy network and implements things like logging the user in, sending/receiving messages from legacy network and so on. Backend's life-cycle is controlled by the Spectrum 2 (or better said by the Libtransport's NetworkPluginServer class).

Spectrum 2 spawns the backend and gives it `"--host localhost --port 32453"` parameters. Backend then has to connect the Spectrum 2 located at the given host/port and start receiving the commands sent by the Spectrum 2 main instance. For C++, there is wrapper class called NetworkPlugin which does the parsing and allows programmer to code backend just by implementing few virtual methods.

## Frontends

Frontends allow communication with the network Spectrum 2 users are using. Frontends are statically linked libraries currently. They are implementing `./include/transport/Frontend.h` class.
