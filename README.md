Dashing mining dashboard
====================

[Dashing](https://github.com/Shopify/dashing) web dashboard designed for monitoring cryptocurrency mining. Works on both desktop and mobile browsers

Check out [a sample dashboard](http://mining-dashboard.herokuapp.com/gigabyte).

![iMac, iPad and iPhone Dashboard](http://f.cl.ly/items/280W0j1x1q2L3K0D1O1C/placeit.jpg)

DMD allows you to easily aggregate statistics from multiple cgminer instances via [agent script](https://github.com/suda/dashing-mining-agent).

Features
========

* simple setup
* automatic refresh
* responsive layout
* displaying khps and GPU temperature graphs and shares accepted, rejected, HW errors, time elapsed time values
* mobile friendly web app (view on any mobile or desktop browser)

Setup
=====

You can do this on any computer (not nesessary your worker).

1. Create Heroku account and install toolbelt: https://toolbelt.heroku.com/
2. Clone repository: `git clone https://github.com/suda/dashing-mining-dashboard.git`
3. Enter directory: `cd dashing-mining-dashboard`
4. Create Heroku application: `heroku apps:create APP_NAME`
5. Rename/duplicate `default.erb` in `dashboards` directory to your worker names. Every worker should have it's own dashboard.
6. Set `AUTH_TOKEN` config variable using Heroku toolbelt: `heroku config:set AUTH_TOKEN=your_secret_token`
7. Deploy your dashboard to Heroku: `git push heroku master`
8. Start [sending events using agent](https://github.com/suda/dashing-mining-agent)
9. Visit your dashboard at `http://APP_NAME.herokuapp.com/WORKER_NAME`

On iOS you can add this page to home screen for better experience.

[Optional] Setup on your own Ubuntu Server
=====

Instead of using Heroku, you cna choose to setup the required pieces on your own web server.
Below are instructions of setting this up on an Ubuntu server (tested with Raspberry PI with Raspbian)

1. Setup Ubuntu flavor on your device with minimal installation
2. Perform updates and install dependencies
* `sudo apt-get update`
* `sudo apt-get install git-core git build-essential libssl-dev zlib1g-dev`
3. Change directory to opt (`cd /opt`)
4. Clone repository: `sudo git clone https://github.com/suda/dashing-mining-dashboard.git`
5. Enter directory: `cd dashing-mining-dashboard`
6. Rename/duplicate `default.erb` in `dashboards` directory to your worker names. Every worker should have it's own dashboard. (`sudo cp default.erb worker_name.erb`)
7. Edit `config.ru` and replace `set :auth_token, ENV['AUTH_TOKEN']` with `set :auth_token, 'YourSecureKey`
8. Bundle your dashboard (`bundle`)
9. Start the dashboard (`dashing start`)
10. Start [sending events using agent](https://github.com/suda/dashing-mining-agent)
11. Visit your dashboard at `http://IP/WORKER_NAME`
12. If everything is working properly, add the following to automatically start the dashing server on startup
* Copy dashboard.sh to service directory (`sudo cp /opt/dashing-mining-dashboard/dashboard.sh /etc/init.d/`)
* Update permissions of the file (`sudo chmod 755 /etc/init.d/dashboard`)
* Update rc.d (`sudo update-rc.d dashboard defaults`)
* Reboot and verify it starts up automatically

If you have any problems with setting up this dashboard, [create new issue](https://github.com/suda/dashing-mining-dashboard/issues/new) and I'll try to help.

[Optional] Edit Dashboard Units
=====

Your agent can now send modified temperature and hashing units.  Be sure to update your dashboard if oyu change the defaults otherwise visually the dtaa will not make sense.

1. Navigate to the `dashboards` directory
2. Edit `layout.erb`
3. Update Hashing Units:
* Replace `<div data-id="<%= params[:dashboard] %>_hash" data-view="Graph" data-title="KH/s" style="background-color:#ff9618">`
* With `<div data-id="<%= params[:dashboard] %>_hash" data-view="Graph" data-title="[PROPERUNITS]" style="background-color:#ff9618">`
* `[PROPERUNITS]` = KH/s, MH/s, or GH/s
4. Update Temperature Units:
* Replace `<div data-id="<%= params[:dashboard] %>_temperature" data-view="Temperature" data-title="Temperature (&deg;C)" style="background-color:#12B0C5;">`
* With `<div data-id="<%= params[:dashboard] %>_temperature" data-view="Temperature" data-title="Temperature (&deg;[PROPERUNITS])" style="background-color:#12B0C5;">`
* `[PROPERUNITS]` = "C or F"

Todo
====

* switching between dashboards
* adding dashboards without need to deploy
* update data_title in dashboards dynamically to reflect unit types being sent to them (temperature and hash)

Donations
========

Any donations are welcome and should result in more features in less time :)

BTC address: `1AAkZXsn9c2EWWbo7yzDEgMz1b3wMBN52Q`

LTC address: `LehFD6SvT3PfE4gBbrQwhXdrmFWwdrFxrU`

Author
======

Wojtek Siudzinski - [@suda](https://twitter.com/suda)

License
=======

Distributed under the [MIT license](https://github.com/suda/dashing-mining-dashboard/blob/master/LICENSE)
