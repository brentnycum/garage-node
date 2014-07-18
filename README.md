# Garage-node

This is the node.js app that is controlling my garage door openers via Raspberry Pi. To get the full details on my project check out my [post](http://itsbrent.net/2013/03/hacking-my-garage-with-a-raspberry-pi/) on my [blog](http://itsbrent.net).

## Requirements

Install [node.js](http://nodejs.org/) and npm onto your Raspberry Pi.

``` shell
[sudo] apt-get install nodejs npm
```

I recommend using [forever](https://github.com/nodejitsu/forever) to run your app so that it stays up.

``` shell
[sudo] npm install forever -g
```

## Installation

After checking out this code from Github, just run `npm install` from the app directory to install all dependencies.

## Dependencies Used

 * [Express](http://expressjs.com/)
 * [async.js](https://github.com/caolan/async)
 * [pi-gpio](https://github.com/brentnycum/pi-gpio) My fork. [Original Repo](https://github.com/rakeshpai/pi-gpio)

I used `pi-gpio` over [rpi-gpio](https://github.com/JamesBarwell/rpi-gpio.js) because I liked the fact pi-gpio made use of [GPIO Admin](https://github.com/quick2wire/quick2wire-gpio-admin), allowing you to be able to control the gpio pins without being the root user. Couldn't get `rpi-gpio` to use that.

## Configuration

In the root directory there is a file called `config.js`. In here you will find several variables that should be changed to work for your garage.

 * `LEFT_GARAGE_PIN` - Location of the gpio pin controlling the left garage door. Using the pin map from the `pi-gpio` README.
 * `RIGHT_GARAGE_PIN` - Same as above but for the right garage door.
 * `RELAY_ON` - Relay on state.
 * `RELAY_OFF` - Relay off state.
 * `RELAY_TIMEOUT` - How long the relay should stay on before turning off.

## Gotchas

My particular relay I used turns on when you fire low `0`, and off when firing high `1`. The app them use the `RELAY_TIMEOUT` to simulate a half second press, as if you pushed the wall switch.

The Raspberry Pi defaults the GPIO pin to low, but the pin is default set to input so it will not immediately fire on bootup. On first call the pin is opened for output and immediately low which turns on the relay since it's in the low state and making the set low call redundant but the relay is set high in the next call so it will need to be set low again on the next call.

## Todo

 * Error handling, currently there really is none.
 * Logging.

## Parts

 * Raspberry Pi running Raspbian "wheezy"
 * [Edimax EW-7811Un](http://www.amazon.com/dp/B003MTTJOY?tag=itsbr-20) (WiFi dongle, b/g/n)
 * [SainSmart 2-Channel Relay](http://www.amazon.com/dp/B0057OC6D8?tag=itsbr-20)

## Links

 * [Hacking My Garage With A Raspberry Pi](http://itsbrent.net/2013/03/hacking-my-garage-with-a-raspberry-pi/)
 * [WiringPi](https://projects.drogon.net/raspberry-pi/wiringpi/)
 * [The GPIO Utility](https://projects.drogon.net/raspberry-pi/wiringpi/the-gpio-utility/)
 * [GPIO Pin Mapping](https://projects.drogon.net/raspberry-pi/wiringpi/pins/)
