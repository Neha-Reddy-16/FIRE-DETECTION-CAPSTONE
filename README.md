# Multi-Modal Fire Detection System using EfficientNetV2, TCN and Meta-Learning with IoT Sensors

This project presents a multi-modal fire detection system that combines image-based deep learning with IoT sensor-based temporal analysis. The system is designed to improve reliability by using both visual information and environmental sensor data instead of relying on a single source.

The implementation uses EfficientNetV2 for image classification, a Temporal Convolution Network (TCN) for modeling sensor sequences, and a meta-learning approach to combine both outputs into a final prediction.

---

## Overview

The system is built around a Raspberry Pi and ESP32 setup. A USB camera connected to the Raspberry Pi captures images, while an ESP32 collects real-time data from multiple sensors. Both streams are processed independently and then fused to produce the final fire or safe prediction.

This approach makes the system more robust in real-world scenarios where lighting conditions, smoke visibility, or sensor noise alone may not be sufficient for reliable detection.

---

## IoT Integration

The IoT component plays a key role in this system by providing continuous environmental data.

An ESP32 microcontroller is used to collect sensor readings and send them to the main system. The following sensors are used:

* MQ2 Gas Sensor
* MQ135 Air Quality Sensor
* Temperature Sensor
* Flame Sensor
* Ultrasonic Sensor

The ESP32 reads these sensor values in real time and transmits them via serial communication to the Raspberry Pi. The data is collected as a sequence over time and passed to the TCN model.

Since the sensor model depends on temporal patterns, the system waits until a sufficient number of readings (sequence length) is collected before making predictions. This initial delay is expected behavior.

---

## Model Components

### EfficientNetV2 (Image Model)

EfficientNetV2-S is used for classifying images into fire or safe categories.

* AUC on test set is approximately 0.9999
* Very strong separation between fire and non-fire samples
* Some false positives occur due to threshold selection

This model is highly effective in ranking samples correctly, even though threshold tuning is required for optimal classification.

---

### TCN (Sensor Model)

The Temporal Convolution Network processes sequences of sensor readings.

* AUC on test set is around 0.88
* Captures temporal trends in environmental data
* Useful for detecting conditions not clearly visible in images

This model complements the vision model by providing additional context.

---

### Fusion Models

Two fusion approaches are used to combine predictions:

#### OR Fusion (Hard Fusion)

Takes the maximum of probabilities from both models.

* Improves fire detection recall
* Can increase false positives

#### Meta Fusion (Soft Fusion)

A meta-model is trained using outputs from both models.

* AUC close to 1.0
* Produces more stable and reliable predictions
* Reduces inconsistencies between models

This is the final model used for decision making.

---

## Key Observations from Results

* EfficientNetV2 provides near-perfect ranking performance
* TCN improves detection by capturing temporal sensor behavior
* Fusion significantly improves robustness
* High AUC does not imply perfect classification; threshold selection is important

---

## Dataset

The dataset is organized into multiple folders such as:

* merged1_test
* merged2_test
* ...
* merged20_test

Each folder contains image sequences used for evaluation under different conditions.

---

## Project Structure

```text
FIRE-DETECTION-CAPSTONE/
│
├── merged*_test/        # Dataset folders
├── esp32/              # ESP32 related files
├── esp_code.ino        # Sensor data acquisition code
├── infer.py            # Inference pipeline
├── requirements.txt    # Dependencies
├── README.md
```

---

## Model Files

The trained models include:

* effnetv2s_model.keras
* tcn_model.keras
* meta_model.pkl
* scaler_tcn.pkl
* meta.pkl

These are stored in the repository using Git LFS due to size constraints.

---

## Running the System

1. Clone the repository:

```text
git clone https://github.com/Neha-Reddy-16/FIRE-DETECTION-CAPSTONE.git
cd FIRE-DETECTION-CAPSTONE
```

2. Install dependencies:

```text
pip install -r requirements.txt
```

3. Run inference:

```text
python infer.py
```

---

## Important Notes

* The system requires a sequence of sensor readings before producing output
* Initial delay is expected until sufficient data is collected
* Ensure ESP32 is connected and detected (e.g., /dev/ttyUSB0)
* Ensure camera is properly connected

---

## Conclusion

This project demonstrates that combining computer vision with IoT-based sensor data leads to a more reliable fire detection system.

The image model provides strong visual detection, while the sensor model captures environmental changes over time. The meta-learning approach effectively combines both, making the system suitable for real-world deployment.

---

## Tech Stack

* Python
* TensorFlow / Keras
* OpenCV
* scikit-learn
* ESP32 (Arduino)
* Raspberry Pi

---
