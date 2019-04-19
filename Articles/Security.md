# Security

## Principles

### Authentication

> Identify if a person is who they say they are.

### Authorization

> Identify what the person is allowed to do (whether or not the person is identified).

## Protocols

### HTTP(S)

The `HyperText Transfer Protocol`, also known as `HTTP` protocol is used for sending messages over a network.
It is an `Application Layer` protocol, and specifically used for sending messages of the `World Wide Web`.
The `World Wide Web` is compromised of `HTML` (`HyperText Markup Language`) documents which link to eachother using `URI`'s (`Uniform Resource Identifier`). In typical use the main `HTML` document is rendered in a `browser` using the files that have been referenced, this can be `images` and `scripts` among others.

The `Hypertext Transfer Protocol Secure`, also known as `HTTPS` protocol is an extension of the `HTTP` protocol, and provides extra security measures. 
`HTTPS` is used to:
- Prevent `eavesdropping` by 3th parties.
- Prevent `tampering` by 3th parties.

`HTTPS` encrypts the message using the `Application Layer` protocol named `TLS` (formarly known as `SSL`).
`TLS` fully encrypts the output of the `HTTP` protocol using a `Certificate`.

#### How it works 

You enter a web address in the browser (`https://google.be`)

The browser issues a `DNS` request to retrieve the location (`ip address`) of the website (`216.58.211.99`) if it is not stored in `cache` or in the `hosts file`. (`DNS` requests are send using the connectionlesss `Transport Layer` protocol `UDP`)

To send the actual request for the webpage, first a `TCP` (a `Transport Layer` protocol that creates a connection) connection is formed with some handshaking.

In case of `HTTP`, a `request` will be send without any encryption. 

The `request` will be send in the following format:

    GET / HTTP/1.1
    Host: google.be
    Connection: close
    ...

The `response` returned by the serer will be in this format:

    200 OK
    Date: Sun, 10 Oct 2010 23:26:07 GMT
    Content-Length: 0
    Connection: close
    Content-Type: text/html
    ...

Note that both follow the same format (`StatusLine` + `Headers` + `Body`)

In case of `HTTPS`, the above `request` will not be send without encrypting.

To encrypt the content, a `TLS` handshake is started after initiating a `TCP` connection with the server.
The `TLS` request contains the following information for the server:

- `TLS Version`
- `Ciphersuites`
- `Compression methods`

The server will select a version and a ciphersuite (and optionally a compression method).
Afterwards it will return a `certificate` to the client.

The `certificate` must be trusted by the client or a party that the client trusts. 
You can manually trust a certificate by installing it on the client machine.
You can delegate trust to a trusted party by using a `CA` certificate.

After this trust relationship is established, it is certain that the content is coming from the chosen website.
The only remaing task is for the `request` shown above to be encrypted using an algorithm associated with the chosen ciphersuite, and for it to be send to the server.

To authenticate with the `HTTP(S)` protocol, there are multiple ways. But there are 2 different categories:

#### HTTP(S) Authentication

`HTTP(S)` has support authentication built into the protocol.

Credentials are send as a request header with every `HTTP(S)` request.
A drawback is that the user cannot login on the website itself.
This is compareable to owning the keys of a house.

- You need to use the keys every single time you come home.
- Nobody is in the house to help with getting the keys.
- As long as you are in the house, you can keep the door unlocked.

When authentication is enabled the webserver will return `401 Unauthorized` with a `response header` named `www-authenticate`.
The `www-authenticate` header gives information about the specific `authentication scheme` that needs to be used.

Common methods

- `Basic`
- `Digest` 
- `Negotiate`
- `NTLM`

Modern browsers are implemented to display a login form which will allow the user to send their credentials back to the server.

Further requests that are linked to this browser session will also include this `request header` and prevent the need to authenticate on each individual request.

##### Basic

Response (when sending request without credentials)

> WWW-Authenticate: `realm="realm1"`

Valid request

> Authorization: `Basic D08mRvgvbhDsU`

##### Digest

Response (when sending request without credentials

> WWW-Authenticate: `Digest realm="realm1", qop="auth,auth-int", nonce="jdi"`

Valid request

> Authorization: `Digest username="user", realm="realm1", nonce="jd8d", uri="/path/res", qop="auth", nc=001, cnonce="jdi", response="JHD"`

##### Alternatives

Most modern websites do not enable authentication on the webserver, but inside the website.

This method of authentication is different and not part of the `HTTP(S)` protocol.
However, it serves a similar purpose from the end user perspective.

What happens is that the website will return a anonymous page in which the user only has restricted access.
A `login form on the webpage` can be used to login, providing a much `better user experience`.

This corresponds to going to a hotel and checking in to your room.
- The first time you come in the hotel you can book a room (`create account`)
- If you forgot your keycard you can ask the staff for assistance (`forgot password`)
- You can always enter the hotel, even if you don't have access to the room. (`branded login form inside the website`)

This solution uses `cookies`, which are also send and retrieved using a `request header` and `response header`.
After logging in the webserver will return the `response header` named `Set-Cookie` which will contain authenticated credentials.
Subsequent `HTTP(S)` requests will then include `Cookie` to keep the session valid, this is also automatic browser behavior.

Note that there are 2 types of cookies.

- `Session cookies` that expire after the session has ended, this can be implemented differently according to the `browser`.
- `Persistent cookies` that expire after a due date, the cookie is `saved on disk` along with the due date.

`Persistent cookies` are typically used when tagging a `remember me` checkbox on the login form.

## Assymetric Cryptography

`Assymetric Cryptography` is used for encrypting messages.
It makes use of a `public key` and `private key` which both contain non-identical large numbers.
You can encrypt a message with either key, and decrypt with the opposite key.
The `public key` can be shared without any risk.
The `private key` is to be kept secret.

`TLS` and other protocols such as `SSH`, `S/MIME` makes use of `Assymetric Cryptography` to ensure safe communications.
Since `HTTPS` uses `TLS`, it also makes use of `Assymetric Cryptography`

An example of `SSH` is a `linuxserver` where you can add the `public key` in a hidden folder `.ssh/id_rsa` in the user directory.
Using `PuTTY` on windows you can login without entering credentials by loading your `private key` into `Pageant`.
This is more secure than using password authentication.

## Certificate ->

hash
public key
private key
clientcertificate-servercertificate
pfx-p12-pem-cer-crt-der-X.509
selfsignedcertificate
CA

## Authentication -> 

OpenId Connect

## Authorization ->

oauth2-accesstoken-refreshtoken

## API ->

json-xml
wsdl
swagger-openapi-rest-odata

apikey (not related to user nor authorization, just key that allows you to use the webservice)

## Single Sign On -> 

kerberos-ntlm
securetokenservice
WS
SAML
claims

Identity Provider
Service Provider

## Webpage ->

Cross-Origin Resource Sharing CORS
Cookies

## Extra -> 

HMAC
PASETO
JWT
