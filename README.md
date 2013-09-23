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
        -a, --auth PASSWORD              PASSWORD for HTTP Basic Auth. Defaults to disabled. Username is the SHA1 certificate fingerprint
        -r, --realm REALM                HTTP Basic Auth REALM. Defaults to sslserve realm
        -h, --help                       Show this message
        -v, --version                    Show version


Basic Auth
==========

You can turn on HTTP basic authentication using the --auth (-a) option.

The password is supplied to the --auth option.

The username is case-insensitive MD5 certificate fingerprint with spaces and/or colons removed automatically. For example, the following four are all the same, valid usernames:

* 36:40:1E:14:FC:86:7E:D6:0C:40:E3:D8:8D:75:8B:C2
* 36 40 1E 14 FC 86 7E D6 0C 40 E3 D8 8D 75 8B C2
* 36401E14FC867ED60C40E3D88D758BC2
* 36401e14fc867ed60c40e3d88d758bc2

If you're going to use HTTP basic auth, it's probably because you want some level of security (otherwise you could just use the serve gem). So, you want a username and password to authenticate the user and protect the content, and you want SSL (TLS) to encrypt the traffic to stop people from snooping. With basic auth, the username and password are basically sent in the clear, so it is especially important to protect that data.

Luckily, sslserve provides the SSL in a super easy to use way. However, it uses self-signed certificates. This means that if an attacker managed to man-in-the-middle your connection, they could just swap out the certificate for one they control and your end user would ever know. So, we combat that using a fingerprint of the certificate.

When you run sslserve, it prints out the MD5 and SHA-1 fingerprints, like so:

    ==============================================================================
     SHA-1 Certificate fingerprint:
     5D:B0:FA:5C:F0:9B:C0:98:99:AF:81:DC:66:B5:1F:EE:93:BE:49:06
     MD5 Certificate fingerprint:
     73:F7:EC:62:A0:7E:CC:BF:93:A0:B9:DD:FC:49:FF:86
    
     Certificate expires at 2013-09-23 21:49:49 +0100
    
     Basic auth realm: 'sslserve realm'
     Basic auth user:  73F7EC62A07ECCBF93A0B9DDFC49FF86
     Basic auth pass:  'password'

     Further information: https://github.com/zeroXten/sslserve
    ==============================================================================

You could telephone the person that needs access to your data, and check the fingerprint they see with the one printed to the screen. But, you've got to give them the username and password anyway, so we may as well re-use the username and automatically verify the fingerprint from that. All you need to do is give them the password.

See also
========

* [http://rubygems.org/gems/serve](http://rubygems.org/gems/serve)
