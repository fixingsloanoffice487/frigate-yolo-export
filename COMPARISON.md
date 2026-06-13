# Frigate Detection Models: YOLO26 vs YOLO11 vs YOLOv8

## Overview

Frigate's object detection ecosystem has evolved significantly with support for multiple YOLO-based architectures and hardware accelerators. While Frigate v0.17 officially centers around YOLOv9, many users evaluate newer Ultralytics models such as YOLO11 and YOLO26 for custom ONNX, TensorRT, and OpenVINO deployments.

This document compares YOLOv8, YOLO11, and YOLO26 from the perspective of Frigate deployments, including architecture improvements, performance benchmarks, inference characteristics, and recommended hardware configurations.

---

# Model Families

| Model            | Variants      | Release   | Primary Focus                            |
| ---------------- | ------------- | --------- | ---------------------------------------- |
| YOLOv8           | n, s, m, l, x | 2023      | Anchor-free redesign and efficiency      |
| YOLO11           | n, s, m, l, x | 2024      | Improved accuracy and feature extraction |
| YOLO26           | n, s, m, l, x | 2025      | NMS-free inference and edge optimization |
| YOLOv9 (Frigate) | t, s, m, c, e | 2024–2026 | Frigate flagship detector family         |

### Variant Naming

| Suffix | Meaning     | Typical Use                 |
| ------ | ----------- | --------------------------- |
| n      | Nano        | Edge devices, low power     |
| s      | Small       | General-purpose deployments |
| m      | Medium      | Balanced performance        |
| l      | Large       | Higher accuracy             |
| x      | Extra Large | Maximum accuracy            |

---

# Accuracy Comparison

## COCO mAP50-95

| Model   | Accuracy                                   |
| ------- | ------------------------------------------ |
| YOLOv8s | ~44–45%                                    |
| YOLO11s | 47.0%                                      |
| YOLO11m | 51.5%                                      |
| YOLO11x | 54.7%                                      |
| YOLOv9s | ~55.6%                                     |
| YOLO26s | Slightly above YOLO11s (dataset dependent) |

### Key Takeaways

* YOLO11 improves accuracy over YOLOv8 while using fewer parameters.
* YOLO26 focuses on deployment efficiency and improved inference pipelines.
* YOLOv9 remains Frigate's most mature and widely supported detector family.

---

# Inference Performance

## YOLO11 Official Benchmarks (640×640)

| Model   | CPU ONNX | Nvidia TensorRT |
| ------- | -------- | --------------- |
| YOLO11n | 56 ms    | 1.5 ms          |
| YOLO11s | 90 ms    | 2.5 ms          |
| YOLO11m | 183 ms   | 4.7 ms          |
| YOLO11l | 239 ms   | 6.2 ms          |
| YOLO11x | 463 ms   | 11.3 ms         |

### Relative Performance

| Model       | Speed                                |
| ----------- | ------------------------------------ |
| YOLOv8n     | Fast                                 |
| YOLO11n     | Very Fast                            |
| YOLO11s     | Excellent Balance                    |
| YOLO26s     | Optimized Throughput                 |
| YOLO26m/l/x | Higher Accuracy, Higher Compute Cost |

---

# Architectural Improvements

## YOLOv8

### Major Features

* Anchor-free detection head
* Decoupled classification and regression heads
* Simplified architecture
* Excellent baseline efficiency

### Advantages

* Mature ecosystem
* Broad hardware compatibility
* Easy export to ONNX and TensorRT

### Limitations

* Lower accuracy than newer generations
* Less efficient feature extraction

---

## YOLO11

### Major Improvements

* Enhanced backbone architecture
* Improved feature aggregation
* Better small-object detection
* Reduced parameter count
* Improved memory efficiency

### Advantages

* Higher accuracy than YOLOv8
* Better speed-to-accuracy ratio
* Strong GPU acceleration support

### Best Use Cases

* Home surveillance
* Multi-camera Frigate deployments
* GPU-accelerated inference

---

## YOLO26

### Major Improvements

* Native NMS-free inference
* Progressive Loss Balancing (ProgLoss)
* Small-Target-Aware Label Assignment (STAL)
* Optimized deployment pipeline
* Reduced post-processing overhead

### Advantages

* Faster end-to-end inference
* Better edge-device performance
* Improved deployment efficiency

### Best Use Cases

* Edge AI devices
* TensorRT deployments
* Future Frigate integrations

