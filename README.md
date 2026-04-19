# FireSense-AI

FireSense-AI is a multi-modal fire detection system that combines computer vision and sensor-based temporal analysis using Raspberry Pi and ESP32. The system integrates image-based predictions with real-time sensor data to provide reliable fire detection.

---

## Overview

This project uses a fusion approach:

* Image-based detection using EfficientNetV2
* Sensor-based temporal modeling using TCN (Temporal Convolution Network)
* Final decision using a meta-model

The system reads live sensor data from ESP32 and camera input from Raspberry Pi, processes both streams, and outputs a fire or safe prediction.

---

## Hardware Requirements

* Raspberry Pi (64-bit OS recommended)
* ESP32 microcontroller
* USB camera compatible with Raspberry Pi
* Sensors:

  * MQ2 Gas Sensor
  * MQ135 Air Quality Sensor
  * Temperature Sensor
  * Flame Sensor
  * Ultrasonic Sensor
* USB cable for ESP32 connection
* Jumper wires and breadboard

---

## Hardware Setup

1. Connect all sensors to the ESP32:

   * MQ2
   * MQ135
   * Temperature sensor
   * Flame sensor
   * Ultrasonic sensor

2. Update pin configurations in the ESP32 code according to your wiring.

3. Upload the ESP32 code:

   * Navigate to the `esp32/` folder
   * Upload the `.ino` file using Arduino IDE or PlatformIO

4. Connect ESP32 to Raspberry Pi via USB.

5. Connect the USB camera to Raspberry Pi.

---

## Project Setup

1. Clone the repository:

```bash
git clone https://github.com/Shreesh-125/FireSense-AI.git
cd FireSense-AI
```

---

## Models

The trained model files are not included in this repository due to size constraints.

You can generate the models using the training scripts provided in the project(Will be uploaded if not).

Ensure that the following files are generated and placed inside same directory as infer.py before running:

* effnetv2s_model.keras
* tcn_model.keras
* meta_model.pkl
* scaler_tcn.pkl
* meta.pkl

---

## Dataset

A small sample dataset is included in the repository under the `dataset/` folder.

* This sample is provided only for demonstration and testing purposes.
* It is not sufficient to train the model.
* You should use your own dataset or extend the dataset for proper training.

---

## Virtual Environment Setup

Create and activate a virtual environment:

```bash
python3 -m venv venv
source venv/bin/activate
```

Install dependencies:

```bash
pip install -r requirements.txt
```

---

## Running the System

```bash
python infer.py
```

---

## Important Notes

* Ensure ESP32 is connected and detected (e.g., `/dev/ttyUSB0`)
* Ensure camera is properly connected
* Ensure all model files are present in the `models/` folder

---

## Initial Behavior

The system requires an initial sequence of sensor data before making predictions.

* It collects approximately `SEQ_LEN` (e.g., 30) data points
* During this phase, no fire/safe output will be shown
* This is expected behavior

Once sufficient data is collected, predictions will begin.

---

## Output

The system prints:

* SAFE → No fire detected
* FIRE ALERT → Fire detected

---

## Project Structure

```
FireSense-AI/
├── infer.py
├── requirements.txt
├── README.md
├── esp32/
├── dataset/        (sample data only)
```

---

## Tech Stack

* Python
* TensorFlow / Keras
* OpenCV
* scikit-learn
* ESP32 (Arduino)

---
