Make sure these boxes are checked before submitting your issue -- thank you!


- [ ] You are using PHP > 5.6
- [ ] You have installed all extensions and modules listed in [here]
Dependencies
PHP >= 5.6 is required

To support encryption, API requires (MUST):

PHP Protobuf
Curve25519 PHP
crypto PECL
Installation for Linux and OS X users

phpize
./configure
make
sudo make install
Pre-Compiled extensions (Curve25519 and protobuf) for Windows users:

Curve25519
PHP Protobuf
Dependencies:

ffmpeg, openssl, gd, curl, sqlite PDO, sockets, mcrypt

For OS X users (using ports):

sudo port install ffmpeg
sudo port install php56-openssl
sudo port install php56-gd
sudo port install php56-curl
sudo port install php56-sockets
sudo port install php56-sqlite
sudo port install php56-mcrypt
For Linux users:

sudo apt-get install ffmpeg
...
Docker
https://github.com/Shaked/docker-whatsapp

If the repo is not available use this one:

https://github.com/mgp25/docker-whatsapp

Note: No support for Windows users yet

Thanks to all the community for making this possible, we'll keep with the good work :)


- [ ] You are using latest Chat API code.


### Error
Post your error below:


### Debug log
Post your debug log below:

```xml
YOUR DEBUG LOG HERE
```
