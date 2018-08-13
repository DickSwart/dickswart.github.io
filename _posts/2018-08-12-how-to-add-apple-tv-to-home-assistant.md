---
layout: post
title:  "How to add Apple TV to Home Assistant"
date:   2018-08-12
category: [ blog, home-assistant ]
---

I had issues finding my Apple TV with the scan as explained in the [official documentation](https://home-assistant.io/components/apple_tv/), So this is what I did to finally get it working.

### Installation:

1. First, make sure your raspberry pi is up to date.
```
$ sudo apt-get update && sudo apt-get upgrade
```

2. Install the required packages.
```
$ sudo apt-get install build-essential libssl-dev libffi-dev python-dev
```

3. Stop your home-assistant.
```
sudo systemctl stop home-assistant@homeassistant.service
```

4. Activate the virtual environment and then install [pyatv](http://pyatv.readthedocs.io/en/master/atvremote.html), this is the software that allows us to communicate and interact with the apple tv.
```
$ sudo -u homeassistant -H -s
$ source /srv/homeassistant/bin/activate
$ pip3 install --upgrade pyatv
$ exit
```

5. Edit your `configuration.yaml` file, add `apple_tv:`.
If you want to discover new devices automatically, just make sure you have `discovery`: in your `configuration.yaml` file.

6. Now head over to your apple tv and make sure _Home Sharing_ is enabled. Go to `Settings > Accounts > Home Sharing` if it is off just enable it, you will be asked to sign in with your Apple Id.

7. Now we will scan for out apple tv, this can be done via the frontend as explained by the [documentation in the guid](https://www.home-assistant.io/components/apple_tv/#guides) you will just need to start your home assistant (`sudo systemctl start home-assistant@homeassistant.service`) or you can just do it from the command line.
```
$ sudo -u homeassistant -H -s
$ atvremote scan
```
Hold your thumbs and hopefully you will get an output that looks like this:
```
Found Apple TVs:
- Apple TV at 10.0.10.22 (login id: 00000000-1234-5678-9abc-def012345678)

Note: You must use 'pair' with devices that have home sharing disabled
```
If no Apple Tv gets returned, try to scan with a long timeout.
```
$ atvremote -t 9 scan
```
If you still don't get any results returned, head over to your Apple TV and disable and re-enable _Home Sharing_ and then restart your apple tv (_Settings > System > Restart_) and scan again using the commands above.

8. Now edit your `configuration.yaml` file (I'm using the example output values from above, you should use your own Apple TV `login id` and `IP address`).
```
# Example configuration.yaml entry
apple_tv:
  - host: 10.0.10.22
    login_id: 00000000-1234-5678-9abc-def012345678
    name: MyAppleTV_Name
```
9. Return to your user if you are still the home assistant user.
```
$ exit
```
10. Start home Assistant.
```
$ sudo systemctl start home-assistant@homeassistant.service
```
11. Go get a beer!!

### Setting up device authentication:
For the authentication part of the setup, I just followed the [official documentation](https://www.home-assistant.io/components/apple_tv/#setting-up-device-authentication).

Do this in front of your Apple TV as you will be required to enter a pin, running back and forward is not a good idea (I tried, it sucks! LOL)

### References
* Home Assistant Apple TV Componant - https://www.home-assistant.io/components/apple_tv
* atvremote - http://pyatv.readthedocs.io/en/master/atvremote.html
* Home Assistant Forum - https://community.home-assistant.io/
