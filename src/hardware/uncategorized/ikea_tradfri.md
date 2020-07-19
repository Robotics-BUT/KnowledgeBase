# IKEA Tradfri

## IKEA Tradfri + Phillips Hue + HomeKit

Both the Tradfri and Hue use ZigBee protocol to operate and also implement some standardized data interface. That means that a Hue bridge can be used to control Tradfri lightbulbs and read data from other sensors.

To connect a lightbulb to the bridge, first in the app it is required to start search for lights. Then the lightbulb needs to be turned off and on 6 times leaving it in connection mode (the bulb changes its brightness in three steps).

The bridge should then identify the bulb and pair with it.

> When the connection mode is activated a new search for lightbulbs in the app can be performed.

After the pairing finishes, the lightbulb can be easily used with the Hue app, unfortunately the app **won't publish** the 3rd party accessories to HomeKit.

There are two solutions to this problem - either buy the IKEA Tradfri bridge and use it to publish the accessories to the HomeKit, or use a home server running Homebridge with appropriate plugins.

The Homebridge server can be installed using the tutorial [here](https://github.com/homebridge/homebridge/wiki) and can then be used to control devices from many more manufacturers that do not implement HomeKit on their own.

To make the devices connected to the Hue bridge work, a plugin [homebridge-hue](https://github.com/ebaauw/homebridge-hue) needs to be installed and configured with the following configuration, that ensures that the plugin won't advertise native Hue devices, but only the third party one.

```json
...
"platforms": [
        ...
        {
            "name": "Hue-tradfri",
            "anyOn": true,
            "lights": true,
            "nativeHomeKitLights": true,
            "nativeHomeKitSensors": true,
            "nupnp": true,
            "resource": true,
            "users": {
                "USER_ID": "TOKEN"
            },
            "platform": "Hue"
        }
        ...
    ]
...
```

> Bear in mind, that after installing the plugin and making it connect to the bridge it is required to save the `USER_ID` and `TOKEN` to the configuration file.