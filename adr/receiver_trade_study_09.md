# Trade Study: Ground Control Station (GCS) Receiver Architecture

## 1. Objective
Evaluate and select the most cost-effective ground-side radio receiver infrastructure capable of reliably closing the 5.8GHz OpenIPC data link over a 0.75-mile operational envelope.

## 2. Technical and Financial Evaluation Matrix

| Engineering Metrics | Option A: Standalone RunCam VRX Box | Option B: High-Power USB Network Card (Selected) |
| :--- | :--- | :--- |
| **Unit Hardware Cost** | **$150.00+** | **~$20.00** (Included in WiFiLink2-G bundle) |
| **GCS Component Footprint** | Large (Requires external battery, Mini-HDMI cable, and UVC capture card) | Minimal (Plugs directly into Laptop USB-A or USB-C port) |
| **Primary Risk Factor** | High capital expenditure; adds mechanical failure points (extra cables) | Lower out-of-the-box omnidirectional range |
| **Geographic Mitigations** | Required for high-attenuation wooded areas | **Sufficient for Open-Field/Clear Line-of-Sight environments** |

## 3. Engineering Justification & Pivot
Initial design constraints assumed a heavily wooded rural environment, which favored the $150 VRX Box due to its 4-port antenna diversity and internal signal amplification. 

However, the geographic operational profile has been refined to a **completely open-field private test range**. This transition eliminates tree canopy signal absorption, providing a clean, un-attenuated Line-of-Sight (LOS) RF path. 

### 3.1 Financial Optimization
By choosing the **$20 USB Network Card** over the $150 standalone hardware box, the project achieves an immediate **86% reduction in ground-station hardware expenditure ($130.00 total savings)**. 

### 3.2 Performance Mitigation Strategy
To guarantee the 0.75-mile range limit is safely closed using the budget network card, the stock linear omni-directional stubby antennas will be unscrewed. The card will be upgraded with a high-gain **5.8GHz LHCP Directional Patch Antenna ($15.00)** mounted to a ground tripod. This setup focuses the receiver's gain cone directly toward the aircraft's flight boundaries, matching the effective operational range of the VRX box within an open field at a fraction of the cost.
