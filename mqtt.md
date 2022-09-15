# MQTT

## MQTT

Using Nodejs: 0. Install MQTT.js globally: npm install -g mqtt

1. In one terminal, subscribe to your topic: mqtt sub -t 'hello' -h 'test.mosquitto.org' -v
2. In another, publish to your topic: mqtt pub -t 'hello' -h 'test.mosquitto.org' -m 'from your MQTT.js instant'

Using Docker: 0. Need docker installed. 0. Docker image 'efrecon/mqtt-client' will be downloaded if not existed. 0. The successive runs would be much faster (because of not downloading anything).

1. In one terminal, subscribe to your topic: docker run -it --rm --name mqtt-subscriber efrecon/mqtt-client sub -h test.mosquitto.org -t 'hello' -v
2. In another, publish to your topic: docker run -it --rm --name mqtt-publisher efrecon/mqtt-client pub -h test.mosquitto.org -t 'hello' -m 'from your mqtt-client on docker'



## On Linux

### Installing

#### Install MQTT Broker

```
sudo apt install mosquitto
```

#### Install MQTT Client

```
 sudo apt install mosquitto-clients
```

### Using

#### Subscribing

```
mosquitto_sub -t "test"
```

#### Publishing

```
mosquitto_pub -t "test" -m "message from mosquitto_pub client"
```

## Configure Mosquitto

```
# /etc/mosquitto
allow_anonymous true
listener 1883
```
