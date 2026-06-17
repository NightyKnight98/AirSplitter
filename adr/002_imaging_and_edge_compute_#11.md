# Architectural Decision Record (ADR-002)
## Trade Study: Imaging Architecture & Edge-Computing Method

### 1. Status
**Proposed** (Linked to Issue #11)

### 2. Context & Problem Statement
The baseline configuration of the AirSplitter project utilized an Arducam module connected to a Raspberry Pi Zero 2 W, which passed a raw video stream down to a Ground Control Station (GCS) laptop. The GCS was scoped to handle object detection models. 

However, live bench testing and long-distance link-budget calculations revealed that transmitting high-bandwidth raw video over a standard wireless connection introduces unmanageable packet drops, high transmission latency, and range degradation. The system requires an architecture that ensures reliable long-distance video telemetry while preserving the capability to see real-time target bounding boxes.

### 3. Evaluated Options

#### Option A: Distributed Edge Architecture (Baseline)
* **Hardware:** Arducam + Pi Zero 2 W + Custom High-Bandwidth Wi-Fi Link.
* **Logic:** Raw video transmitted to the ground; inference happens entirely on the laptop.
* **Pros:** Low processing load on the aircraft's local battery cells.
* **Cons:** High link susceptibility; a single drop in signal kills the detection pipeline entirely. High bandwidth requirement limits physical operational range.

#### Option B: True Edge Compute & OpenIPC Pipeline (Proposed Migration)
* **Hardware:** RunCam WiFiLink2-G VTX (Sony IMX415 Sensor) + Pi Zero 2 W + OpenIPC Open-Source Firmware + RTL8812AU Ground Card.
* **Logic:** The Sony Camera streams directly to the Pi Zero 2 W. The Pi handles the CV inference models at the edge, draws the target bounding boxes directly onto the video frames in memory, and pipes the compiled stream over a low-bandwidth (~15Mbps) 5.8GHz injection link via the VTX.
* **Pros:** Extreme long-range performance due to highly optimized OpenIPC encoding. Low-bandwidth requirement. Resilience against transmission drops (if the signal drops, the plane still independently runs tracking models).
* **Cons:** Increases thermal output on the airframe; requires careful power routing from the Mateksys F405 BEC rails.

### 4. Decision Outcome
**Selected: Option B (True Edge Compute & OpenIPC Pipeline).** 

#### Expected System Impact:
* **True Edge Processing:** The Raspberry Pi Zero 2 W shifts from a passive transmitter to an active localized data-processing engine running optimized lightweight models (e.g., YOLOv8-nano via ONNX Runtime).
* **Downlink Reliability:** OpenIPC lowers the data bandwidth ceiling, ensuring a solid video stream to the UVC network card on the ground.
* **ConOps Adjustment:** The system now guarantees that bounding boxes are baked directly into the video stream before transmission, eliminating ground-side latency sync issues.

### 5. Constraints & Mitigation
* **Thermal Management:** The RunCam WiFiLink2-G features an integrated cooling fan; it must be mounted in an open airframe cutout to maintain constant airflow.
* **Power Regulation:** The VTX draws up to 15W peak. It must be powered cleanly from the 9V/12V filtered BEC pads on the Mateksys F405-WING-V2, never from the Pi's GPIO pins.
