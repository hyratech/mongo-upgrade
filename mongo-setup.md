# Mongodb Upgrade Guide from 3.6 to 5.0

## Befor upgrade task
* Make sure that server is updated
```
sudo apt update
sudo apt list --upgradable
# if nothing critical is updating, perform update
sudo apt upgrade
```


## Upgrade from 3.6 to 4.0
https://docs.mongodb.com/manual/release-notes/4.0-upgrade-replica-set/

* Connect to primary and check if protocol version is set to 1, if not set it
```bash
mongo -host 127.0.0.1 -u "admin" -p "MONGOPASS" --authenticationDatabase "admin"
```
in mongo shell run
```js
cfg = rs.conf();
cfg.protocolVersion=1;
rs.reconfig(cfg);
```

* Connect to each secondary and execute, in addition secondary can be removed from cluster before upgrading
```bash
sudo service mongod stop
wget -qO - https://www.mongodb.org/static/pgp/server-4.0.asc | sudo apt-key add -
echo "deb [ arch=amd64,arm64 ] https://repo.mongodb.org/apt/ubuntu bionic/mongodb-org/4.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-4.0.list
sudo apt update
# sudo apt-get install mongodb-org
sudo apt upgrade
sudo service mongod start
```

* Connect to primary
```bash
mongo -host 127.0.0.1 -u "admin" -p "MONGOPASS" --authenticationDatabase "admin"
```
in mongo shell run
```javascript
rs.stepDown();
```
* Upgrade it like any other secondary using command given above
```bash
sudo service mongod stop
wget -qO - https://www.mongodb.org/static/pgp/server-4.0.asc | sudo apt-key add -
echo "deb [ arch=amd64,arm64 ] https://repo.mongodb.org/apt/ubuntu bionic/mongodb-org/4.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-4.0.list
sudo apt update
# sudo apt-get install mongodb-org
sudo apt upgrade
sudo service mongod start
```

-----------
-----------

## Upgrade from 4.0 to 4.2
https://docs.mongodb.com/manual/release-notes/4.2-upgrade-replica-set/
* Connect to primary 
```bash
mongo -host 127.0.0.1 -u "admin" -p "MONGOPASS" --authenticationDatabase "admin"
```
in mongo shell run
```js
db.adminCommand( { getParameter: 1, featureCompatibilityVersion: 1 } )
// Update to 4.0 if 3.6
db.adminCommand( { setFeatureCompatibilityVersion: "4.0" } )
```

* Connect to each secondary and execute, in addition secondary can be removed from cluster before upgrading

```bash
sudo service mongod stop
wget -qO - https://www.mongodb.org/static/pgp/server-4.2.asc | sudo apt-key add -
echo "deb [ arch=amd64,arm64 ] https://repo.mongodb.org/apt/ubuntu bionic/mongodb-org/4.2 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-4.2.list
sudo apt-get update
sudo apt list --upgradable
sudo apt upgrade
sudo service mongod start
```

* Connect to primary
```bash
mongo -host 127.0.0.1 -u "admin" -p "MONGOPASS" --authenticationDatabase "admin"
```
in mongo shell run
```javascript
rs.stepDown();
```
* Upgrade it like any other secondary using command given above
```bash
sudo service mongod stop
wget -qO - https://www.mongodb.org/static/pgp/server-4.2.asc | sudo apt-key add -
echo "deb [ arch=amd64,arm64 ] https://repo.mongodb.org/apt/ubuntu bionic/mongodb-org/4.2 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-4.2.list
sudo apt-get update
sudo apt list --upgradable
sudo apt upgrade
sudo service mongod start
```
------------
------------

## Upgrade from 4.2 to 4.4
https://docs.mongodb.com/v4.4/release-notes/4.4-upgrade-replica-set/
* Connect to primary 
```bash
mongo -host 127.0.0.1 -u "admin" -p "MONGOPASS" --authenticationDatabase "admin"
```
in mongo shell run
```js
db.adminCommand( { getParameter: 1, featureCompatibilityVersion: 1 } )
// Update to 4.2 if 4.0
db.adminCommand( { setFeatureCompatibilityVersion: "4.2" } )
```


