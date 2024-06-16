# SETUP I2C OLED 128x64px ON HEADLESS Pi5 BOOKWORM 

Install Bookworm 64bit on your Pi5.

SSH Into Pi with terminal. 
    If you get   “WARNING: REMOTE HOST IDENTIFICATION HAS CHANGED!” THEN ENTER...
    
``nano ~/.ssh/known_hosts``
- Type Ctrl+K to remove all the pi files.
- Type Ctrl+X to exit
- Type Y to save
- Hit enter to finish.

Now SSH into Pi again.

In terminal, enter…

``sudo raspi-config``

Navigate to "3. Interface Options" > I4 I2C > Yes > ok

Select Finish

Enter Terminal Commands

``sudo apt-get update``

``sudo apt-get -y upgrade``

``sudo apt-get install python3-pip``

``sudo apt install --upgrade python3-setuptools``

## Now we need to set up a virtual environment

``sudo apt install python3-venv``

Were going to call our virtual environment "oled_env"

``python3 -m venv oled_env --system-site-packages``

Now we need to activate the virtual environment

``source oled_env/bin/activate``

You should see (oled_env) at the beginning of your terminal line now.

While In your virtual environment, type into terminal…

``cd ~``

``pip3 install --upgrade adafruit-python-shell``

``wget https://raw.githubusercontent.com/adafruit/Raspberry-Pi-Installer-Scripts/master/raspi-blinka.py``

``sudo -E env PATH=$PATH python3 raspi-blinka.py``

When done hit “Y” to reboot.

SSH back into Pi via terminal after reboot.

in terminal, enter...

``i2cdetect -y 1``

You should see a grid of lines with 3c like below which lets you know your OLED is working correctly.

<img width="374" alt="Screenshot 2024-05-18 at 9 49 50 AM" src="https://github.com/RUDEWORLD/Pi5OLED/assets/45774998/a1e34f84-ab08-40ba-a61c-150bc42948e1">

If it doesn’t look like this, then check the wiring to make sure you wired the OLED correctly.

Now we need to go back into virtual environment and run some commands.

``cd ~``

``source oled_env/bin/activate``

``pip3 install --upgrade adafruit_blinka``

``pip3 install adafruit-circuitpython-ssd1306``

``sudo apt-get install python3-pil``

Now exit virtual environment and run some more commands.

``deactivate``

``sudo apt-get install git``

``cd ~``

``git clone https://github.com/RUDEWORLD/OLED_Stats.git``

Now we need to go back into virtual environment.

``cd ~``

``source oled_env/bin/activate``

``cd OLED_Stats``

``python3 stats.py``


Your OLED Should turn on at this point! But the problem is it’s running in a virtual environment that will turn off when you reboot the system.
Now We'll make an executable file that will fix this. Enter terminal command below to download file from GitHub, then the next command will make it executable.

``curl -OL https://raw.githubusercontent.com/RUDEWORLD/Pi5OLED/main/OLED_ACTIVATE``

``sudo chmod +x /home/pi/OLED_ACTIVATE``

MAKE SURE TO REPLACE “pi” WITH YOUR USER NAME IN LINE ABOVE IF YOU CHANGED THE USER NAME.


Now we need to tell the Pi to execute that file on reboot. In terminal, enter

``crontab -e``

If this is the first time opening crontab, type 1 and hit enter. Now, at the bottom of the blue text, enter a line of text without an "#" marks. It should look like the image below.

<img width="568" alt="Screenshot 2024-05-18 at 10 36 05 AM" src="https://github.com/RUDEWORLD/Pi5OLED/assets/45774998/83a6540c-3e19-4380-bc9d-1d63759fb47b">

@reboot /home/pi/OLED_ACTIVATE &

MAKE SURE TO REPLACE “pi” WITH YOUR USER NAME IN LINE ABOVE IF YOU CHANGED THE USER NAME.
- Type Ctrl+X
- Type Y to save
- Hit Enter to finish

# Now reboot your Pi and your OLED should stay on!





