# homebridge-airvisual

[![NPM Version](https://img.shields.io/npm/v/homebridge-airvisual.svg)](https://www.npmjs.com/package/homebridge-airvisual)

Homebridge plugin for the AirVisual API which allows access to outdoor air quality, humidity, and temperature.

## Installation

1. Install homebridge using the instructions at https://github.com/nfarina/homebridge#installation
2. Install this plugin using: `npm install -g homebridge-airvisual`
3. Register for an account and get an API key at https://www.airvisual.com/api
4. Update the Homebridge configuration file

## Configuration

Example config.json:

```js
"accessories": [
  {
    "accessory": "AirVisual",
    "name": "AirVisual",
    "api_key": "",
    "sensor": "air_quality",
    "aqi_standard": "us",
    "latitude": ,
    "longitude": ,
    "city": "",
    "state": "",
    "country": "",
    "ppb_units": ["no2", "o3", "so2"],
    "polling": false,
    "https": true
  }
],
```

## Details

Field | Required | Default | Description
:--- | :---: | :---: | :---
`accessory` | yes | `AirVisual` | Must always be `AirVisual`
`name` | yes | `AirVisual` | Can be specified by user
`api_key` | yes | | Obtain from https://www.airvisual.com/api
`sensor` | no | `air_quality` | Must be `air_quality`, `humidity`, or `temperature`
`aqi_standard` | no | `us` | Only applicable if `sensor` is set to `air_quality`, must be `cn` (for China) or `us` (for United States) 
`latitude` | no | | See [**Location**](#location) notes below
`longitude` | no | | See [**Location**](#location) notes below
`city` | no | | See [**Location**](#location) notes below
`state` | no | | See [**Location**](#location) notes below
`country` | no | | See [**Location**](#location) notes below
`ppb_units` | no | | See [**Units**](#units) notes below
`polling` | no | `false` | Must be `true` or `false` (must be a boolean, not a string)
`https` | no | `true` | Must be `true` or `false` (must be a boolean, not a string)

## Location

By default, AirVisual will use IP geolocation to determine the nearest station to get data from (no configuration needed).

Alternatively, GPS coordinates (`latitude` and `longitude`) or a specific city (`city`, `state`, and `country`) can be used.

* GPS coordinates can be found using https://www.latlong.net

  * Coordinates must be entered as numbers, not strings

* A specific city, state, and country can be found using https://www.airvisual.com/world

  * Browse to the desired city

  * Format will be shown as *City > State > Country*

* If both `latitude`, `longitude` and `city`, `state`, `country` are specified; the GPS coordinates will be used.

## Units

If a "Startup" or "Enterprise" API key is used, then AirVisual should return concentration for individual pollutants in units of µg/m3. However, various locations appear to report some pollutants in units of ppb.

These pollutants can be converted to µg/m3, which is required for HomeKit, with the following steps:

1. Use the AirVisual app or website to see the reported units of each pollutant for the desired location.

2. Then use the `ppb_units` configuration option to indicate which pollutants should be converted from ppb to µg/m3.

Only `no2`, `o3`, and `so2` are supported for conversion.

## Miscellaneous

* Homebridge supports multiple instances for accessories; the configuration entry can be duplicated for each location and/or sensor type desired.

* This plugin supports additional characteristics for air quality sensors if a "Startup" or "Enterprise" API key from AirVisual is used.