* Connect to each secondary and execute, in addition secondary can be removed from cluster before upgrading
```bash
# connect to mongo and shutdown
mongo
```

```javascript
rs.secondaryOk();
use admin;
db.auth("admin","MONGOPASS");
db.shutdownServer()
```
Perform upgrade
```bash
wget -qO - https://www.mongodb.org/static/pgp/server-4.4.asc | sudo apt-key add -

echo "deb [ arch=amd64,arm64 ] https://repo.mongodb.org/apt/ubuntu focal/mongodb-org/4.4 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-4.4.list
sudo apt-get update
sudo apt list --upgradable
# Command will give warning due to split of mongo packages
sudo apt -y upgrade

sudo apt --fix-broken install
sudo service mongod start
```

* Connect to primary
```bash
mongo -host 127.0.0.1 -u "admin" -p "MONGOPASS" --authenticationDatabase "admin"
```
in mongo shell run
```javascript
rs.stepDown();
exit

mongo
rs.secondaryOk();
use admin;
db.auth("admin","MONGOPASS");
db.shutdownServer()
```
* Upgrade it like any other secondary using command given above
```bash
wget -qO - https://www.mongodb.org/static/pgp/server-4.4.asc | sudo apt-key add -

echo "deb [ arch=amd64,arm64 ] https://repo.mongodb.org/apt/ubuntu focal/mongodb-org/4.4 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-4.4.list
sudo apt-get update
sudo apt list --upgradable
# Command will give warning due to split of mongo packages
sudo apt -y upgrade

sudo apt --fix-broken install
sudo service mongod start
```
------------
------------

## Upgrade from 4.4 to 5.0
https://docs.mongodb.com/v5.0/release-notes/5.0-upgrade-replica-set/
* Connect to primary 
```bash
mongo -host 127.0.0.1 -u "admin" -p "MONGOPASS" --authenticationDatabase "admin"
```
in mongo shell run
```js
db.adminCommand( { getParameter: 1, featureCompatibilityVersion: 1 } )
// Update to 4.4 if 4.2
db.adminCommand( { setFeatureCompatibilityVersion: "4.4" } )
```


* Connect to each secondary and execute, in addition secondary can be removed from cluster before upgrading
```bash
# connect to mongo and shutdown
mongo
```

```javascript
rs.secondaryOk();
use admin;
db.auth("admin","MONGOPASS");
db.shutdownServer()
```
Perform upgrade
```bash
wget -qO - https://www.mongodb.org/static/pgp/server-5.0.asc | sudo apt-key add -
echo "deb [ arch=amd64,arm64 ] https://repo.mongodb.org/apt/ubuntu focal/mongodb-org/5.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-5.0.list
sudo apt-get update
sudo apt list --upgradable
sudo apt upgrade
sudo service mongod start
```

* Connect to primary
```bash
mongo -host 127.0.0.1 -u "admin" -p "MONGOPASS" --authenticationDatabase "admin"
```
in mongo shell run
```javascript
rs.stepDown();
exit

mongo
rs.secondaryOk();
use admin;
db.auth("admin","MONGOPASS");
db.shutdownServer()
```
* Upgrade it like any other secondary using command given above
```bash
wget -qO - https://www.mongodb.org/static/pgp/server-5.0.asc | sudo apt-key add -
echo "deb [ arch=amd64,arm64 ] https://repo.mongodb.org/apt/ubuntu focal/mongodb-org/5.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-5.0.list
sudo apt-get update
sudo apt list --upgradable
sudo apt upgrade
sudo service mongod start
```

* Set feature compatbility, Connect to primary 
```bash
mongo -host 127.0.0.1 -u "admin" -p "MONGOPASS" --authenticationDatabase "admin"
```
in mongo shell run
```js
db.adminCommand( { getParameter: 1, featureCompatibilityVersion: 1 } )
db.adminCommand( { setFeatureCompatibilityVersion: "5.0" } )
```
---------