markdown# Fused Logger Core (`fused-logger-core`)

The open-source core engine for the world's first chamber-specific environmental data logger featuring **native AI anomaly detection** and **multispectral point-of-capture fusion**.

`fused-logger-core` synchronizes time-series sensor data (temperature, humidity) with real-time Long-Wave Infrared (LWIR) and Short-Wave Infrared (SWIR) imaging directly at the edge, eliminating data time-drift and manual verification layers.

---

## 🚀 Key Framework Modules

The core architecture is divided into two primary diagnostic engines:

*   **Product-Under-Test (PUT) Engine:** Maps thermal arrays to individual PCB components. Tracks micro-thermal runaway and aligns infrared telemetry directly with automotive CAN/Ethernet OBD fault codes.
*   **Chamber Infrastructure Engine:** Monitors the validation envelope. Tracks wide-angle SWIR door gasket degradation, micro-frost buildup, and atmospheric plume shifts.

---

## 🛠️ System Architecture Diagram

```text
+-------------------------------------------------------------------------+

|                        PHYSICAL INGESTION LAYER                         |
|  [Modbus/MQTT Clients]   +   [LWIR Thermal Stream]   +   [SWIR Matrix]  |
+------------------------------------+------------------------------------+
                                     |
                                     v
+-------------------------------------------------------------------------+

|                        EDGE SMART LOGGER MATRIX                         |
|  - Frame Synchronization & Millisecond Alignment Engine                |
|  - Local Time-Series Buffer (Encrypted SQLite / Edge DB)                |
|  - Local Embedded AI Inference Models (PUT + Infrastructure Modules)    |
+------------------------------------+------------------------------------+
                                     |
                                     v
+-------------------------------------------------------------------------+

|                        EXPORT & INTERACTION LAYER                       |
|  [Real-Time Dashboard API]   +   [Cryptographic Compliance Reporting]  |
+-------------------------------------------------------------------------+
```

---

## 📦 Getting Started

### Prerequisites
* Python 3.10+
* OpenBLAS / CUDA (for accelerated edge inference)
* `libmodbus` or equivalent system message brokers

### Quick Installation
```bash
git clone https://github.com
cd fused-logger-core
pip install -r requirements.txt
```

### Basic Integration Example
```python
import fused_logger_core as flc

# Initialize connection to local edge unit
node = flc.LoggerNode(ip_address="192.168.1.100")

# Configure ingestion pipeline 
config = flc.PipelineConfig()
config.enable_thermal_stream(resolution=(320, 240), frame_rate=30)
node.apply_config(config)

# Capture fused frame packets
for fused_frame in node.stream_fused_data():
    pcb_faults = flc.ai_evaluator.detect_pcb_anomalies(fused_frame.thermal_matrix)
    if pcb_faults.has_runaway_detected():
        print(f"Alert: Critical thermal exception at {pcb_faults.coordinates}")
```

---

## 📝 Compliance Tracking
The core engine generates unified data files cryptographically signed to maintain strict data lineage for high-stakes certification frameworks:
* **Automotive:** IATF 16949
* **Aerospace:** AS9100, DO-160, MIL-STD-810

---

## 📄 License
This project is licensed under the MIT License - see the `LICENSE` file for detai
