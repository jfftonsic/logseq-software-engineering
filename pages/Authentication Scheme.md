sources:: [Internet Assigned Numbers Authority Hypertext Transfer Protocol (HTTP) Authentication Scheme Registry](https://www.iana.org/assignments/http-authschemes/http-authschemes.txt)

- [[Basic]]
- [[Bearer]]
- [[Digest]]
- [[HOBA]]
- [[Mutual]]
- [[Negotiate]]
- [[OAuth]]
- [[SCRAM-SHA-1]]
- [[SCRAM-SHA-256]]
- [[vapid]]
-
- Basic                      [RFC7617]
     Bearer                     [RFC6750]
     Digest                     [RFC7616]
                                                         The HOBA scheme can be used with either HTTP servers or proxies. When used in response to a
     HOBA                       [RFC7486, Section 3]     407 Proxy Authentication Required indication, the appropriate proxy authentication header
                                                         fields are used instead, as with any other HTTP authentication scheme.
     Mutual                     [RFC8120]
                                                         This authentication scheme violates both HTTP semantics (being connection-oriented) and
     Negotiate                  [RFC4559, Section 3]     syntax (use of syntax incompatible with the WWW-Authenticate and Authorization header field
                                                         syntax).
     OAuth                      [RFC5849, Section 3.5.1]
     SCRAM-SHA-1                [RFC7804]
     SCRAM-SHA-256              [RFC7804]
     vapid                      [RFC 8292, Section 3]
-