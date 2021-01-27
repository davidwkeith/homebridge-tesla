# Tesla Accessory

Example config.json:

```json
{
  "accessories": [
    {
      "accessory": "Tesla",
      "name": "Model 3",
      "vin": "5JJYCB522AB296261",
      "username": "bobs@burgers.com",
      "password": "bobbobbaran",
      "authToken": "authToken",
      "refreshToken": "refreshToken",
      "waitMinutes": 1
    }
  ]
}
```

Exposes lock services for doors, trunk, and front trunk. Also exposes an on/off switch for climate control, charge state, and keyless driving.

_If_ you define a value for `waitMinutes`, you can control the amount of
time the plugin will wait for the car to wake up. The default is one minute.

_If_ you define a value for `authToken` and `refreshToken`,
you do not need to provide your username or password credentials.
Generating a token can be done many ways.
[Tokens for Teslas](https://tokens-for-teslas.herokuapp.com) is one option.
(Does not currently generate a refresh token, so will need to re-auth in 90 days)
`npm install -g generate-tesla-token` is another, which fully supports refresh tokens.

If you use the example above, you would gain Siri commands like:

- _"Unlock the Model 3 Doors"_ (unlock the vehicle)
- _"Open the Model 3 Trunk"_ (pop the trunk)
- _"Open the Model 3 Front Trunk"_ (pop the frunk)
- _"Open the Model 3 Charge Port"_ (open the charging port)
- _"Turn on the Model 3 Climate"_ (turn on climate control)
- _"Turn on the Model 3 Charger"_ (begin charging even if outside your schedule)
- _"Turn on the Model 3 Starter"_ (enable keyless driving for 2 minutes - requires `password` in config, `authToken` is not sufficient)
- _"Turn on the Model 3 Connection"_ (wake the car up so you can ask if it's locked, etc.)

**Important Note**: The Home app will allow you to customize the default names of these services. You may be tempted to, for instance, change your "Model 3 Front Trunk" service to just "Front Trunk" so you can say "Open the Front Trunk". Don't do this! The names of these services are essentially _global_ and live in a giant pool of names. Siri will get confused unless every service has an easily distinguished name.

You may disable various services using additional boolean configuration settings. See `config.schema.json` for more information.

## Multiple Vehicles

Have a garage full of Teslas? Well you're in luck Mr. Musk, because you can
easily add all of them to HomeKit by creating a separate accessory for each one
distinguished by their unique VIN numbers:

    {
      "accessories": [
        {
          "accessory": "Tesla",
          "name": "Model 3",
          "vin": "5JJYCB522AB296261",
          "username": "bobs@burgers.com",
          "password": "bobbobbaran"
        },
        {
          "accessory": "Tesla",
          "name": "Model X",
          "vin": "1XSYCA2224A216162",
          "username": "bobs@burgers.com",
          "password": "bobbobbaran"
        }
      ]
    }

## HomeLink

For vehicles with HomeLink support, the plugin allows you to enable the feature to send a HomeLink signal from the car. This is disabled by default. To enable the feature, you may use the UI (check the Enable HomeLink Service to enable it) or add a setting in the accessory section for the vehicle. You also must provide a latitude and longitude value for the HomeLink device.

Once that is done, you can issue commands like "Open the Model 3 HomeLink". If you don't have any other garage doors in HomeKit, you may also be able to just say "Open the garage door" since it's exposed as a true garage door service.

    {
      "accessories": [
        {
          "accessory": "Tesla",
          "name": "Model 3",
          "vin": "5JJYCB522AB296261",
          "username": "bobs@burgers.com",
          "password": "bobbobbaran"
          "enableHomeLink": true,
          "latitude": "37.492655",
          "longitude": "-121.944644"
        }
      ]
    }

## Charge Level

The car can supply the current charge level of the battery as a percentage. This will only update if the car is awake. Asking Siri about your car's charge level will not wake up the car (because Siri asks a lot in the background). So you'll have to issue a write command like "Turn on the Model 3 Connection" first.

The Charge Level can be disabled in either your Homebridge UI or by adding the following to the car's accessory section.

> "disableChargeLevel": true

## Development

You can run Rollup in watch mode to automatically transpile code as you write it:

```sh
  npm run dev
```
