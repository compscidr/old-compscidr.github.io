---
layout: default
title: "Turning a Netgear r7000 into an augmented traffic control (atcd) router"
published: true
featured-image: "ddwrt.png"
featured-alt: "ddwrt screenshot"
categories: [Wireless Networks]
tags: [ 3rd world, atcd, ddwrt, traffic-shaping, linux, open source, developing countries, Internet	]
---

At [work](https://left.io) we're developing apps that are being used in developing countries and half of the office works out of Vancouver where our networks are very good. Unfortunately, this means that we often don't think about user experience problems and bugs that occur only when the app is operating with a poor quality connection.

To combat this, we are taking motivation from Facebook, which offers "2G Tuesdays" to employees so they can experience what it's like for people in other parts of the world. Facebook also released a tool call [Augmented Traffic Control](https://engineering.fb.com/production-engineering/augmented-traffic-control-a-tool-to-simulate-network-conditions/) which allows you to simulate these types of conditions with your own equipment.

At our office, we'd like to have a dedicated device that provides this - and I had an old netgear R7000 router at home. Here's a bit of a guide on how to get everything up and running.

First, flash dd-wrt onto the router. Detailed instructions here: [https://dd-wrt.com/wiki/index.php/DD-WRT_on_R7000](https://wiki.dd-wrt.com/wiki/index.php/DD-WRT_on_R7000): basically take the r7000 file from here: [ftp://ftp.dd-wrt.com/betas/](ftp://ftp.dd-wrt.com/betas/) and upload it to the router using the update tool in the stock Netgear web tool.

SSH into the router. Username will be root and the password will be whatever you set your user password to in the router web interface.
Next, update the firmware:

```
ddup --flash-latest
```

Enable usb storage:

I formatted the usb stick so that the entire thing is filled with a ext3 partition first (although you could add a swap partition if you wanted too).

![ddwrt screenshot](/assets/img/ddwrt.png)

(note: you may need to automount it first without putting the uuid in for opt and then refresh the page so it shows the uuid for the partition) - then fill it in save the page, unplug and replug the usb stick and probably refresh the page.

Next, we need a package manager to install all of the extra software we'll need. First I tried optware, but it wouldn�t work when I tried to install the python tools because there were certificates installed. So next I tried entware which worked much better: [https://gist.github.com/dreamcat4/6e58639288c1a1716b85](https://gist.github.com/dreamcat4/6e58639288c1a1716b85).

```
cd /tmp
wget http://qnapware.zyxmon.org/binaries-armv7/installer/entware_install_arm.sh
chmod +x entware_install_arm.sh
./entware_install_arm.sh
```

You should be able to run the opkg command now (the package manager). Try running:

```
opkg update
```

If you have no errors we are good to install the other packages.

```
opkg install ca-certificates python-base python-crypto python-logging nano
```

And now we want to get the pip python package manager (note previously we installed the ez_setup script first, but it has been deprecated and replaced with setuptools being installed by pip in the next step).

```
curl https://bootstrap.pypa.io/get-pip.py -k > get-pip.py
python get-pip.py
```

And finally all of the stuff required by the [Augmented Traffic Control](https://github.com/facebookarchive/augmented-traffic-control):
```
pip install setuptools atc_thrift atcd django-atc-api django-atc-demo-ui django-atc-profile-storage
```
Make a directory for the django project:

```
mkdir /opt/var/django
cd /opt/var/django
django-admin startproject atcui
cd atcui
```

Open atcui/settings.py and enable the ATC apps by adding to `INSTALLED_APPS`:

```
INSTALLED_APPS = (     
...     
# Django ATC API     
'rest_framework',     'atc_api',     
# Django ATC Demo UI     
'bootstrap_themes',     'django_static_jquery',     'atc_demo_ui',     # Django ATC Profile Storage     
'atc_profile_storage', )
Also in the same file, make sure you set ALLOWED_HOSTS = [�*�] (otherwise it will give you a disallowed hosts error when you go to the website).
Now, open atcui/urls.py and enable routing to the ATC apps by adding the routes to urlpatterns:
from django.conf.urls import *
from django.views.generic.base import RedirectView
from django.conf.urls import url
from django.contrib import admin
from django.conf.urls import include
urlpatterns = [
   url(r'^api/v1/', include('atc_api.urls')),
   url(r'^atc_demo_ui/', include('atc_demo_ui.urls')),
   url(r'^api/v1/profiles/', include('atc_profile_storage.urls')),
   url(r'^$', RedirectView.as_view(url='/atc_demo_ui/', permanent=False)),
]
```

You probably want to test that everything is working (follow the guide from the atcd git / wiki [or just follow the instructions below for a condensed version]).

(start atcd - note the lan and wan ports are needed otherwise it wont perform shaping correctly. Also the iptables thing is required because it can�t seem to find iptables on its own on the ddwrt setup)

```
atcd --atcd-wan vlan2 --atcd-lan br0 --atcd-mode unsecure --atcd-iptables /usr/sbin/iptables
```

(start the web server: atc_demo_ui:)

```
cd /opt/var/django/atcui
python manage.py runserver 0.0.0.0:8000
```

Setup a shaping profile at [http://192.168.1.1:8000/atc_demo_ui/](http://192.168.1.1:8000/atc_demo_ui/) and apply it to yourself. See if it works. If it does, setup a redirect and create a startup script so that it always starts on router boot.

In order to enable all devices to be forced into using the shaping - I setup a nocatsplash redirect to the webui for the atcd: [http://192.168.1.1:8000/atc_demo_ui/](http://192.168.1.1:8000/atc_demo_ui/)

Then to make atcd and the ui start on boot, I added the startup commands to a startup.sh script and put it into /opt/startup:
```
#!/bin/sh
atcd --atcd-wan vlan2 --atcd-lan br0 --atcd-mode unsecure --atcd-iptables /usr/sbin/iptables --daemon
nohup python /opt/var/django/atcui/manage.py runserver 0.0.0.0:8000 &
```

and then used the ddwrt web interface to set it as a startup script so it runs on boot every time.

Originally published on April 4, 2016. Updated Aug 22, 2017. Also appears on Medium [Turning a Netgear r7000 into an augmented traffic control (atcd) router
](https://medium.com/@compscidr/turning-a-netgear-r7000-into-an-augmented-traffic-control-atcd-router-f0c9db861fd7)
