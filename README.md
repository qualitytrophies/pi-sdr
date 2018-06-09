# pi-sdr
Using the Raspberry Pi as an SDR

NOTE - Make sure you have an actually sufficient power supply solution - this will sometimes draw over 1.2A, idle @ 700-800mA

The image is preconfigured.  It's a customised Raspbian Lite installation, snmpd is installed as well - the python code for the SDR has already been changed.  The adafruit-tft sdr is also included.

Passwords :-

  *   root      password
  *   pi        password
  
you can use the image, or this is how to do it :-

1. install Raspbian


2. To install dependencies :-

		time sudo apt-get -y install python-numpy python-scipy python-matplotlib ipython python-pandas python-sympy python-nose cmake build-essential python-pip libusb-1.0-0-dev git pandoc python-pygame



3. To install the rtlsdr :-

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



4. To install the panadapter :-

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
