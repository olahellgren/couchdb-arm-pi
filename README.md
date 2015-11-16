# couchdb-arm-pi

Instructions from: http://www.fanciullimassimiliano.it/2015/02/01/coucbdb-on-a-raspberrypi/

## DON'T DO THIS, YOU'LL TOO NEW ERLANG ##
Edit the apt servers list:
`sudo nano /etc/apt/sources.list`

Add the following line at the end of the file:
`deb http://packages.erlang-solutions.com/debian wheezy contrib`

Add the Erlang Solutions public key for apt-secure using following commands:
`wget http://packages.erlang-solutions.com/debian/erlang_solutions.asc`
`sudo apt-key add erlang_solutions.asc`

Update the aptitude repository by running:
`sudo apt-get update`

## CONTINUE AFTER THIS ##

After updating start installing all needed dependencies:

`sudo apt-get install erlang-nox erlang-dev libmozjs185-1.0 libmozjs185-dev libcurl4-openssl-dev libicu-dev`

Now create the couchDB account by executing:
```
sudo useradd -d /var/lib/couchdb couchdb
sudo mkdir -p /usr/local/{lib,etc}/couchdb /usr/local/var/{lib,log,run}/couchdb /var/lib/couchdb
sudo chown -R couchdb:couchdb /usr/local/{lib,etc}/couchdb /usr/local/var/{lib,log,run}/couchdb
sudo chmod -R g+rw /usr/local/{lib,etc}/couchdb /usr/local/var/{lib,log,run}/couchdb
```

All the dependencies are now set. Download the source code and unpack it:

`wget http://ftp-stud.hs-esslingen.de/pub/Mirrors/ftp.apache.org/dist/couchdb/source/1.6.0/apache-couchdb-1.6.0.tar.gz tar xzf apache-couchdb-*.tar.gz`

Move inside the unpacked code and configure the package:
```
cd apache-couchdb-1.6.0
./configure —-prefix=/usr/local —-with-js-lib=/usr/lib —-with-js-include=/usr/include/js —-enable-init
```

When finished it is time to build and install CouchDB:

`make && sudo make install`

It will take some time to build the whole package. When finished set it as daemon by running:
```
sudo chown couchdb:couchdb /usr/local/etc/couchdb/local.ini
sudo ln -s /usr/local/etc/init.d/couchdb /etc/init.d/couchdb
sudo /etc/init.d/couchdb start
sudo update-rc.d couchdb defaults
```

By default CouchDB is bound at http://localhost:5984. To test if everything works properly you can run:

`curl http://127.0.0.1:5984/`

If you see an output like the following everything is set properly:
```
{“couchdb”:”Welcome”,”uuid”:”dc91fe432758b49a0d4708cbfcffaedb”,”version”:”1.6.0”,”vendor”:{“name”:”The Apache Software Foundation”,”version”:”1.6.0”}}
```
