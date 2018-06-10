# pi-sdr
Using the Raspberry Pi as an SDR

NOTE - Make sure you have an actually sufficient power supply solution - this will sometimes draw over 1.2A, idle @ 700-800mA

Hardware Required :-

Raspberry Pi 2 or 3
Realtek Based USB Tv Stick
Decent power supply solution

1. install Raspbian Lite

		$ = user prompt, don't copy & paste as part of command
		# = root prompt, don't copy & paste as part of command

Install Raspbian Lite, with NOOOBS or direct img to microSD.

Boot the Pi, login and change the passwords, update the firmware and reboot :-

	login : 	pi
	password :	raspberry
	
	$ sudo su
	# passwd pi
			enter the new password, twice
			
	# passwd
			enter the new password, twice
			
	# rpi-update
	# reboot
	
Let the Pi finish booting with the updated firmware, then login as pi and run raspi-config, to select options such as CPU/GPU memory split, overlclocking, interfacing & localisation. Make sure to enable SSH under interfacing, We will be able to use putty for the console commands. It will ask you to reboot when you are finished.

	$ sudo raspi-config

Now We get to making Raspbian Lite a bit more useful, with as few extras as possible. Login via putty - the IP address appears in the boot messages, making it easier - away We go !

First, We update what's there, so We have a good starting position

	$ sudo apt-get update
	$ sudo apt-get dist-upgrade
	$ sudo apt-get clean
	$ sudo apt-get autoclean
	
Now We get to filling out the Raspbian Lite installation.	
	
	$ sudo apt-get -y install --no-install-recommends xserver-xorg xinit
	$ sudo apt-get -y install raspberrypi-ui-mods
	
Run raspi-config again, to set up autologin & VNC - perhaps set the hostname too.

	$ sudo raspi-config
	
It will ask to reboot, select Yes and you should be presented with a graphical desktop when it has finished rebooting.

Now to install the last few auxillary programs.

	$ sudo apt-get -y install snmpd snmp-mibs-downloader whowatch iptraf zenmap
	$ sudo apt-get clean
	$ sudo apt-get autoclean

I will pause here and make an image of the microSD card.  Another image will be made after all of the Software Defined Radio installation has been completed.

2. To install dependencies :-

		time sudo apt-get -y install python-numpy python-scipy python-matplotlib ipython python-pandas python-sympy python-nose cmake build-essential python-pip libusb-1.0-0-dev git pandoc python-pygame



3. To install the rtlsdr, the driver that it uses to talk with the Tv Tuner directly :-

		cd ~
		git clone git://git.osmocom.org/rtl-sdr.git
		cd rtl-sdr
		mkdir build
		cd build
		cmake ../ -DINSTALL_UDEV_RULES=ON -DDETACH_KERNEL_DRIVER=ON
		make
		sudo make install
		sudo ldconfig
		sudo pip install pyrtlsdr



4. To install the panadapter, the display software :-

		cd ~
		git clone https://github.com/Banjopkr/WQ7Tpanadapter.git
		cp -r WQ7Tpanadapter/FreqShow_Large FreqShow_Large


5. To make panadapter run on any screen, not just TFT :-

		cd ~/FreqShow_Large
		nano freqshow.py

			Comment out these lines :-
		
		    	# Initialize pygame and SDL to use the PiTFT display and touchscreen.
				os.putenv('SDL_VIDEODRIVER', 'fbcon')
				os.putenv('SDL_FBDEV'      , '/dev/fb1')
				os.putenv('SDL_MOUSEDRV'   , 'TSLIB')
				os.putenv('SDL_MOUSEDEV'   , '/dev/input/touchscreen')
6. To run the panadapter :-

		cd ~/FreqShow_Large
		sudo python freqshow.py

7. Enjoy !
