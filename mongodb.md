# MongoDB

## Install

[https://docs.mongodb.com/manual/tutorial/install-mongodb-on-ubuntu/](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-ubuntu/)

```
wget -qO - https://www.mongodb.org/static/pgp/server-5.0.asc | sudo apt-key add -
```

```
echo "deb [ arch=amd64,arm64 ] https://repo.mongodb.org/apt/ubuntu focal/mongodb-org/5.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-5.0.list
```

```
sudo apt-get update
sudo apt-get install -y mongodb-org
```

```
sudo systemctl enable mongod
sudo systemctl start mongod
```

## CLI

```
mongosh
```

## GUI

[https://www.mongodb.com/try/download/compass](https://www.mongodb.com/try/download/compass)

## Others

### Fixes after deleting

```
sudo chown mongodb:mongodb /var/log/mongodb/mongod.log
sudo chown -R mongodb:mongodb /var/lib/mongodb
```

### Directories

data directory: `/var/lib/mongodb`

log directory: `/var/log/mongodb`

### See change log

```
sudo tail -f /var/log/mongodb/mongod.log
```

## Connect to MongoDB Atlas

#### Connect with the MongoDB Shell

mongosh "mongodb+srv://cluster0.l5brt.mongodb.net/myFirstDatabase" --username minhtaile2712

#### Connect your application

mongodb+srv://minhtaile2712:\<password>@cluster0.l5brt.mongodb.net/myFirstDatabase?retryWrites=true\&w=majority

#### Connect using MongoDB Compass

mongodb+srv://minhtaile2712:2mRWZWDvExvgGrEc@cluster0.l5brt.mongodb.net

## Uninstall

Stop MongoDB

```bash
sudo service mongod stop
```

Remove packages

```bash
sudo apt purge mongodb-org*
```

Remove data and log

```bash
sudo rm -r /var/log/mongodb /var/lib/mongodb
```

List key

```bash
sudo apt-key list
```

Remove key

```bash
sudo apt-key del E2C63C11
```

Key name is the last two blocks out of ten block

Remove list file

```bash
sudo rm /etc/apt/sources.list.d/mongodb-org-5.0.list
```
