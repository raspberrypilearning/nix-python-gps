## Using GPS

- Most GPS receivers talk to your computer via **serial communication**. To do this, they use a specific port on your computer, and you will need to find out which port your receiver is using.

- With the receiver not connected, open up a terminal window and type the following command:

	```bash
	ls /dev/ > dev_list_1.txt
	```
	
- This will create a text file containing all the devices connected to your computer.

- Now plug in the GPS receiver and type the following line, which will display the new devices that have been added:

	```bash
	ls /dev/ | diff --suppress-common-lines -y - dev_list_1.txt
	```

- You should see a list similar to this:

  ```bash
  serial							      <
  ttyACM0
  ```

- The device you're interested in is the one starting with `tty`, so in this case it is `ttyACM0`.

- To make sure the device can be read, type the following into the terminal window:

	```bash
	cat /dev/ttyACM0
	```

- You should now see a continuous stream of data from the device, similar to this:

```
$GPRMC,,V,,,,,,,,,,N*53
$GPVTG,,,,,,,,,N*30
$GPGGA,,,,,,0,00,99.99,,,,,,*48
$GPGSA,A,1,,,,,,,,,,,,,99.99,99.99,99.99*30
$GPGLL,,,,,,V,N*64
$GPRMC,,V,,,,,,,,,,N*53
$GPVTG,,,,,,,,,N*30
$GPGGA,,,,,,0,00,99.99,,,,,,*48
$GPGSA,A,1,,,,,,,,,,,,,99.99,99.99,99.99*30
$GPGLL,,,,,,V,N*64
```
- If you don't see a stream of data, or nothing at all, have a look at the troubleshooting section below.

- The data you see in your terminal window contains your longitude, latitude, altitude, and the current time. You can use Python to make a little more sense of this.

- Firstly, download a small file that contains the necessary Python program we've written for you.

	```bash
	wget https://raw.githubusercontent.com/raspberrypilearning/piGPS/master/piGPS.py
	```

- Then download and install the `pyserial` module this program needs.

	```bash
	sudo pip3 install pyserial
	```
- Now make a new directory for your project, and then move the downloaded code to that directory.

	```bash
	mkdir my_cool_project
	mv piGPS.py my_cool_project/,
	```
	
- As long as the `piGPS.py` file is in the same directory as the code you are writing, you can now access all the data using a few simple lines of Python.

```python
from piGPS import GPS
gps = GPS()

print(gps.lon)
print(gps.lat)
print(gps.time)
print(gps.alt)
```

## Troubleshooting

- If you don't see anything after using the `cat /dev/ttyACM0` command, this may be because you do not have access to the `/dev/ttyACM0` device. This can be the case on some *nix systems. To gain access, first change your permissions using the command below, and then log out and back in again.

```bash
sudo usermod -a -G dialout your_username
```

- If you do not see a continuous stream of data after using the `cat /dev/ttyACM0` command, it may be that your computer is missing a piece of software. Try the following and see whether that fixes your problem.

```bash
sudo apt install cu
cu -l /dev/ttyACM0 -s 115200
```
