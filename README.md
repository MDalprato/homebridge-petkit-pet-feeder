## homebridge-petkit-pet-feeder

<p align="center">
  <img src="https://raw.githubusercontent.com/jubepue/homebridge-petkit-pet-feeder/master/images/petkit-pet-feeder.jpg">
  <br>
  <a href="https://www.npmjs.com/package/homebridge-petkit-pet-feeder">
    <img src="https://flat.badgen.net/npm/v/homebridge-petkit-pet-feeder" alt="NPM Version" />
  </a>
  <a href="https://www.npmjs.com/package/homebridge-petkit-pet-feeder">
    <img src="https://flat.badgen.net/npm/dt/homebridge-petkit-pet-feeder" alt="Total NPM Downloads" />
  </a>
  <a href="https://github.com/homebridge/homebridge/wiki/Verified-Plugins">
    <img src="https://badgen.net/badge/homebridge/verified/purple" alt="verified-by-homebridge" />
  </a>
  <br>
  <strong><a href="#2-how-to-setup">Setup Guide</a> | <a href="#4-how-to-contribute">Contribute</a> </strong>
</p>

## DEPRECATED. You can find the new plugin that replaces it in: <a href="https://github.com/jubepue/homebridge-petkit-platform">homebridge-petkit-platform</a>

## 1) Description

control your petkit pet feeder from homekit, get full use of iOS automation.

<p align="center">
  <img src="https://raw.githubusercontent.com/jubepue/homebridge-petkit-pet-feeder/master/images/screenshot.jpeg" alt="screenshot" /><img src="https://raw.githubusercontent.com/jubepue/homebridge-petkit-pet-feeder/master/images/screenshot2.jpeg" alt="screenshot2" />
  <br>
  <img src="https://raw.githubusercontent.com/jubepue/homebridge-petkit-pet-feeder/master/images/screenshot3.jpeg" alt="screenshot3" /><img src="https://raw.githubusercontent.com/jubepue/homebridge-petkit-pet-feeder/master/images/screenshot4.jpeg" alt="screenshot4" />
</p>



### features

- uses a fan speed to control the meal amount;
- uses a switch to commit the drop;
- uses a switch to control the Petkit pet feeder light mode;
- uses a switch to control the Petkit pet feeder manual lock;
- uses a occupancy to indicate food storage status;
- uses a filter maintenance to indicate desiccant status(include auto reset desiccant left days, this may not show in homekit);
- uses a battery service to indicate device power status(include power level, charging status and low battery alert in Homekit)
- can fetch device info from Petkit server and shows in Homekit.



### limitations

- currently this plugin for <a href="https://github.com/homebridge/homebridge">homebridge</a> just tested with <a href="https://petkit.com/products/fresh-element-solo/">Petkit feeder Element SOLO (official store link)</a>, and this plugin currently only tested and works in Asia(include China mainland), United States and Europe, other area may need more work.
- to continuously use this plugin you should login Petkit app and never logoff, this plugin uses session id from the app and it will change every time you logoff and relogin.
- because version >= 2.x.x is developed on a dynamic platform plugin, and not implement auto remove deleted device(s), so you may need to delete cached accessories manually.
- ......




## 2) How to setup

### Firstly, setup your Petkit mobile app.

Goto App Store, download Petkit mobile app, register, login, add device. before use this plugin you <strong>MUST</strong> make sure the app works fine with you.

- for China mainland users, download “小佩宠物”
- for those Asia users outside China mainland, you should download "PetKit(International)"
- for users not from Asia, like United States, Europe, etc. you also should download "PetKit(International)", but the API url address may not the same, this plugin may or may not works with you, if you are experienced in network, you could use "Quantumult X" to capture app network records, submit the api address to me or just create a PR or just modify this plugin for your own.



### Secondly, prepare to capture Petkit app http request.

Please goto <a href="https://github.com/jubepue/homebridge-petkit-pet-feeder/wiki#how-to-capture-http-netflow">this</a> wiki page to find more detail.

### Finally, retrieve infomation.

you should provide one critical infomation to this plugin: **X-Session**, if you have more than one Petkit feeder mini device, then you should alse provide **deviceId** in the header field of the config.json file.

Be aware of that, to minimize the effect to the Petkit server with unnecessary http requests, the plugin just update device status more than 10s interval, which means the status will bufferd at lease 10s. 

- X-Session: this value will change every time you login you Petkit app, so do not logoff your Petkit app unless necessary. 
- deviceId: this value indicate which device you wanna to control. If you just have one Petkit feeder device, you can ignore this value.

