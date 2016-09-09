Introduction
============

FreeRadius server configured to use an OpenLDAP backend. Primarily intended to
be used as a Cisco IPSEC VPN AAA server.

Optional support is provided so that users must be a member of a certain LDAP
group in order to receive RADIUS access. If this is not used, all users which
can authenticate successfully using LDAP will be granted RADIUS access.

Environment Variables
=====================

- `LDAP_HOST` - LDAP server hostname(s) (default: "ldap1.example.com ldap2.example.com")
- `LDAP_PORT` - LDAP server port (default: "389")
- `LDAP_USER` - LDAP server user (default: "cn=admin,dc=example,dc=com")
- `LDAP_PASS` - LDAP server password (default: "password")
- `LDAP_BASEDN` - LDAP server Base DN (default: "dc=example,dc=com")
- `LDAP_USER_BASEDN` - LDAP server Users Base DN (default: "ou=Users,dc=example,dc=com")
- `LDAP_GROUP_BASEDN` - LDAP server Groups Base DN (default: "ou=Groups,dc=example,dc=com")
- `LDAP_CLIENT_BASEDN` - LDAP server Freeradius Clients Base DN (default: "ou=Clients,dc=example,dc=com")

- `LDAP_RADIUS_ACCESS_GROUP` - The LDAP group which users must belong to in order to have RADIUS access (optional) (default: "")
- `RADIUS_CLIENT_CREDENTIALS` - The Freeradius server client credentials (comma separated "hostname:password" pairs, default: "")

- `RADIUSD_ARGS` - Arguments to pass to radiusd (default: "-f -l stdout")

Example Docker Compose Configuration
====================================

    radius:
      image: irasnyd/freeradius-ldap:latest
      ports:
        - "1812:1812/udp"
        - "1813:1813/udp"
      environment:
        - "LDAP_HOST=ldap.example.com"
        - "LDAP_USER=cn=admin,dc=example,dc=com"
        - "LDAP_PASS=adminpassword"
        - "LDAP_BASEDN=dc=example,dc=com"
        - "LDAP_USER_BASEDN=ou=Users,dc=example,dc=com"
        - "LDAP_GROUP_BASEDN=ou=Groups,dc=example,dc=com"
        - "LDAP_RADIUS_ACCESS_GROUP=vpnaccess"
        - "RADIUS_CLIENT_CREDENTIALS=1.2.3.4:password1234,5.6.7.8:password5678"
      mem_limit: "1g"
      restart: "always"
