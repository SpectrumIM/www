# Gateway mode and server mode

Spectrum 2 can work in two modes: Gateway mode and Server mode. This chapter describes differences between those two modes. If you want to find out how to configure Spectrum 2 to run in gateway or server mode, read the Configuration part of this documentation.

## Gateway mode

Gateway mode represents the classic way how XMPP gateway works. You have to configure an external XMPP server (like Prosody or Ejabberd) to serve the subdomain you want to use for Spectrum 2 (for example "icq.domain.tld"). Spectrum 2 in gateway mode then connects the XMPP server as its component and users are able to find out "icq.domain.tld" in Service Discovery, register it and use it.

#### Advantages:
- Users can use more legacy networks using single XMPP account (and using single TCP connection).
- It's easy to extend existing XMPP servers using gateway mode.

#### Disadvantages:
- Passwords are stored (even in encrypted form) on server.
- Roster (contact list) synchronization can be problematic, because it depends on the client user uses. This can be improved by usage of Remote Roster protoXEP.
- You have to setup XMPP server and use database even if you only want to use Spectrum 2 as a tool to connect legacy networks using XMPP protocol.

## Server mode

In server mode, Spectrum 2 behaves as standalone server. User then logins legacy networks by logging XMPP account like this one: "my_msn_name%hotmail.com@msn.domain.tld".

#### Advantages:
- Passwords are not stored on server.
- Roster synchronization is easy, because Spectrum 2 acts as normal server.
- If you want to use Spectrum 2 as wrapper between different networks, you don't need database or Jabber server as another layer.
- Using SRV records you can easily run Spectrum 2 on different machines to scale it.

#### Disadvantages:
- Clients have to support more accounts to connect more legacy networks (Therefore they will need have to use more TCP connections).
