### Definition

- Embedded system: any device that includes a computer but is not itself a general-purpose computer.
- Hardware and Software - part of some larger systems and expected to function without human intervention
- Respond, monitor, control external environment using sensors and actuators

### Embedding a computer

Simplest model

### Characteristics of embedded systems

- Sophisticated functionality.
- Real-time operation (always?).
- Low manufacturing cost.
- Application dependent Processor (?)
- Restricted Memory
- Low power.
- Power consumption is critical in battery-powered devices.
- Excessive power consumption increases system cost even in wall-powered devices.

### Manufacturing Cost

- Manufacturing cost has different components.
- Non-recurring Engineering cost for design and development;
- cost of production and marketing each unit;
- Best technology choice will depend on the number of units we plan to produce

### Real-time operation

- Must finish operations by deadlines.
- Hard real time: missing deadline causes failure.
- Soft real time: missing deadline results in degraded performance.
- Many systems are multi-rate: must handle operations at widely varying rates.

### Application dependent requirements

- Fault-tolerance
- Continue operation despite hardware or software faults
- Safe
- Systems to avoid physical or economic damage to person or property

### More Features

- Dedicated systems
- Predefined functionality – accordingly hardware and software designed
- Programmability rarely used during lifetime of the system
- Real-time, fault-tolerant, safe

### Present Technology Landscape of Embedded Systems

### Examples

- Printer, Camera
- Mobile phone.
- Automobile: engine, brakes, dash, etc.
- Television.
- Household appliances.
- Surveillance Systems.
- Medical Devices

### Mobile

Phone 16 is built for Apple Intelligence, the personal intelligence system that helps you write, express yourself and get things done effortlessly. With groundbreaking privacy protections, no one else can access your data — not even Apple. Powered by advanced intelligence and Spatial Audio capture, Audio Mix lets you adjust the way voices sound in your videos using three different voice options.

Spec

Super Retina XDR display

OLED display, 2556x1179-pixel resolution at 460 ppi

A18 chip

New 6‑core CPU

New 5‑core GPU

New 16‑core Neural Engine

8GB RAM

Advanced dual-camera system, 48MP Fusion: 26 mm, ƒ/1.6 aperture, Also enables 12MP 2x Telephoto:

iOS 18 OS with Apple Intelligence

Apple Intelligence combines generative models with a user's personal context to provide tailored information.

5G (sub‑6 GHz) with 4x4 MIMO

6

Gigabit LTE with 4x4 MIMO and LAA

6, Wi‑Fi 7 (802.11be) with 2x2 MIMO

7, Bluetooth 5.3

### Automobile as Embedded System: Automotive Electronics

- Automotive electronics are any electrically-generated systems used in road vehicles.
- Automotive electronics originated from the need to control Engines.
- The first electronic pieces were used to control engine functions and were referred to as engine control units (ECU).
- A modern car may have up to 100 ECU's and a commercial vehicle up to 40.

### What is an ECU?

In the Automobile industry an electronic control unit (ECU) is an embedded electronic device, basically a digital computer, that reads signals coming from sensors placed at various parts and in different components of the car and depending on this information controls various important units e.g. engine and other automated operations within the car among many

### ECU(Electronic Control Unit) and its mount location

### Types of ECU

ECM - Engine Control Module

EBCM - Electronic Brake Control Module

PCM – Power train Control Module

VCM - Vehicle Control Module

BCM - Body Control Module

### What an ECU Does?

The ECU uses closed-loop control, a control scheme that monitors outputs of a system to control the inputs to a system, managing the emissions and fuel economy of the engine (as well as a host of other parameters).

Gathering data from dozens of different sensors, the ECU performs millions of calculations each second, including looking up values in tables, calculating the results of long equations to decide on the best spark timing or determining how long the fuel injector is open.

### Applications

Depending upon the nature of the circuit the Engine mappings can change completely. On slower and twister tracks, the engine control system will help the driver have more control on the throttle input by making the first half of the pedal movement very sensitive.

At high speed circuits, the driver has to jump on the throttle more, rather than gradually applying full throttle. The accelerator will be set so that only a small movement will result in full engine acceleration

### Example: Automobile

### Automotive Sensors

### TYPES OF SENSORS

- ENGINE SENSORS
    - Oxygen Sensor
    - Throttle Position Sensor
    - Crankshaft Position Sensor
    - MAP Sensor
    - Engine Coolant Temperature Sensor
    - Mass Air Flow Sensor

