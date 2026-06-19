# System Subsystem Decomposition Matrix

The AirSplitter Unmanned Aircraft System (UAS) is broken down into seven distinct functional subsystems, mapping physical hardware nodes to explicit technical domains:

## 1. Subsystem Breakdown Structure (SBS)

### 1.1 Propulsion Subsystem
*   **Primary Hardware Components:** Cobra C-2814/8 Brushless Motor (1850Kv), APC 7x5 Thin Electric Pusher Propeller.
*   **Functional Objective:** Converts electrical energy into aerodynamic thrust, maintaining airspeed limits above the 14-knot stall threshold.

### 1.2 Power Subsystem
*   **Primary Hardware Components:** Zeee 3S 11.1V 3200mAh 50C LiPo Battery, Current On-Off Electric Power Switch (XT60), XT60 12AWG Pigtail Adapter Cable.
*   **Functional Objective:** Manages raw current distribution, isolates high-draw motor spikes, and provides filtered, continuous step-down voltage rails to critical computing blocks.

### 1.3 Avionics & Flight Control Subsystem
*   **Primary Hardware Components:** Mateksys F405-WING-V2 Flight Controller (FC), Matek M10Q-5883 GNSS & Compass Module, and  Matek Digital Airspeed Sensor AS-DLVR-I2C
*   **Functional Objective:** Computes real-time inertial navigation, tracks spatial coordinates/altitude via GPS, executes automated stabilization loops, computes airspeed for stabilization, and injects MAVLink telemetry data streams into the video path.

### 1.4 RF & Communications Subsystem
*   **Primary Hardware Components:** RadioMaster RP3 ELRS 2.4GHz Nano Receiver, RadioMaster TX16S MK3 Radio Controller (Ground Station Transmitter).
*   **Functional Objective:** Establishes a highly secure, non-interfering 2.4GHz uplink for long-range pilot control commands and semi-autonomous flight mode updates.

### 1.5 Edge Computing & Video Subsystem
*   **Primary Hardware Components:** Raspberry Pi Zero 2 W Companion Computer, RunCam WiFiLink2-G VTX (with bundled Sony Camera and RTL8812AU-based USB Laptop Network Card).
*   **Functional Objective:** Captures real-time environment data, runs local Python/OpenCV computer vision object-detection scripts, and broadcasts low-latency 5.8GHz video frames down to the GCS Laptop.

### 1.6 Actuation Subsystem
*   **Primary Hardware Components:** TowerPro MG92B High-Torque Metal Gear Servos (4), 3-pin Servo Extension Cables.
*   **Functional Objective:** Deflects the control surfaces (Ailerons, Elevator, Rudder) to translate autopilot electronic stabilization commands into mechanical aircraft attitude adjustments.

### 1.7 Structures Subsystem
*   **Primary Hardware Components:** White Paper-Backed Foam Board (20" x 30"), Adhesives (Hot glue/epoxy), Heat shrink, Soldering connections.
*   **Functional Objective:** Forms the aerodynamic lift-generating geometries (airfoils) and provides the physical chassis (fuselage) protecting internal electrical components from high-G flight strains.


---

```mermaid
graph TD
    %% Power Subsystem Nodes
    Battery[Zeee 3S 3200mAh LiPo Battery] -->|11.1V Raw DC| Power_Switch[XT60 Current Power Switch]
    Power_Switch -->|12AWG Pigtail Bus| Matek_FC[Mateksys F405-WING-V2 Flight Controller]
    
    %% Propulsion Paths
    Matek_FC -->|VCC Battery Pass-Through Pads| Cobra_ESC[Cobra 60A FPV Wing ESC]
    Cobra_ESC -->|3-Phase AC Power| Cobra_Motor[Cobra C-2814/8 Brushless Motor]
    Cobra_Motor ===>|Mechanical Torque| APC_Prop[APC 7x5 Pusher Propeller]
    
    %% Main Avionics & Servo Power Distribution Hub
    Matek_FC -->|Vx Rail: 5.5V 8A BEC + PWM Signal| Servos[TowerPro MG92B Servos x4]
    
    %% Avionics Control Interfaces & Sensor Buses
    Matek_FC -->|DShot Telemetry Protocol| Cobra_ESC
    Matek_GNSS[Matek M10Q-5883 GNSS/Compass] <-->|UART2 Serial Data + I2C| Matek_FC
    Matek_Airspeed[Matek AS-DLVR Airspeed Sensor] <-->|I2C Shared Sensor Bus| Matek_FC
    
    %% Uplink Control Path
    RP3_Rx[RadioMaster RP3 ELRS 2.4GHz Rx] ==>|CRSF Telemetry Protocol| Matek_FC
    TX16S[RadioMaster TX16S MK3 GCS Controller] -.->|2.4GHz Wireless Link| RP3_Rx
    
    %% THE CRITICAL TELEMETRY CORRELATION FIX
    Matek_FC ===>|Dedicated UART3: MAVLink Telemetry Stream| Pi_Zero[Raspberry Pi Zero 2 W]
    
    %% Edge Compute & OpenIPC Downlink Network
    Matek_FC -->|Filtered 5V 2A Power Rail| Pi_Zero
    Matek_FC -->|Filtered 12V 2A Video Rail| OpenIPC_VTX[RunCam WiFiLink2-G VTX]
    Matek_FC <-->|UART1 Serial MAVLink Telemetry| OpenIPC_VTX
    
    %% Streamlined Video Pipeline
    OpenIPC_Cam[RunCam Bundled Sony Camera] -->|Native MIPI Ribbon Bus| Pi_Zero
    Pi_Zero -->|USB High-Speed Data Bridge with YOLO Overlays| OpenIPC_VTX
    
    %% Ground Control Infrastructure
    OpenIPC_VTX -.->|5.8GHz Wireless Video + Alert Overlay Data| Laptop_NetCard[RTL8812AU USB Laptop Network Card]
    Laptop_NetCard -->|Native USB Bus Port| GCS_Laptop[Ground Control Station Laptop]

```