---

# CPU vs GPU Performance

## CPU-Only Detection

### Recommended Models

* YOLOv8n
* YOLO11n

### Suitable For

* Testing
* Development environments
* 1–2 camera installations

### Expected Performance

| Hardware           | Inference   |
| ------------------ | ----------- |
| Modern Desktop CPU | 50–150 ms   |
| Intel N100         | 80–200 ms   |
| Low-Power ARM      | 150–500+ ms |

### Limitations

* High CPU utilization
* Poor scalability
* Competes with video decoding
* Not recommended for production Frigate systems

---

# Intel OpenVINO

## Recommended Models

* YOLO11n
* YOLO11s
* YOLOv9-t
* YOLOv9-s

## Recommended Hardware

* Intel UHD 770
* Intel Iris Xe
* Intel Arc A310
* Intel Arc A380
* Intel NPU (Core Ultra)

## Typical Performance

| Hardware | Inference |
| -------- | --------- |
| Arc A310 | 4–8 ms    |
| Arc A380 | 4–7 ms    |
| Iris Xe  | 6–12 ms   |
| UHD 770  | 8–15 ms   |

### Advantages

* Excellent price/performance
* Hardware video decode included
* Low power consumption
* Ideal Frigate platform

---

# Nvidia GPU

## Recommended Models

* YOLO11s
* YOLO11m
* YOLO26s
* YOLO26m
* YOLOv9-s

## Recommended Hardware

* RTX 3050
* RTX 3060 12GB
* RTX 4060
* RTX 4070

## Typical Performance

| Hardware | Inference |
| -------- | --------- |
| RTX 3050 | 8–12 ms   |
| RTX 3060 | 6–10 ms   |
| RTX 4060 | 5–8 ms    |
| RTX 4070 | 3–6 ms    |

### Advantages

* Highest throughput
* CUDA Graphs support
* Best TensorRT ecosystem
* Scales to large camera counts

### Best For

* 8–30+ cameras
* Face recognition
* Semantic search
* Future AI workloads

---

# Apple Silicon

## Recommended Models

* YOLO11n
* YOLO11s
* YOLOv9-s

## Typical Performance

| Device | Inference |
| ------ | --------- |
| M1     | 8–10 ms   |
| M3 Pro | 5–7 ms    |
| M4     | 5–10 ms   |

### Advantages

* Exceptional performance-per-watt
* Integrated NPU
* No additional accelerator required

---

# Edge AI Accelerators

## Hailo-8 / Hailo-8L

| Device   | Inference |
| -------- | --------- |
| Hailo-8L | ~11 ms    |
| Hailo-8  | ~7 ms     |

### Best For

* Raspberry Pi deployments
* Fanless systems
* Ultra-low-power installations

---

## MemryX MX3

### Typical Performance

* 9–16 ms latency
* 300+ FPS pipeline throughput

### Best For

* Industrial edge deployments
* Embedded AI appliances

---

# Recommended Hardware by Deployment Size

## Small Installations (1–4 Cameras)

### Best Choice

* Intel N100 + OpenVINO
* Intel UHD Graphics
* Apple M1/M2

### Recommended Models

* YOLO11n
* YOLOv9-t

---

## Medium Installations (4–12 Cameras)

### Best Choice

* Intel Arc A310/A380
* Intel Iris Xe

### Recommended Models

* YOLO11s
* YOLOv9-s

---

## Large Installations (12–30+ Cameras)

### Best Choice

* RTX 3060 12GB
* RTX 4060
* RTX 4070

### Recommended Models

* YOLO11m
* YOLO26s
* YOLOv9-s

---

# Summary

YOLOv8 established the modern anchor-free Ultralytics architecture and remains a lightweight, reliable detector for resource-constrained systems. YOLO11 improves upon YOLOv8 through enhanced feature extraction, better small-object detection, increased accuracy, and improved efficiency, making it one of the best general-purpose object detection models available today. YOLO26 extends this evolution further with NMS-free inference, deployment-focused optimizations, and stronger edge-device performance, reducing end-to-end latency while maintaining competitive accuracy. For Frigate deployments, GPU or NPU acceleration remains strongly recommended over CPU-only inference. Intel OpenVINO currently offers the best value for most home installations, while Nvidia RTX GPUs provide the highest scalability for large multi-camera systems. Edge deployments benefit most from Hailo, MemryX, and Apple Silicon NPUs.
