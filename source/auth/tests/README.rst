==========
Auth Tests
==========

The YAML and JSON files in this directory tree are platform-independent tests
that drivers can use to prove their conformance to the Auth Spec at least with
respect to connection string URI input.

Drivers should do additional unit testing if there are alternate ways of
configuring credentials on a client.

Driver must also conduct the prose tests in the Auth Spec test plan section.

Format
------

Each YAML file contains an object with a single ``tests`` key. This key is an
array of test case objects, each of which have the following keys:

- ``description``: A string describing the test.
- ``uri``: A string containing the URI to be parsed.
- ``valid:`` A boolean indicating if the URI should be considered valid.
- ``credential_configured:`` A boolean indicating if the URI is considered to have
  configured a credential for the purposes of deciding if the driver should
  authenticate to the topology.
- ``credential``: An object containing the following keys:

  - ``username``: A string containing the username. For auth mechanisms
    that do not utilize a password, this may be the entire ``userinfo`` token
    from the connection string.  If this is the empty string,
    test harnesses should interpret it as unspecified.
  - ``password``: A string containing the password.  If this is the empty string,
    test harnesses should interpret it as unspecified.
  - ``source``: A string containing the authentication database.
  - ``mechanism``: A string containing the authentication mechanism.  The string "default"
    is used to indicate that a mechanism wasn't specified and that mechanism negotiation
    is required.  Test harnesses should modify the mechanism test as needed to assert
    this condition; drivers are not required to use the string literal "default" as a
    mechanism name.
  - ``mechanism_properties``: A document containing mechanism-specific properties

If a test case includes a null value for one of these keys (e.g. ``credential:
~``), no assertion is necessary.

Implementation notes
====================

Testing whether a URI is valid or not should simply be a matter of checking
whether URI parsing (or MongoClient construction) raises an error or exception.

If a credential is configured, its properties must be compared to the
``credential`` field.