### OXYGEN SENSOR

- This sensor is used in the mechanism serving for air
- fuel ratio measurement, it is installed in the exhaust
- system of the vehicle, attached to the engine's
- exhaust manifold, the sensor measures the ratio of
- the air-fuel mixture.

### THROTTLE POSITION SENSOR

A throttle position sensor is a sensor used to monitor the position of the throttle in an internal combustion engine. The sensor is usually located on the butterfly spindle so that it can directly monitor the position of the throttle valve butterfly.

### CRANK POSITION SENSOR

A crank position sensor is a component used in an internal combustion engine to monitor the position or rotational speed of the crankshaft. This information is used by engine management systems to control ignition system timing and other engine parameters.

### Coolant Temperature Sensor

- The coolant temperature sensor is used to measure the temperature of the engine coolant of an internal combustion engine.
- The coolant temperature sensor works using resistance. As temperature subjected to the sensor increases the internal resistance changes.
- Depending on the type of sensor the resistance will either increase or decrease.

### Mass Air Flow Sensor

A mass air flow sensor (MAF) is used to find out the mass flow rate of air entering a fuel injected internal combustion engine.

The air mass information is necessary for the engine control unit (ECU) to balance and deliver the correct fuel mass to the engine.

Air changes its density as it expands and contracts with temperature and pressure.

### SPEED SENSORS

Speed sensors are machines used to detect the speed of an object, usually a transport vehicle.They include:

- Wheel speed sensors
- Speedometers
- Pitometer logs
- Pitot tubes
- Airspeed indicators
- Piezo sensors (e.g. in a road surface)
- LIDAR
- ANPR (where vehicles are timed over a fixed distance)

### WHEEL SPEED SENSOR

A wheel speed sensor or vehicle speed sensor (VSS) is a type of tachometer.

It is a sender device used for reading the speed of a vehicle's wheel rotation. It usually consists of a toothed ring and pickup.

### SPEEDOMETER

A speedometer is a device that measures the instantaneous speed of a land vehicle.

The various types of speedometers include:

- Eddy current speedometer
- Electronic speedometer
- Bicycle speedometer

### LIDAR

LIDAR (Light Detection And Ranging) is an optical remote sensing technology that measures properties of scattered light to find range and/or other information of a distant target.

The prevalent method to determine distance to an object or surface is to use laser pulses.

### TYPES OF VEHICLE SENSORS

- Rain Sensor
- Parking sensor
- Air Conditioning Sensor
- Oil sensor
- Fuel gauge
- Radar gun
- Water Sensor

### RAIN SENSORS

Automotive rain sensors detect rain falling on the windshield of a vehicle. One of the more common rain sensor implementations employs an infrared light that is beamed at a 45-degree angle onto the windshield from inside the car. If the glass is wet, less light makes it back to the sensor, and the wipers turn on.

### PARKING SENSORS

Parking sensors are proximity sensors for road vehicles which can alert the driver to unseen obstacles during parking manoeuvres.

The ultrasonic sensors are currently available in several brands of cars. Some systems are also available as additional upgrade kits for later installation.

### FUEL GAUGE

A fuel gauge is an instrument used to indicate the level of fuel contained in a tank. As used in cars, the gauge consists of two parts:

- The sensing unit
- The indicator

### Actuators

### What is an actuator?

- Actuators are devices used to produce action or motion.
- Input(mainly electrical signal , air, fluids)
- Electrical signal can be low power or high power.
- Actuators output can be position or rate i. e. linear displacement or velocity.

### Classification of Actuators

- Hydraulic
- Electrical/Electronic
- Pneumatic

### Hydraulic Actuator

A hydraulic drive system is a drive or transmission system that uses pressurized hydraulic fluid to drive hydraulic machinery.

The term "hydraulic actuator" refers to a device controlled by a hydraulic pump.

### Parts of a Typical Cylinder

- Single- and Double-Acting Cylinders

### Electrical Actuator

- Electrically actuated system are very widely used in control system.
    
    Working Principle of motor
    
    Every motor works on the principle that when a current-carrying conductor is placed in a magnetic field, it experiences a mechanical force.
    
- There are three types of motor used in control system
    
    - D.C. motor
    - A.C. motor
    - Stepper motor
- A.C. Motor
    
- Stepper Motor
    
    A stepper motor is an electromechanical device which converts electrical pulses into discrete mechanical movements.
    
    - Permanent magnet type
    - Variable reluctance type
    - Hybrid type

