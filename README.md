# Odometry-Computer
The goBILDA Odometry Computer is a device designed to solve the Pose Exponential calculation commonly associated with Dead Wheel Odometry systems. It reads two encoders, and an integrated system of senors to determine the robot's current heading, X position, and Y position. 

it uses an ESP32-S3 as a main cpu, with an STM LSM6DSV16X IMU. It is validated with goBILDA "Dead Wheel" Odometry pods, but should be compatible with any quadrature rotary encoder. The ESP32 PCNT peripheral is speced to decode quadrature encoder signals at a maximum of 40mhz per channel. Though the maximum in-application tested number is 130khz.

The device expects two perpendicularly mounted Dead Wheel pods. The encoder pulses are translated into mm and their readings are transformed by an "offset", this offset describes how far away the pods are from the "tracking point", usually the center of rotation of the robot.

Dead Wheel pods should both increase in count when moved forwards and to the left. The gyro will report an increase in heading when rotated counterclockwise.

The Pose Exponential algorithm used is described on pg 181 of this book: https://github.com/calcmogul/controls-engineering-in-frc

#### User input variables are:
* xPodOffset = this describes how far sideways the X pod is from the center of robot rotation. left increases. 0 is the point that the device tracks.
* yPodOffset = this describes how far forward the Y pod is from the center of robot rotation. forward increases. 0 is the point that the device tracks.
* yawOffset = this is a user-tunable variable that scales the heading up or down. This is tested best by rotating the robot 10 times and recording the measured total rotation, then dividing the expected result by the measured number.
* mmPerTick = mm per quadrature tick of the dead wheel pods used.
* xReversed = revereses the X pod, so instead of increasing in count, it decreases.
* yReversed = revereses the Y pod, so instead of increasing in count, it decreases.
