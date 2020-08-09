To configure SSL support for Spectrum2 in server mode, you have to generate server-side certificate, convert it to PKCS#12 format and configure path to it in Spectrum 2 config file.

This article describes how to generate self-signed server certificate and use it in Spectrum 2.

### Setup your own CA (Certificate Authority)

	$ openssl genrsa -des3 -out my-ca.key 2048
	$ openssl req -new -x509 -days 3650 -key my-ca.key -out my-ca.crt

### Make a key and a certificate for the server

When prompted for "Common Name (eg, your name or your server's hostname) []:", add the hostname/JID of your transport (for example "localhost").

When prompted for "A challenge password []:", *do not* set it.

	$ openssl genrsa -des3 -out spectrum2-server.key 1024
	$ openssl req -new -key spectrum2-server.key -out spectrum2-server.csr
	$ openssl x509 -req -in spectrum2-server.csr -out spectrum2-server.crt -sha1 -CA my-ca.crt -CAkey my-ca.key -CAcreateserial -days 3650

### Convert server key and certficate to PKCS#12 format

When generating pkcs12 file, *do not* set the Export password. Spectrum 2 currently doesn't parse pkcs12 certificates with password.

	$ openssl pkcs12 -export -in spectrum2-server.crt -inkey spectrum2-server.key -out spectrum2-server.p12

### Set path to certificate in config file

Set the path to cert and configure certificate password if you set one for the pkcs12 file.

	[service]
	...
	cert=/etc/spectrum2/certificates/spectrum2-server.p12