### Other types of Actuators

Heaters - used with temperature sensors And temperature controller to control the temperature in automated moulding Equipment and in soldering operation.

Lights - Lights are used on almost all machines to indicate the machine state and provide feedback to the operator.

* LED

* LCD’s

* Gas plasma display

* CRT

Sirens/Horns - Sirens or horns can be useful for unattended or dangerous machines to make conditions well known.

### ADAS: Advance Driver Assist Systems

- The ADAS systems were designed to help the driver in the driving process.
- These technologies automate, facilitate, and improve vehicular systems to assist drivers for safe and better driving.
- ADAS systems help drivers in avoiding on-road collisions by generating alerts on potential hazards.
- The Advanced Driver Assistance Systems are also helpful for different kinds of vehicle fleets such as trucking fleets, delivery fleets, taxi cab fleets, commercial fleets, etc.

### Sensors in ADAS

1. Cameras: Cameras are used to detect and analyze the visual environment, including road markings, traffic signs, and other vehicles.
2. Radar: Radar sensors use radio waves to detect the distance and speed of objects in the vehicle’s path, helping the system to detect and avoid collisions.
3. Lidar: Lidar sensors use laser light to create a 3D map of the vehicle’s surroundings, providing accurate information about the distance and position of objects.
4. Ultrasonic Sensors: Ultrasonic sensors are used for parking assistance systems and can detect obstacles and other vehicles at low speeds.

### Sensors (contd.)

1. GPS: GPS technology is used to determine the vehicle’s location, speed, and direction of travel.
2. Inertial Measurement Units (IMUs): IMUs are used to measure the vehicle’s acceleration, speed, and orientation, which is useful for controlling the vehicle’s stability and performance.
3. Microphones: Microphones can be used to detect driver behavior, including voice commands and any audible warning signals issued by the ADAS.

### Sensors in ADAS

### Applications

1. Collision Avoidance: ADAS systems such as forward collision warning, automatic emergency braking, and pedestrian detection can help prevent accidents by warning drivers of potential collisions and automatically applying the brakes if necessary.
2. Lane Departure Warning: ADAS systems can detect when a vehicle is drifting out of its lane and alert the driver to take corrective action.
3. Blind Spot Monitoring: ADAS systems can detect when another vehicle is in a driver’s blind spot and alert the driver to prevent accidents during lane changes.

### Applications

1. Adaptive Cruise Control: ADAS systems can maintain a safe distance from the vehicle in front, adjusting speed automatically to prevent collisions.
2. Parking Assistance: ADAS systems can help drivers park their vehicles safely and efficiently, with features such as automated parking and 360-degree cameras.
3. Night Vision: ADAS systems can enhance visibility in low-light conditions by using infrared sensors and cameras to detect objects on the road.

### Applications

1. Traffic Sign Recognition: ADAS systems can recognize and interpret traffic signs such as speed limits and stop signs, and provide alerts to the driver if necessary.
2. Driver Drowsiness Detection: Driver drowsiness detection is a technology used in vehicles to monitor the driver’s level of fatigue or drowsiness, and alert them if necessary. The aim of this technology is to prevent accidents caused by drowsy driving, which can be as dangerous as driving under the influence of alcohol.
3. GPS Navigation: GPS navigation is a key feature in many advanced driver assistance systems (ADAS). GPS (Global Positioning System) technology enables vehicles to determine their location and provides turn-by-turn directions to a destination.

### Applications

1. Hill Descent Control: Hill Descent Control (HDC) is an advanced driver assistance system (ADAS) designed to help drivers maintain control of their vehicle when driving down steep or slippery slopes. The system uses a combination of sensors, brakes, and engine power to ensure the vehicle remains at a safe speed and doesn’t slide or skid out of control.
2. Intelligent Speed Adaptation: Intelligent Speed Adaptation (ISA) is an advanced driver assistance system (ADAS) designed to help drivers adhere to posted speed limits and reduce the risk of accidents caused by speeding. The system uses a combination of GPS technology, digital mapping, and speed limit recognition to monitor the vehicle’s speed and provide feedback to the driver.

### Applications

1. Lane Departure Warning System: Lane Departure Warning (LDW) is an advanced driver assistance system (ADAS) that helps drivers avoid unintentional lane departures while driving. The system uses a camera or other sensors to detect the vehicle’s position relative to the lane markings on the road, and provides feedback to the driver if the vehicle starts to drift out of its lane. AI Algorithms on Embedded Platform