here is a example of Quantumult X capture data page:

<p align="center"><img src="https://raw.githubusercontent.com/jubepue/homebridge-petkit-pet-feeder/master/images/quantumultX.jpg" alt="quantumultX" /></p>
you can find X-Session data from the request header area and deviceId in response data area.



## 3) Configure

### config.json fields

| field   name |  type  | required |       default        |        range         | description                                                  |
| :----------: | :----: | :------: | :------------------: | :------------------: | :----------------------------------------------------------- |
|   platform   | string |   yes    | 'petkit_pet_feeder'  | 'petkit_pet_feeder'  | Must be 'petkit_pet_feeder' in order to use this plugin.     |
|  log_level   |  int   |    no    |          2           |      1,2,3,4,9       | one of these values:<br/>- 1: Debug<br/>- 2: Info<br/>- 3: Warn<br/>- 4: Error<br/>- 9: None |
|   devices    | object |   yes    |         ---          |         ---          | Petkit Feeder device config.<br/>See more detail info at <a href="#devices-field">devices field</a> below. |



### devices field

|           field   name            |  type  | required |                  default                   |                range                | description                                                  |
| :-------------------------------: | :----: | :------: | :----------------------------------------: | :---------------------------------: | ------------------------------------------------------------ |
|             location              | string |   yes    |               'international'                    | 'cn',<br>'asia',<br>'international' | China users:'cn';<br>Asia users: 'asia';<br>International (United States/Europe) users: 'international';<br>other location because lack of infomation, not sure it will work.<br/>You can find more info <a href="https://github.com/jubepue/homebridge-petkit-pet-feeder/wiki/How-to-choose-server-location">here</a>. |
|               model               | string |    no    |                'D4'                        |      'FeederMini',<br>'Feeder',<br>'D4',<br>'D3' | Petkit Feeder Mini: 'FeederMini'<br>Petkit Feeder Element: 'Feeder'<br>Petkit Feeder Element SOLO: 'D4'<br>Petkit Feeder Element Infiniti: 'D3' |
|             deviceId              | string |   tbd    |                    ---                     |                 ---                 | your Petkit feeder element SOLO Id, which is buildin your device, will never change. <br/>If you just have one Petkit feeder device, you can ignore this value.<br/>If you just have more than one Petkit Feeder device, you must set this value. |
|              headers              | array  |   yes    |                    ---                     |                 ---                 | http request headers.<br/>See more detail info at <a href="#headers-field">headers field</a> below. |
|         enable_http_retry         |        |    no    |                   false                    |             true/false              | Enable or disable HTTP retry function, useful when your device or homebridge has a bad internet connection. |
|         http_retry_count          |  int   |    no    |                     3                      |               1 to 5                | max retry times when a http request failed.                  |
|           DropMeal_name           | string |    no    |                 'DropMeal'                 |                 ---                 | name of DropMeal switch in HomeKit.                          |
|          MealAmount_name          | string |    no    |                'MealAmount'                |                 ---                 | name of MealAmount fan speed in HomeKit.                     |
|         FoodStorage_name          | string |    no    | 'FoodStorage_Empty'<br>or<br>'FoodStorage' |                 ---                 | name of FoodStorage indicator in HomeKit.<br>Note: if reverse_foodStorage_indicator value is set to true, then the default name is 'FoodStorage_Empty', otherwise 'FoodStorage' |
|        DesiccantLevel_name        | string |    no    |              'DesiccantLevel'              |                 ---                 | name of DesiccantLevel indicator in HomeKit.                 |
|          ManualLock_name          | string |    no    |                'ManualLock'                |                 ---                 | name of ManualLock switch in HomeKit.                        |
|          LightMode_name           | string |    no    |                'LightMode'                 |                 ---                 | name of LightMode switch in HomeKit.                         |
|           Battery_name            | string |    no    |                 'Battery'                  |                 ---                 | name of Battery indicator in HomeKit.                        |
|           BlockDoor_name          | string |    no    |                 'BlockDoor'                |                 ---                 | name of BlockDoor indicator in HomeKit.                      |
|          enable_polling           |  bool  |    no    |                    true                    |             true/false              | Automatically update device info from Petkit server.         |
|         polling_interval          |  int   |    no    |                     60                     |             60 to 3600              | update device info interval from Petkit server(s).           |
|         enable_manualLock         |  bool  |    no    |                   false                    |             true/false              | if enabled, a switch will show in homekit to control the manual lock of the feeder. |
|         enable_lightMode          |  bool  |    no    |                   false                    |             true/false              | if enabled, a switch will show in homekit to control the lighe mode of the feeder. |
| reverse_foodStorage<br>_indicator |  bool  |    no    |                   false                    |             true/false              | normally, the occupancy will show an alert in homekit when there is enough food in the feeder, in which situation may not so recognizable, so you can reverse the status but set this value to true, so when there is not much food, it can show an alert in homekit. |
| ignore_battery_when<br/>_charging |  bool  |    no    |                   false                    |             true/false              | Ignore battery low level alerm when device connected to a power source.<br>And battery function is disabled when using a Petkit Feeder Element device. |
|           fast_response           |  bool  |    no    |                   false                    |             true/false              | if set to true, then when received a Homekit set request, return immediately, ignore the result.<br>this is useful when your homebridge or Petkit device has a bad internet connection. |
|           meal_amount             |  int   |    yes   |                     8                      |             0 to 10                 | Meal amount set defaults.           |
|         feed_daily_list           | array  |    no    |                    ---                     |                 ---                 | feed daily list.<br/>See more detail info at <a href="#feed-daily-list-field">feed daily list field</a> below. |
|          enabled_daily_feeds      |  bool  |    no    |                    true                    |             true/false              | enable/disable feed daily plan.<br>(the choice is for every day of the week.  |
|          overwrite_daily_feeds    |  bool  |    no    |                    false                   |             true/false              | overwite feed daily plan with the data of this plugin.  |



### headers field

| field   name  |  type  | required | default  |   range   | description                                                                                                                                                                                                       |
| :-----------: | :----: | :------: | :------: | :-------: | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
|   X-Session   | string |   yes    |   ---    |    ---    | Tell server who you are. This changes everytime you login Petkit app.                                                                                                                                             |
| X-Api-Version | string |  prefer  | '7.18.1' |    ---    | For China mainland users, this field is not necessary, but for users outside China mainland, this field is optional, if not provided, then the default value will be used. but we recommand to fufill this field. |
|  X-Timezone   |  int   |    no    |    8     | -12 to 12 | Your local timezone offset, UTC time. If autoDeviceInfo is set to true, it will overwrited with the timezone of your device, which is set in your Petkit app.                                                     |

we recomand you entered all the headers you captured. If you don't want to do so, please ensure header "X-Session" is correctly entered.


### feed daily list field

| field   name  |  type  | required | default  |   range    | description                                                                                                                                                                                                       |
| :-----------: | :----: | :------: | :------: | :-------:  | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
|     name      | string |    no    |    ---   |    ---     |       food name. |    
|    amount     |   int  |    no    |     1    |    1-10    |     amount food dispensed. |
|     time      |  int   |    no    |     1    | 1 to 86400 | time in seconds the food will be dispensed. |


### example of config.json file

```json
"platforms": [{
  "log_level": 2,
  "devices": [
      {
        "headers": [
          {
            "key": "X-Session",
            "value": "xxxxxx"
          }
        ],
        "location": "international",
        "model": "D4",
        "enable_http_retry": false,
        "http_retry_count": 3,
        "DropMeal_name": "DropMeal",
        "MealAmount_name": "MealAmount",
        "FoodStorage_name": "FoodStorage",
        "DesiccantLevel_name": "DesiccantLevel",
        "ManualLock_name": "ManualLock",
        "LightMode_name": "LightMode",
        "Battery_name": "Battery",
        "BlockDoor_name": "BlockDoor",
        "enable_polling": true,
        "polling_interval": 60,
        "enable_desiccant": true,
        "alert_desiccant_threshold": 7,
        "enable_autoreset_desiccant": true,
        "reset_desiccant_threshold": 5,
        "enable_manualLock": true,
        "enable_lightMode": true,
        "reverse_foodStorage_indicator": true,
        "fast_response": true,
        "meal_amount": 8,
        "enabled_daily_feeds": true,
        "overwrite_daily_feeds": true,
        "feed_daily_list": [
            {
                "name": "Desayuno",
                "amount": 8,
                "time": 25200
            },
            {
                "name": "Comida",
                "amount": 6,
                "time": 46800
            },
            {
                "name": "Cena",
                "amount": 8,
                "time": 72000
            }
        ]
      }
  ],
  "platform": "petkit_pet_feeder"
}]
```



## 4) How to contribute

everyone is welcome to contribute to this plugin. PR/issue/debug all are welcome.

or you can send me an e-mail: jubepue@gmail.com



## 5) Buy me a coffee

if you think my work helps you, and wanna thank me, please consider buy me a coffee: https://paypal.me/jubepue

