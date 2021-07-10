---
layout: post
title: How to connect to WhatsApp from desktop (Ubuntu 14.04/14.10/15.04) by installing Pidgin and registering any mobile number with Yowsup
comments: true
author: Rohan Paul
categories: Pidgin-WhatsApp

---
<img src="/images/fulls/pidgin-whatsapp.jpg" class="fit image">

**Pidgin** is an excellent graphical IM program that lets you sign on to WhatsApp, AIM, Jabber, MSN, Yahoo!, and other IM networks.

In this article I shall cover compiling Pidgin from its source, as my experience to install Pidgin directly from Ubuntu Software Center was very bad. And then we shall register a mobile number with Yowsup to generate a WhatsApp password to have the complete setup to connect to WhatsApp from Ubuntu desktop.

##Preparing the machine by installing the necessary libraries and headers


**A>** As suggested in [Pidgin's official page for compiling from source](https://developer.pidgin.im/wiki/Installing%20Pidgin#Compiling) run the below command to install the development headers for Pidgin packages. Open a Terminal ( Ctrl + Alt + T) and run the below command.

``$ sudo apt-get build-dep pidgin``

For a fresh machine, it will install around 77MB of packages.

**B>** Install full GNOME Desktop developments environment with the below command in a terminal

``$ sudo apt-get install gnome-devel``

For a new machine it will install around 450MB of packages.

You may actually choose to skip installing the full GNOME Desktop development at this stage. However in that case, during Pidgin compilation process, the terminal will throw error messages showing the specific missing packages / libraries and you can then only install those specific ones. In my case, I had quite a few of them to install separately, so I just avoided installing them later by installing the full GNOME devlopment environment now.

**C>** Download the latest version of [Pidgin from its official source](https://www.pidgin.im/download/ubuntu/). As of the time of this writing (Aug-2015), the latest file available is "pidgin-2.10.11.tar.bz2".

**D>** Open a terminal and cd into the destination directory where you have downloaded the above .tar.bz2 file and assuming you have installed the above necessary libraries run these commands sequentially.

So if you have downloaded Pidgin source (the .tar.bz2 file) into ~/Downloads/Pidgin-Source

``$ cd ~/Downloads/Pidgin-Source``

``$ tar xjvf pidgin-2.10.11.tar.bz2``

``$ cd pidgin-2.10.11``

``$ ./configure``

``$ sudo make``

``$ sudo make install``

After that to clean up the extracted directory you may choose to run ``$ sudo make distclean``

**E>** Now in my case, I had to restart the machine at this step, as Ubuntu was not showing Pidgin as an installed application in my machine before the complete reboot.


##In the next phase we are going to install whatsapp-purple library


**A>** The official site of [whatsapp-purple](https://github.com/davidgfnet/whatsapp-purple) gives a very nice guide for its installation.

Basically open your System Settings > Software & Updates > move to "Other Software" tab > click on "Add" button > Write the following line:

``deb http://ppa.launchpad.net/whatsapp-purple/ppa/ubuntu trusty main``

Now just run the below command

``$ sudo apt-get install whatsapp-purple``

**B)** Now ideally, if you open Pidgin and move into Add Protocol > in the dropdown list “WhatsApp” with its green phone logo should appear.

But in my case, even though the WhatsApp protocol appeared, the green phone logo was missing.

-----
**C)** To solve the above issue – I had to do the following further steps (hopefully, if everything goes well in your machine, you would not have to go through these extra steps).


+ Go into the official site of [whatsapp-purple](https://github.com/davidgfnet/whatsapp-purple) again
+ Click on the link for Ubuntu's Official binary source. It will take you to - https://launchpad.net/~whatsapp-purple/+archive/ubuntu/ppa
+ In this page click on "View Package Details" and filter for the relevant seres of your Ubuntu - e.g. Vivid / Utopid / Trusty.
+ Now Expand the section "source" and download the .tar.gz package files for your machine; e.g. for for my Ubuntu 15.04 and 64bit machine, the file I downloaded is "pidgin-whatsapp_0.8.4.tar.gz"
+ Extract the .tar.gz file > Quickly glance through the "Readme" file > it will instruct to just the following 2 commands
+ First in the Terminal cd into that extracted directory > Run ``$ make`` and then run ``$ sudo make install``
+ Note in this case, unless the Readme file suggests otherwise, you don't need to run < ./configure > as very first command, (as is usual in the case of compiling from source).

-----


**D>** And now finally the WhatsApp Protocol with its green phone logo appeared in Pidgin when I go into "Add Account". Now add your account username and password.

The Mobile Number that you have registered with WhatsApp is your Username and the Password is the one that you received at the time of registering. Here we will register our mobile number and create a password with Yowsup. (There are also couple of other tools available for this purpose as an alternative to Yowsup).

##Possible Issues that may come up during installation, that I faced.

**First Issue >** **After creating a new account with correct username and password and adding successfully to Pidgin, I was getting - ``not-authorized`` and also ``Mobile_number disabled``** -

**Solved this issue as below -**

In Pidgin go into Accounts > Modify Account > Advanced > Delete whatever default value is being assigned in the “Resource” field which in my case was “Android-2.12.5-443”. Then go into Account > Manage Account again > and enable the account by check marking the **Enable** box for your Account.

Then for sometime I had to do this step every time I restart Pidgin, as Pidgin will automatically assign the old non-functioning value (“Android-2.12.5-443”) back to the "Resource" field.

Only recently from [this github issue page](https://github.com/davidgfnet/whatsapp-purple/issues/275) came to know that Current whatsapp android version is 2.12.176. So, changed the "Resource" field value to "Android-2.12.176" - And now everything is working perfectly and I don't have to do anything in the "Resource" field manually.

**Second Issue >** In the step C above ( which hopefully you will not have to go through if everything goes well ) When installing WhatsApp Purple library– after extracting the .tar.gz file and then running ``$ make`` in the Terminal failed giving the below error message -

``“gcc -c -O2 -Wall -Wno-unused-function -fPIC -DPURPLE_PLUGINS -DPIC -I/usr/local/include/libpurple -I/usr/include/glib-2.0 -I/usr/lib/x86_64-linux-gnu/glib-2.0/include -o imgutil.o imgutil.c imgutil.c:2:23: fatal error: FreeImage.h: No such file or directory #include FreeImage.h”``

**Solved this issue as below -**

From Synaptic, I searched for **FreeImage** and selected some 3 packages for installation (libfreeimage3, libfreeimage3-dbg, libfreeimage-dev)

Now ran again command ``$ make`` in the terminal, it completed successfully and at the end should show something like the below -

``“install -D libwhatsapp.so /usr/local/lib/purple-2/libwhatsapp.so install -D -m 644 whatsapp16.png /usr/local/share/pixmaps/pidgin/protocols/16/whatsapp.png install -D -m 644 whatsapp22.png /usr/local/share/pixmaps/pidgin/protocols/22/whatsapp.png install -D -m 644 whatsapp48.png /usr/local/share/pixmaps/pidgin/protocols/48/whatsapp.png ”``

##Registering the mobile number with Yowsup and get the password for logging into WhatsApp


**A>** **First method to install yowsup**

Take a quick look at the [Yowsup's official github site](https://github.com/tgalal/yowsup) and then run the below command sequentially. Run the below command sequentially.

**1>** Your machine will need to have python dev package so run

``$ sudo apt-get install python-dev``

or you can also run

``$ sudo apt-get install python-dev build-essential``

If you face the error (like in my case,) “Package python-dev is not available” after running the above command in Ubuntu-15.04 – just add the universe repo to your version of Ubuntu, run the following commnds in sequence -

``sudo add-apt-repository "deb http://archive.ubuntu.com/ubuntu $(lsb_release -sc) universe"``

``sudo add-apt-repository "deb http://archive.ubuntu.com/ubuntu $(lsb_release -sc) main universe restricted multiverse"``


In the above 2 commands ``$(lsb_release -sc)`` checks your Ubuntu version and puts its name in the link adding the corresponding Ubuntu repositories. So in my machine running Ubuntu 15.04 (called vivid ), just to be safe, I tested the command by running only ``lsb_release -sc`` and it threw “vivid”.

OK, and after running the above 2 commands, enabling the Universe of Ubuntu repositories, run the below ones.


``sudo apt-get update``

``$ sudo apt-get install python-dev``

And then run the below to install ``yowsup2``

``$ sudo pip install yowsup2``

For the above command you may need to install pip first, if you don't have it already, by running the below one

``$ sudo apt-get install python-pip``

Now terminal would give give you confirmation saying something like - ``“Successfully installed yowsup2 python-axolotl protobuf python-axolotl-curve25519”``

**Second method to install yowsup (in case the first one does not work for some reason)**

$ git clone https://github.com/tgalal/yowsup.git

$ cd yowsup

$ sudo python setup.py install

**B>** **Now the actual WhatsApp registration involves 2 steps. First you need to request a registration code. And then you resume the registration with that code you got.**

The [youwsup wiki page](https://github.com/tgalal/yowsup/wiki/yowsup-cli-2.0#yowsup-cli-registration) explains it very nicely

Run the below command to request the code assuming your mobile number is 12345677890 and country code of 91

``$ yowsup-cli registration --requestcode sms --phone 911234567890 --cc 91 --mcc 405 --mnc 67``

Get the value of **"mcc"** (Mobile Country Code) and **"mnc"** Mobile Network Code for your specific case. Yowsup's official Github page suggests, you can check this [wikipedia page](https://en.wikipedia.org/wiki/Mobile_country_code) for the **mcc** and **mnc** codes.

For example for Vodaphone in WestBengal (India), the mcc is - 405 and mnc is 67

And after running the above command the termianl displayed the below -

``"status: sent retry_after: 1805 length: 6 method: sms”``

The code should be reach your mobile in 5/8 seconds ideally.

**C>** Assuming you received a code of 123456, now to resume registraion run the below command

``$ yowsup-cli registration --register 123456 --phone 49XXXXXXXX --cc 91``

And got the below in Terminal

status: ok

kind: free

pw: Full_Password

price: ₹ 55

price_expiration: 4444444465

currency: INR

cost: 55.00

expiration: 4444444465

login: 911234567890

type: existing


**D>** That's it, in the above message the Price and Cost has not significance (at least as of now). Just make a note of the password displayed in Terminal, go to Pidgin and Add your Account credentials.

A quick point to note, you will not be able to login to the same WhatsApp account from both your Mobile and desktop.



Thanks for taking time to read this post and if you enjoyed, follow me on **[Twitter](https://twitter.com/paulr_rohan)** for future updates.