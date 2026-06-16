# Operational Environment & Regulatory Framework

## 1. Regulatory Context & Compliance Matrix
The AirSplitter Unmanned Aircraft System (UAS) operates strictly under the legal boundaries defined by the Federal Aviation Administration (FAA). Because this asset performs autonomous object detection and custom payload routing development, it complies with the following explicit frameworks:

### 1.1 Primary Framework: FAA Part 107 (Commercial/Research)
*   **Operating Authority:** Conducted under FAA Advisory Circular (AC) 107-2A. 
*   **Airspace Restriction:** Restricted entirely to **Class G (Uncontrolled) Airspace** up to the baseline structural ceiling. Operations within Class B, C, D, or E surface areas require explicit LAANC (Low Altitude Authorization and Notification Capability) digital clearance prior to battery initialization.
*   **Visual Line of Sight (VLOS):** All flights must maintain un-aided VLOS with a designated Visual Observer (VO) assisting the Remote Pilot in Command (RPIC). The 0.75-mile downlink range limit is the absolute theoretical boundary; physical flight paths will truncate if atmospheric haze degrades direct visual monitoring.

### 1.2 Registration & Identification
*   **FAA Digital Registration:** The airframe must display its unique FAA registration number externally via permanent marker or engraving.
*   **Remote ID (RID) Compliance:** Because the takeoff weight exceeds 250 grams (0.55 lbs), the aircraft must transmit active Remote ID telemetry broadcast signals via an onboard broadcast module or a firmware-integrated flight controller broadcast bus.

---

## 2. Preliminary Environmental Operating Envelope

*   **Altitude Limits:** Maximum **400 feet AGL (Above Ground Level)** per FAA § 107.51, with local operational flight testing targets set to a conservative 150–250 feet AGL to maximize the ArduCam's downward pixel density for object detection.
*   **Airspace Environment:** Class G Uncontrolled. Zero operations permitted over unprotected human assemblies or moving vehicles (§ 107.39).
*   **Geographic Obstructions:** Dense rural tree canopies. Flight paths must maintain a minimum 50-foot vertical clearance buffer above the highest documented treeline canopy to mitigate localized mechanical wind shear and signal degradation.

---

## 3. Physical, Geographic, and RF Environment Constraints

### 3.1 Physical Environment & Material Boundaries (Paper-Backed Foam Board)
Because the airframe is fabricated from paper-backed polystyrene foam board, structural integrity is directly bound to immediate atmospheric conditions:
*   **Temperature Range:** **40°F to 90°F (4.4°C to 32.2°C).** Temperatures below 40°F cause the hot-glue joints and plastic control horns to become brittle and crack under stress. Temperatures exceeding 90°F cause localized skin warping and weaken internal adhesive bonds, risking structural failure under high-G maneuvers.
*   **Humidity Limitation:** **Maximum 75% Non-Condensing.** High relative humidity softens the paper-skin layer of the foam board. This completely destroys the tension-bearing skin strength, leading to wing warping or catastrophic mid-air main-wing structural failure.
*   **Precipitation Limit:** **0.0 mm/hr (Strict Zero-Tolerance).** Flight operations during active rain, drizzle, or heavy morning fog are prohibited. Moisture delaminates the paper backing from the foam core instantly and will flood the internal fuselage, causing immediate shorts on the uninsulated Raspberry Pi and Mateksys circuitry.

### 3.2 Geographic Operational Site Boundaries
Testing will be restricted to a designated **unpopulated rural open field / private test range** matching the following site-specific profile:
*   **Takeoff/Landing Surface:** Low-cut grass or smooth dirt clearing. Due to the lack of rigid landing gear on a lightweight 3 lb pusher airplane, the airframe will be hand-launched and belly-landed. 
*   **Obstruction Clearance:** Ground track boundaries must maintain a minimum 150-foot lateral buffer away from residential power lines, tall silos, and dense wooded treelines. Belly-landings must be executed in clearings free of rocks and thick brush to prevent tearing the fragile underbelly foam skin.

### 3.3 Radio Frequency (RF) Environment & Interference Matrix
The system utilizes a dual-link radio topology (2.4GHz for control link, 5.8GHz for OpenIPC video data stream). The operating site RF characteristics are defined below:
*   **Control Link (2.4GHz ExpressLRS / ELRS):** Operating with a minimum 250mW transmission packet output power profile, the ELRS link provides a theoretical line-of-sight range exceeding 5 miles, far eclipsing the 0.75-mile mission limit. 
*   **Interference Mitigation:** 
    *   *Co-site Interference:* The 2.4GHz RC control receiver antennas must be physically separated from the 5.8GHz OpenIPC transmitter antennas inside the fuselage by at least 8 inches to prevent the high-power video signal from drowning out (RF desensing) the control link.
    *   *Environmental Interference:* Testing sites must be chosen away from industrial 2.4GHz/5.8GHz commercial Wi-Fi routers, high-voltage overland power lines, and cellular repeating towers to maintain a clean RF noise floor.

## 4. LiPo Battery Thermodynamic Operating Limits & Endurance Planning

### 4.1 Temperature-Induced Performance Deltas
The AirSplitter propulsion and edge-computing arrays are powered by a central Lithium-Polymer (LiPo) battery pack. Chemical discharge efficiency is bound to the following temperature-dependent operational envelopes:
*   **Optimal Range [60°F to 90°F / 15.5°C to 32.2°C]:** Baseline flight performance. Internal resistance is minimized, allowing consistent nominal current delivery to the Cobra 60A ESC. Expected flight endurance: **~22–25 minutes** at full cruise throttle.
*   **Cold Weather Degradation Threshold [<50°F / 10°C]:** Severe drop in ion mobility spikes internal resistance. Attempting launch bursts triggers instantaneous voltage sag. *Endurance Safety Factor:* Flight planning timelines must automatically be truncated by **30% to 40%** (~13–15 minute flight ceilings maximum) to prevent unannounced cell exhaustion. 
*   **Thermal Safety Limit [>110°F / 43.3°C]:** High ambient temperatures combined with high-current internal discharge heat risk triggering structural swelling ("puffing") and internal layer short-circuits. Operations must be suspended to prevent catastrophic thermal runaway over wooded zones.

### 4.2 Pre-Flight Mitigations
For ambient operations below 55°F, batteries must be stored inside insulated, heated transport containers prior to airframe insertion. Launching with a cold-soaked battery pack is a strict flight safety violation.

---

![AirSplitter Operational Environment Diagram](docs/00_ConOps/assets/Operational_Environment_Diagram.png)

![AirSplitter Operational Environment Aerial View](docs/00_ConOps/assets/true_aerial_view_operating_environment.png)

![AirSplitter Operational Environment Aerial View](docs/00_ConOps/assets/true_front_view_operating_environment.png)