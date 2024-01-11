![Spectrum 2 animation](/animation.gif){:.animation}

Spectrum 2 is an XMPP transport/gateway and XMPP server. It allows XMPP users to communicate with their friends who are using one of the supported networks. It supports a wide range of different networks such as ICQ, XMPP (Jabber, GTalk), AIM, MSN, Facebook, Gadu-Gadu, IRC and SIMPLE. Spectrum is written in C++ and uses the Swiften XMPP library and different libraries for legacy networks. Spectrum 2 is open source and released under the GNU GPL2+ license.

## Spectrum 2 frontends and backends

As it was already written above, Spectrum 2 supports multiple IM networks. Spectrum 2 distinguishes between **Frontend** and **Backend** networks.

If some network is supported as **Frontend**, it means that Spectrum 2 allows its users to use this network to communicate with other users using the **backend** network.

#### Example

If you for example use **Slack** as a frontend and **IRC** as a backend, the Spectrum 2 users can add Spectrum 2 to their Slack team, join the IRC network and communicate with their friends on IRC network.

## Supported frontends

Currently, following frontends are supported:

* XMPP
* Slack

## Supported backends

* IRC
* XMPP
* Facebook
* MSN
* Yahoo
