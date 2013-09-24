sslserve
========

Serve local files with this simple HTTPS web server

Installation
============

Gem is available here: https://rubygems.org/gems/sslserve

    $ gem install sslserve

Usage
=====

    $ sslserve --help
    Usage sslserve [options]
        -d, --dir DIR                    Directory to serve files from. Defaults to cwd
        -l, --listen IP                  IP address bind to. Defaults to 127.0.0.1
        -p, --port PORT                  PORT to listen on. Defaults to 3443
        -n, --name NAME                  NAME for SSL certificate. Defaults to localhost
        -e, --expires HOURS              Certificate expires in HOURS hours. Defaults to 1
        -U, --user USERNAME              USERNAME for HTTP Basic Auth. Defaults to disabled
        -P, --pass PASSWORD              PASSWORD for HTTP Basic Auth. Defaults to disabled
        -r, --realm REALM                HTTP Basic Auth REALM. Defaults to sslserve realm
        -h, --help                       Show this message
        -v, --version                    Show version


Basic Auth
==========

You can use HTTP Basic Auth to limit access to the data. It is enabled using the -U and -P options, both of which must be set.

Certificate Verification
========================

Because basic auth sends the username and password in the clear, the connection must be encrypted, which is the case when using sslserve. However, sslserve generates self-signed certificates which are easily spoofed. This means it is possible for someone to man-in-the-middle the connection and present a different certificate. 

If you really need secure the connection on an untrusted network (pro tip: all networks should be treated as untrusted), then you should have your user verify the certificate fingerprint. When sslserve starts up it shows the MD5 and SHA-1 fingerprints for the current certificate. This should be checked against what the user sees in their browser. You probably have to give them the username and password anyway, so this can be done at the same time.

Here is an example fingerprint:

    ==============================================================================
     SHA-1 Certificate fingerprint:
     46:A5:A2:7C:00:F8:93:6A:7E:2E:E0:07:C7:AE:53:63:B8:94:AB:94
     MD5 Certificate fingerprint:
     35:6D:9E:02:BB:65:AC:FB:97:94:74:E6:FD:2E:DE:73

     Certificate expires at 2013-09-24 10:16:44 +0100

     Further information: https://github.com/zeroXten/sslserve
    ==============================================================================

Beyond Stupid
=============

So, last night I had added in the HTTP basic auth to sslserve. It originally took a username and password as it does now. I then realised that most people probably wouldn't bother with the fingerprint verification, because we humans are naturally lazy. It's too easy to think that the connection would be secure "on this occasion". Trying to be clever, I wanted a way to make the fingerprint verification both necessary and easy. Now, given sslserve only supports one username at a time, the username isn't really that meaningful. The "clever" idea was to reuse the username and use it for the fingerprint verification. The user could look up the fingerprint in the browser, then provide along with the password and sslserve would check against its own fingerprint and confirm they were the same.

I'll let you think for a second about why this idea is so amazing stupid....





Hopefully you've spotted the flaw. I was trying to verify the connection, using the connection. The whole point is that the connection is untrusted, so it cannot be trusted to transport the fingerprint and password. If someone was sitting in the middle, they could just lookup the correct fingerprint and send it instead of the what the user provided. Any the were given the password. It added no security whatsoever. The fingerprint verification has to be done through a second, trusted channel such as a phone call or perhaps an email.

From a security engineering and psychological perspective, this is in my opinion a nice little example of why security is "hard". Focus shifted to making fingerprint verification easy, so it would actually be used. But that was at the cost of the actual security it was trying to implement. Obviously it didn't take me too long to suddenly realise how dumb the approach was and I fixed it immediately. I think the same sort of thing happens at much larger scales, in major software and platforms and at a slower speed. 

The lesson learnt is to think slowly, carefully and critically about the security implication of all new features, and not to be blinded by the feature's shininess.

See also
========

* [http://rubygems.org/gems/serve](http://rubygems.org/gems/serve)
