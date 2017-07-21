## Using GPS

- Most GPS receivers will talk to your computer using **serial communication**. This will use a specific port on your computer, and you will need to find out which port your receiver is using.

- With the receiver unattached, open up a terminal and type the following command:

	```bash
	ls /dev/ > dev_list_1.txt
	```
	
- This will create a text file, containing all the devices connected to your computer.

- Now plug in the GPS receiver and type the following line, which will display the new devices that have been added:

	```bash
	ls /dev/ | diff --suppress-common-lines -y - dev_list_1.txt
	```

- A list such as this should be displayed.

  ```bash
  serial							      <
  ttyACM0
  ```

- The device needed is the one starting with `tty`, so in this case it is `ttyACM0`.

- To make sure the device can be read, try typing the following into the terminal.

	```bash
	cat /dev/ttyACM0
	```
	
	- On some *nix systems you may not have access to the `/dev/ttyACM0'. Change your permissions first and then logout and back in again.
	```bash
	sudo usermod -a -G dialout your_username
	```
	- Some times a continuous stream of data might not be seen, so try the following:
	
	```bash
	sudo apt install cu
	cu -l /dev/ttyACM0 -s 115200
	```
- The data you see in your terminal contains your longitude, latitude, altitude and the current time. You can use Python to make a little more sense of this.

- Firstly, download a small file that contains the necessary code.

	```bash
	wget https://raw.githubusercontent.com/raspberrypilearning/piGPS/master/piGPS.py
	```

- Then download the `pyserial` module which is needed by this program.

	```bash
	sudo pip3 install pyserial
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
