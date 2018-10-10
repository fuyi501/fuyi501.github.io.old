---
title: Stepper Motor HAT for Raspberry Pi
key: 20160516
tags: 树莓派
comment: true
---

## Overview

![](http://images.fuyix.cn/17-5-16/75364262-file_1494903663676_e548.jpg)

Let your robotic dreams come true with the new DC+Stepper Motor HAT. This Raspberry Pi add-on is perfect for any motion project as it can drive up to 4 DC or 2 Stepper motors with full PWM speed control,It also adds the capability to control 4 Servos with perfect timing.

Powering Motors
Motors need a lot of energy, especially cheap motors since they're less efficient. 
Voltage requirements:
The first important thing to figure out what voltage the motor is going to use. If you're lucky your motor came with some sort of specifications. Some small hobby motors are only intended to run at 1.5V, but its just as common to have 6-12V motors. The motor controllers on this HAT are designed to run from 5V to 12V.

MOST 1.5-3V MOTORS WILL NOT WORK or will be damaged by 5V power
Current requirements:
The second thing to figure out is how much current your motor will need. The motor driver chips that come with the kit are designed to provide up to 1.2 A per motor, with 3A peak current. Note that once you head towards 2A you'll probably want to put a heat-sink on the motor driver, otherwise you will get thermal failure, possibly burning out the chip.
If you don't have to take your project on the go, a 9V 1A, 12V 1A, or 12V 5A will work nicely
99% of 'weird motor problems' are due to having a voltage mismatch (too low a voltage, too high a voltage) or not having a powerful enough supply! Even small DC motors can draw up to 3 Amps when they stall.
Power it up
Wire up your battery pack to the Power terminal block on the right side of the HAT. It is polarity protected but still its a good idea to check your wire polarity. Once the HAT has the correct polarity, you'll see the LED light up
Please note the HAT does not power the Raspberry Pi, and we strongly recommend having two seperate power supplies - one for the Pi and one for the motors, as motors can put a lot of noise onto a power supply and it could cause stability problems!
Installing Software
We have a Python library you can use to control DC and stepper motors, its probably the easiest way to get started, and python has support for multithreading which can be really handy when running multiple stepper motors at onces!
Enable I2C
You will have to make I2C support work on your Pi before you begin, visit our tutorial to enable I2C in the kernel!
Before you start, you'll need to have the python smbus library installed as well as 'git', run apt-get install python-smbus i2c-tools 
Downloading the Code 
The easiest way to get the code onto your Pi is to hook up an Ethernet cable or with a WiFi setup, and clone it directly using 'git', which is installed by default on most distros.
Simply run the following commands from an appropriate location (ex. "/home/pi"):
wget tar xvzf Raspi_MotorHAT.tar
cd Raspi_MotorHAT
Install python-dev if you havent already:
Copy Code
sudo apt-get install python-dev
 
from within the Motor HAT library folder, we have a couple examples to demonstrate the different types of motors and configurations. The next few pages will explain them
Using DC Motors
DC motors are used for all sort of robotic projects.

The Motor HAT can drive up to 4 DC motors bi-directionally. That means they can be driven forwards and backwards. The speed can also be varied at 0.5% increments using the high-quality built in PWM. This means the speed is very smooth and won't vary!

Note that the H-bridge chip is not meant for driving continuous loads over 1.2A or motors that peak over 3A, so this is for small motors. Check the datasheet for information about the motor to verify its OK!
Connecting DC Motors
To connect a motor, simply solder two wires to the terminals and then connect them to either theM1, M2, M3, or M4. If your motor is running 'backwards' from the way you like, just swap the wires in the terminal block
For this demo, please connect it to M3
Now go into the Raspi_MotorHAT / folder and run sudo python DCTest.py to watch your motor spin back and forth.

 
Note: need to external power supply to the motor power supply
DC motor control
Here's the code which shows you everything the MotorHAT library can do and how to do it.
The MotorHAT library contains a few different classes, one is the MotorHAT class itself which is the main PWM controller. You'll always need to create an object, and set the address. By default the address is 0x6F(see the stacking HAT page on why you may want to change the address)
Copy Code
1.	# create a default object, no changes to I2C address or frequency
2.	mh =Raspi_MotorHAT(addr=0x6F)
The PWM driver is 'free running' - that means that even if the python code or Pi linux kernel crashes, the PWM driver will still continue to work. This is good because it lets the Pi focus on linuxy things while the PWM driver does its PWMy things. But it means that the motors DO NOT STOP when the python code quits
For that reason, we strongly recommend this 'at exit' code when using DC motors, it will do its best to shut down all the motors.
Copy Code
1.	# recommended for auto-disabling motors on shutdown!
2.	def turnOffMotors():
3.		mh.getMotor(1).run(Raspi_MotorHAT.RELEASE)
4.		mh.getMotor(2).run(Raspi_MotorHAT.RELEASE)
5.		mh.getMotor(3).run(Raspi_MotorHAT.RELEASE)
6.		mh.getMotor(4).run(Raspi_MotorHAT.RELEASE)
7.	 
8.	atexit.register(turnOffMotors)
Creating the DC motor object
OK now that you have the motor HAT object, note that each HAT can control up to 4 motors. And you can have multiple HATs!
To create the actual DC motor object, you can request it from the MotorHAT object you created above with getMotor(num) with a value between 1 and 4, for the terminal number that the motor is attached to
Copy Code
1.	myMotor = mh.getMotor(3)
DC motors are simple beasts, you can basically only set the speed and direction.
Setting DC Motor Speed
To set the speed, call setSpeed(speed) where speed varies from 0 (off) to 255 (maximum!). This is the PWM duty cycle of the motor
Copy Code
1.	# set the speed to start, from 0 (off) to 255 (max speed)
2.	myMotor.setSpeed(150)
Setting DC Motor Direction
To set the direction, call run(direction) where direction is a constant from one of the following:
•	Raspi_MotorHAT.FORWARD - DC motor spins forward
•	Raspi _MotorHAT.BACKWARD - DC motor spins forward
•	Raspi _MotorHAT.RELEASE - DC motor is 'off', not spinning but will also not hold its place.
Copy C
Using Stepper Motors
Stepper motors are great for (semi-)precise control, perfect for many robot and CNC projects. This HAT supports up to 2 stepper motors. The python library works identically for bi-polar and uni-polar motors
Running a stepper is a little more intricate than running a DC motor but its still very easy
 
Note: need to external power supply to the motor power supply
Connecting Stepper Motors
For unipolar motors: to connect up the stepper, first figure out which pins connected to which coil, and which pins are the center taps. If its a 5-wire motor then there will be 1 that is the center tap for both coils. Theres plenty of tutorials online on how to reverse engineer the coils pinout. The center taps should both be connected together to the GND terminal on the Motor HAT output block. then coil 1 should connect to one motor port (say M1 or M3) and coil 2 should connect to the other motor port (M2 or M4).
For bipolar motors: its just like unipolar motors except theres no 5th wire to connect to ground. The code is exactly the same.
For this demo, please connect it to M1 and M2
Now go into the Raspi_MotorHAT / folder and run sudo python StepperTest.py to watch your stepper motor spin back and forth.
Stepper motor control 
Here's the code which shows you everything the MotorHAT library can do and how to do it.Copy 
The MotorHAT library contains a few different classes, one is the MotorHAT class itself which is the main PWM controller. You'll always need to create an object, and set the address. By default the address is 0x6F (see the stacking HAT page on why you may want to change the address)
Copy Code
1.	# create a default object, no changes to I2C address or frequency
2.	mh = Raspi_MotorHAT(addr = 0x6F)
Even though this example code does not use DC motors, it's still important to note that the PWM driver is 'free running' - that means that even if the python code or Pi linux kernel crashes, the PWM driver will still continue to work. This is good because it lets the Pi focus on linuxy things while the PWM driver does its PWMy things.
Stepper motors will not continue to move when the Python script quits, but it's still strongly recommend that you keep this 'at exit' code, it will do its best to shut down all the motors:
Copy Code
1.	# recommended for auto-disabling motors on shutdown!
2.	def turnOffMotors():
3.	        mh.getMotor(1).run(Raspi_MotorHAT.RELEASE)
4.	        mh.getMotor(2).run(Raspi_MotorHAT.RELEASE)
5.	        mh.getMotor(3).run(Raspi_MotorHAT.RELEASE)
6.	        mh.getMotor(4).run(Raspi_MotorHAT.RELEASE)
7.	 
8.	atexit.register(turnOffMotors)
Creating the Stepper motor object
OK now that you have the motor HAT object, note that each HAT can control up to 2 steppers. And you can have multiple HATs!
To create the actual Stepper motor object, you can request it from the MotorHAT object you created above with getStepper(steps, portnum) where steps is how many steps per rotation for the stepper motor (usually some number between 35 - 200) ith a value between 1 and 2. Port #1 isM1 and M2, port #2 is M3 and M4
Copy Code
1.	myStepper = mh.getStepper(200, 1)       # 200 steps/rev, motor port #1
Next, if you are planning to use the 'blocking' step() function to take multiple steps at once you can set the speed in RPM. If you end up using oneStep() then this step isn't necessary. Also, the speed is approximate as the Raspberry Pi can't do precision delays the way an Arduino would. Anyways, we wanted to keep the Arduino and Pi versions of this library similar so we keptsetSpeed() in:
Copy Code
1.	myStepper.setSpeed(30)                  # 30 RPM
Using "Non-blocking" oneStep()
OK lets say you want a lot of control over your steppers, you can use the one oneStep(direction, stepstyle) which will make a single step in the style you request, with no delay. This will let you step exactly when you like, for the most control
 
Connecting Servos
 
Note: need to external power supply to the motor power supply

Connecting a Servo
Most servos come with a standard 3-pin female connector that will plug directly into the headers on the Motor HAT headers. Be sure to align the plug with the ground wire (usually black or brown) with the bottom row and the signal wire (usually yellow or white) on the top.

Adding More Servos
Up to 4 servos can be attached to one board. If you need to control more than 4 servos, additional boards can be stacked as described on the next page.
Note:MotoHAT’s servo channel: 0,1,14,15

Stepper Servo control 
For this demo, please connect it to Channel 0
Now go into the Raspi_MotorHAT / folder and run sudo python ServoTest.py to watch your servo spin back and forth.

