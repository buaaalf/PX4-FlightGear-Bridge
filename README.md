# FlightGear Bridge

The FlightGear alternative to the current PX4's mainstream simulator Gazebo.

![FlightGear SITL connected with PX4 and QGroundControl](art/screenshot.png)

This stand-alone application adding the possibility of the use of the FlightGear simulator. The system was tested on the [Rascal airplane](http://wiki.flightgear.org/Rascal_110) and [TF-G1](https://github.com/ThunderFly-aerospace/FlightGear-TF-G1) autogyro simulation models.

It connects to FG (over UDP generic protocol) and transforms the data to TCP MAVlink packets for the PX4 stack.

### How to run the development version:

1) Install FlightGear (FG). You should be able to run Flightgear by ```fgfs``` from the command line.
2) Install your required aircraft models ([Rascal_110](http://wiki.flightgear.org/Rascal_110) or [ThunderFly TF-G1](https://github.com/ThunderFly-aerospace/FlightGear-TF-G1) for example)
3) Set write permissions to the `Protocols` folder of the FlightGear (in ubuntu /usr/share/games/flightgear/Protocols)
4) Open [QgroundControl](http://qgroundcontrol.com/)
5) In PX4Firmware folder run: ```make px4_sitl_nolockstep flightgear_plane``` for plane or ```make px4_sitl_nolockstep flightgear_tf-g1``` for autogyro
6) Wait for connection of PX4 to the QGC.  It happens after full loading of FlightGear. 

### ADVANCED OPTIONS

You can tune your FG settings by the following environment variables:

1) FG\_BINARY - absolute path to FG binary to run. (It can be an AppImage)
2) FG\_MODELS\_DIR - absolute path to the folder containing the aircraft models which should be used for simulation.
3) FG\_ARGS\_EX - any additional FG parameters 

FlightGear Bridge and starting script now support multiple instances of PX4. FG\_run script takes the second argument, which is PX4 ID (and automatically adjust the port numbers according to given number) and bridge binary takes this ID as the first argument before the output of get\_FGbridge\_params.py

### Limitations

The PX4 is connected to FlightGear thought "[generic protocol](http://wiki.flightgear.org/Generic_protocol)", which is served synchronously to the simulator graphics engine frame rate. So the PX4 gets the sensor data in frequency, depending on graphics resources and the current scene. The source-code implements artificial upsampling of sensor data to ~100Hz in the order to avoid stale sensor detection. Random noise is added to the sensor data. 

The possible better approach is to obtain the FlightGear using an [HLA](http://wiki.flightgear.org/High-Level_Architecture) interface.

### Credits

 FlightGear bridge was initially developed at [ThunderFly s.r.o.](https://www.thunderfly.cz/) by Vít Hanousek <info@thunderfly.cz>
