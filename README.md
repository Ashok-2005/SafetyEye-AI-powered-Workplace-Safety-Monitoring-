# **🧠 SafetyEye – AI-Powered Workplace Occupancy & Safety Monitor**

### **Final Technical Report (Updated: November 7, 2025)**

### **Author:** *Uppalapati Venkata Ashok Adithya*

### Infosys Springboard Internship – 2025

---

# **1. Executive Summary**

**SafetyEye** is an AI-driven workplace safety and compliance monitoring system designed to detect **Personal Protective Equipment (PPE)** compliance, identify violations in real time, and provide actionable safety intelligence.
Leveraging **YOLOv8** deep-learning models and **video analytics**, the system delivers:

* Real-time PPE detection (Helmet, Mask, Vest, Gloves, Boots, etc.)
* Per-person rule evaluation and 3-frame violation confirmation
* Automated alerting (console/email)
* Violation evidence recording (snapshots, video clips, metadata)
* Occupancy monitoring and worker-level tracking
* Multi-scenario testing and performance evaluation

The system is engineered for deployment in high-risk industrial environments such as construction sites and manufacturing floors.

---

# **2. Project Objectives**

### ✅ Primary Goals

* Detect PPE compliance and violations in real time
* Monitor workplace occupancy using camera feeds
* Trigger alerts upon confirmed safety violations
* Log snapshots and video evidence for safety auditing
* Support robust performance across challenging visual conditions

### ✅ Secondary Goals

* Implement temporal smoothing to reduce false positives
* Ensure scalable architecture for multi-camera environments
* Prepare foundations for a full **Streamlit Safety Dashboard**

---

# **3. Dataset & Preprocessing**

### **Dataset Source**

