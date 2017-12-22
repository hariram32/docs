<!-- @@@title:LDAP Authentication@@@ -->


# LDAP
As of version 5.84, LogZilla may be configured to use LDAP for authentication.

# Before You Begin

In order to avoid conflicts from adding LDAP authentication, you must change any pre-existing local accounts that will have the same email addresses of any LDAP accounts.

# Configuration Steps

1. Log in to your LogZilla server.

2. Next, set the variables below for your LDAP server's information:

```shell
# These are all company specific, please consult your LDAP admin for exact CN's, DN's, and Groups.

LDAP_SERVER_URL='ldap://ldap.company.com'
BIND_PASSWORD='XDab64E8awjeb2AWnEYydVUVm8FBwJd'
LDAP_BIND_DN='cn=Logzilla,ou=Binding,dc=mygroupname,dc=company,dc=com'
USER_SEARCH_DN='ou=Engineers,dc=mygroupname,dc=company,dc=com'
GROUP_SEARCH_DN='ou=Groups,dc=mygroupname,dc=company,dc=com'
OBJECT_SEARCH_CLASS='(objectClass=posixGroup)'
OBJECT_GROUP_CLASS='posixgroup'

# Maps the local user to the user's `First Name` and `Last Name` in LDAP
LDAP_FN='givenName'
LDAP_LN='sn' 

# Maps the local user to the actual login username from LDAP
LDAP_UID='uid' 

# Map the local email associated with account to the LDAP field containing the user's email address.
LDAP_EMAIL='mail' 
```

3. Paste the following:

```shell
sudo /home/logzilla/src/bin/lz5ldapconfig \
    --enabled 1 \
    --server-url "${LDAP_SERVER_URL}" \
    --tls 1 \
    --bind-dn "${LDAP_BIND_DN}" \
    --bind-password "${BIND_PASSWORD}" \
    --user-search-dn "${USER_SEARCH_DN}" \
    --group-search-dn "${GROUP_SEARCH_DN}" \
    --tls-ignore-cert-check 1 \
    --group-search-dn-filter "${OBJECT_SEARCH_CLASS}" \
    --group-object-class "${OBJECT_GROUP_CLASS}" \
    --last-name-field "${LDAP_LN}" \
    --first-name-field "${LDAP_FN}" \
    --username-field "${LDAP_UID}" \
    --email-field "${LDAP_EMAIL}"
```


# Testing
To test whether or not LDAP is working, users may use the `lz5ldapconfig` tool. For example:

```
lz5ldapconfig --test
```

After ensuring connectivity, log in to the UI using your LDAP credentials. 

# User Login
Users should be instructed to use only their LDAP username and not the full domain username. 

**Correct Login Name:**
`someuser`

**Incorrect:**
`somuser@domain.com`

**Incorrect:**
`DOMAIN\someuser`

