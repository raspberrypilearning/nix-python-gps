## Using GPS

- Most GPS receivers talk to your computer via **serial communication**. Do do this, they use a specific port on your computer, and you will need to find out which port your receiver is using.

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

- The device needed is the one starting with `tty`, so in this case it is `ttyACM0`.

- To make sure the device can be read, try typing the following into the terminal:

	```bash
	cat /dev/ttyACM0
	```
**What should I expect to see now?**

**See comments on Waffle regarding troubleshooting**
	- On some 'nix systems you may not have access to the `/dev/ttyACM0`. Change your permissions first, and then log out and back in again.
	```bash
	sudo usermod -a -G dialout your_username
	```
	- Sometimes you might not see a continuous stream of data. If this is the case, try the following:
	
	```bash
	sudo apt install cu
	cu -l /dev/ttyACM0 -s 115200
	```
 **Maybe add a screenshot of the terminal window?**
- The data you see in your terminal contains your longitude, latitude, altitude, and the current time. You can use Python to make a little more sense of this.

- Firstly, download a small file that contains the necessary program.

	```bash
	wget https://raw.githubusercontent.com/raspberrypilearning/piGPS/master/piGPS.py
	```

- Then download and install the `pyserial` module this program needs.

	```bash
	sudo pip3 install pyserial
	```
**I think having both files in the same directory should be emphasized a bit more. Maybe say something like "move the file you downloaded to a new folder in which you will save the code you'll write to access the GPS data' plus command line code, etc.**
- As long as the `piGPS.py` file is in the same directory as the code you are writing, you can now access all the data using a few simple lines of Python.

```python
from piGPS import GPS
gps = GPS()

print(gps.lon)
print(gps.lat)
print(gps.time)
print(gps.alt)
```
