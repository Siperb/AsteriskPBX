# Siperb Asterisk Dockerfile
This docker file is for sampling the Siperb Inbound Connections. 

More information here:
[Siperb](https://www.siperb.com/)

[An Introduction to Connections](https://www.siperb.com/kb/article/an-introduction-to-connections/)

### Inbound Connection

Inbound Registration is when the SIP details, are automatically generated on our side. Once created, it is up to you, to REGISTER with us, just like a typical UAC to UAS scenario. This is a slightly unusual as you will need to create a REGISTER entry within your Asterisk Server, to maintain the registration. With this option, you are not required to open your firewall ports as the REGISTER messages will originate from within your network to us, making the necessary communication. This is a more secure option, but does require additional configuration on the Asterisk side, and not all ISP will offer this option.

More information can be found here:
[Siperb - Inbound Registration Connections](https://www.siperb.com/kb/article/inbound-registration-connections/)