Roboflow Construction Site Safety Dataset
[https://www.kaggle.com/datasets/snehilsanyal/construction-site-safety-image-dataset-roboflow](#)

### **PPE Classes Included**

* Hardhat
* No-Hardhat
* Safety Vest
* No-Safety Vest
* Mask / No-Mask
* Gloves / No-Gloves
* Boots / No-Boots
* Person
* Machinery
* Vehicle
* Safety Cone

### **Data Split**

| Split      | Samples |
| ---------- | ------- |
| Train      | 2603    |
| Validation | 114     |
| Test       | 82      |

### **Preparation Tasks**

✅ Annotation validation
✅ Missing/misaligned labels correction
✅ Structure verified for YOLOv8
✅ Data augmentation (flip, blur, exposure, scale)

---

# **4. Model Training (YOLOv8)**

### **Training Parameters**

| Parameter     | Value |
| ------------- | ----- |
| Epochs        | 50    |
| Batch Size    | 16    |
| Image Size    | 640   |
| Learning Rate | 0.005 |
| Optimizer     | AdamW |

### **Performance Summary**

| Metric    | Score     |
| --------- | --------- |
| Precision | **0.855** |
| Recall    | **0.714** |
| mAP50     | **0.786** |
| mAP50-95  | **0.466** |

### **Observations**

* Strong detection accuracy for Helmet, Vest, Mask
* Moderate difficulty in low-light or motion-blur conditions
* Good generalization across unseen videos

---

# **5. Real-Time Detection Engine**

### **Features Implemented**

✅ Multi-video stream support
✅ Per-frame PPE classification and visualization
✅ FPS overlay, inference timing, and worker tracking
✅ Auto-resize and frame-skip optimization (performance boost)
✅ GPU acceleration with FP16 support

### **Performance (RTX 3060)**

| Metric         | Value         |
| -------------- | ------------- |
| Average FPS    | 22–30 FPS     |
| Inference Time | 35–45 ms      |
| Resolution     | 720p (scaled) |

---

# **6. Rule Engine (Per-Person PPE Evaluation)**

A dedicated rule engine evaluates PPE compliance per worker using bounding-box association.

### **PPE Rules Evaluated (R1–R6)**

| Rule | Condition                         |
| ---- | --------------------------------- |
| R1   | No Helmet detected                |
| R2   | No Safety Vest detected           |
| R3   | No Mask detected                  |
| R4   | Gloves missing                    |
| R5   | Boots missing                     |
| R6   | PPE compliant (all items present) |

### **Outputs**

* Green label = PPE present
* Red label = Violation (missing PPE)
* Works independently for each worker

---

# **7. Violation Engine with Temporal Smoothing (3-Frame Confirmation)**

✅ Implemented to reduce false positives
✅ Violation is only confirmed when detected for **three consecutive frames**
✅ Ensures stability under blur, movement, and occlusions

### **Results**

* ~70% reduction in false positives
* Consistent behavior across multi-worker scenarios
* Stable detection even in challenging lighting

---

# **8. Automated Evidence Recording System**

When a violation is confirmed:

✅ Capture **snapshot image**
✅ Record **10-second evidence clip**
(5 seconds before + 5 seconds after violation)
✅ Auto-organize directories:

```
/violations/
   /20251107/
       /worker_5/
           /NoHelmet/
               20251107_143212_NoHelmet.jpg
               20251107_143212_NoHelmet.mp4
```

✅ Save metadata (CSV + JSON) including:

* Timestamp
* Worker ID
* Violation type
* Confidence
* Video and image paths

---

# **9. Alert System (Console + Email Ready)**

### **Alert Types Supported**

* ✅ Console alert
* ✅ Email notification (SMTP)
* ✅ GUI pop-up (optional for desktop)

### **Features**

* No duplicate alerts for the same person + violation
* Alert delay < 2 seconds
* All alerts logged into a CSV

---

# **10. System Optimization**

### **Implemented Improvements**

✅ Frame resizing (1080p → 720p)
✅ Frame Skip (process every 2nd frame)
✅ FP16 inference
✅ GPU acceleration
✅ Efficient video buffering (Ring Buffer)
✅ Multi-thread ready architecture

### **Outcome**

* FPS improved from **15 → 30 FPS**
* Lower inference latency
* Stable performance on long videos

---

# **11. Multi-Scenario Testing**

### **Test Cases & Results**

| Scenario                 | Expected                             | Result |
| ------------------------ | ------------------------------------ | ------ |
| TC1: Full PPE            | No violation                         | ✅      |
| TC2: No Helmet           | Alert + Evidence                     | ✅      |
| TC3: No Vest             | Alert + Evidence                     | ✅      |
| TC4: Night/Low Light     | Minor misses, stable after smoothing | ✅      |
| TC5: Motion Blur         | Slight FPS drop, rules consistent    | ✅      |
| TC6: Multiple Workers    | Independent rule evaluation          | ✅      |
| TC7: Duplicate Violation | Suppressed correctly                 | ✅      |

---

# **12. Final System Status**

| Module                             | Status        |
| ---------------------------------- | ------------- |
| Dataset Preparation                | ✅ Completed   |
| Model Training                     | ✅ Completed   |
| Rule Engine                        | ✅ Completed   |
| Violation Engine (3-Frame Confirm) | ✅ Completed   |
| Evidence Recording                 | ✅ Completed   |
| Alert System                       | ✅ Completed   |
| Scenario Testing                   | ✅ Completed   |
| Optimization                       | ✅ Completed   |
| Streamlit Dashboard                | ⏳ In Progress |

---

# **13. Conclusion**

The **SafetyEye System** has evolved into a robust, real-time, AI-based PPE detection and safety monitoring platform with industry-level capabilities:

✅ Accurate PPE detection
✅ Reliable rule evaluation
✅ Stable tracking across multi-person scenes
✅ Automated alerts and evidence logging
✅ High-performance optimized pipeline

It is ready for integration into enterprise dashboards and industrial IoT environments.

---

# **14. Future Enhancements**

* Streamlit Dashboard for live monitoring
* Edge deployment (NVIDIA Jetson, Raspberry Pi 5)
* Multi-camera centralized analytics
* Worker identity re-identification (Re-ID)
* Cloud sync & API integration (AWS/Azure/GCP)

---
