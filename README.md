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
        -d, --dir DIR                    Directory to serve files from. Defaults to pwd
        -l, --listen IP                  IP address bind to. Defaults to 127.0.0.1
        -p, --port PORT                  PORT to listen on. Defaults to 3443
        -n, --name NAME                  NAME for SSL certificate. Defaults to localhost
        -e, --expires HOURS              Certificate expires in HOURS hours. Defaults to
        -h, --help                       Show this message
        -v, --version                    Show version
