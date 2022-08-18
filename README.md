## Description
PoC MQTT Bridge Architecture using HiveMQ

## Installation steps

### STEP01

Start proxy mqtt services
```
docker run --name hivemq-proxy -p 8080:8080 -p 1883:1883 hivemq/hivemq4
```

Login in proxy services
```
docker exec -it hivemq-proxy bash
```

Inside configure the bridge extension from `/opt/hivemq-4.8.3/extensions/hivemq-bridge-extension\bridge-configuration.xml`

The configuration would be:
```
<?xml version="1.0" encoding="UTF-8"?>
<hivemq-bridge-extension xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
                         xsi:noNamespaceSchemaLocation="bridge-configuration.xsd">
    <bridges>
        <bridge>
            <name>proxy-bridge</name>
            <remote-broker>
                <connection>
                    <static>
                        <host>hivemq-cloud</host>
                        <port>1884</port>
                    </static>
                </connection>
            </remote-broker>
            <topics>
                <topic>
                    <filter>+/event</filter>
                </topic>
            </topics>
        </bridge>
    </bridges>
</hivemq-bridge-extension>
```

### STEP02

Start cloud mqtt services
```
docker run --name hivemq -p 8081:8081 -p 1884:1884 hivemq/hivemq4
```

Login in proxy services
```
docker exec -it hivemq-proxy bash
```

Inside configure the port HiveMQ from `/opt/hivemq-4.8.3/conf/config.xml`

Change the default port from 1883 to 1884
```
<listeners>
        <tcp-listener>
            <port>1884</port>
            <bind-address>0.0.0.0</bind-address>
        </tcp-listener>
        ...
</listeners>
```
