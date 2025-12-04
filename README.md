# Indian Sign Language – Non-Manual Feature Interpreter (MVP)
A lightweight browser-based system that reads facial expressions, head movements, and shoulder posture to gener![Uploading Image1.png…]()
ate meaningful ISL cues in real time.

---

## 🌟 Overview
This project demonstrates a minimal but functional prototype that uses a webcam to capture **non-manual features (NMFs)** used in Indian Sign Language — such as eyebrow movement, mouth openness, nodding, head tilt, shoulder lean — and converts them into readable text outputs.

The current version runs on a **rule-based detection system**, while future versions will introduce a **machine learning classifier** for more accurate output.


# Project Demo

## 1. Input Gesture
![Input Gesture](images/Image1.jpg)

## 2. Hand Landmark Detection
![Landmark](images/landmark.png)

## 3. Output Prediction
![Output](images/output.jpg)
---

## 🚀 How to Run the Project

### 1️⃣ Start Local Server  
(Mediapipe requires local hosting)

```bash
cd isl-nmf-mvp/web
python3 -m http.server 8000
```

### 2️⃣ Open in Browser
Visit:

```
http://localhost:8000
```

Allow webcam permissions.

### 3️⃣ Try These Expressions

| Action | Interpretation |
|--------|----------------|
| Raise eyebrows | Question / Emphasis |
| Open mouth | Surprise / Exclamation |
| Tilt head | Curiosity / Doubt |
| Nod up/down | Yes / Agreement / Thinking |
| Shake head | No / Disagreement |
| Lean shoulders | Direction / Emphasis |

---

## 🔍 Features Detected

| Feature | Method | Meaning |
|---------|--------|---------|
| Eyebrow Raise | Brow–eye distance | questioning, emphasis |
| Mouth Open | Lip gap | surprise, exclamation |
| Head Tilt | Roll angle | curiosity, doubt |
| Head Nod | Vertical movement | yes, thinking |
| Head Shake | Horizontal movement | no, disagreement |
| Shoulder Lean | Shoulder tilt | direction, emphasis |

All features are extracted using **MediaPipe FaceMesh + Pose landmarks**.

---

## 🧩 System Architecture

### **Phase A (Current): Rule-Based Layer**
- MediaPipe FaceMesh (478 points)
- MediaPipe Pose (33 points)
- Threshold evaluation
- Simple mapping to text using if-else logic

### **Phase B (Upcoming): ML Classifier**
- Create labeled CSV dataset  
- Train a lightweight classifier (Logistic Regression / Tiny Neural Net)  
- Convert model to TensorFlow.js  
- Run inference inside the browser  

---

## 📁 Folder Structure

```
isl-nmf-mvp/
│
├── web/
│   ├── index.html
│   ├── app.js
│   └── styles.css
│
├── dataset/
│   └── nmf_training.csv
│
└── train/
    └── train_classifier.py
```

---

## 🔧 Landmarks & Thresholds

### Face Mesh Landmarks Used
- Eyebrows: 70, 300  
- Eyes: 159, 386  
- Mouth: 13, 14  
- Nose: 1, 168  
- Chin: 152  
- Ears: 234, 454  

### Pose Landmarks Used
- Shoulders: 11 (left), 12 (right)

### Default Threshold Values

```javascript
eyebrowRaise: avgBrowDist > 0.04;
mouthOpen: mouthHeight > 0.02;
headTilt: Math.abs(angle) > 0.15;
headNod: Math.abs(deltaY) > 0.03;
headShake: Math.abs(horizontalOffset) > 0.05;
shoulderLean: Math.abs(tiltAngle) > 0.2;
```

---

## 🤖 Phase B – ML Pipeline (Upcoming)

### 1. Dataset Creation  
Sample CSV:

```csv
eyebrow_raise,mouth_open,head_tilt,head_nod,head_shake,shoulder_lean,label
1,0,0,0,0,0,question
0,1,0,0,0,0,exclamation
0,0,1,0,0,0,curiosity
```

### 2. Enable Logging in `app.js`

```javascript
collectMode = true;
currentLabel = 'surprise';
console.log(JSON.stringify(currentFeatures));
```

### 3. Train Classifier

```
python train/train_classifier.py
```

### 4. Export to TensorFlow.js  
Load trained model in browser for real-time prediction.

---

## 🛠️ Customization

### Add New Feature

```javascript
currentFeatures.newFeature = computeFeature(landmarks) > limit ? 1 : 0;
```

### Change Mapping

```javascript
if (features.headTilt === 1) output.push("curiosity");
```

### Adjust Sensitivity  
Modify threshold values depending on lighting and distance from camera.

---

## 🐞 Troubleshooting

| Issue | Fix |
|--------|------|
| Webcam not working | Allow permissions / use localhost |
| Mediapipe not loading | Check CDN |
| Laggy detection | Reduce model complexity |
| Incorrect detection | Increase thresholds |

---

## 📌 Roadmap
- ✔ Rule-based MVP  
- ☐ ML classifier integration  
- ☐ Region-wise ISL variations  
- ☐ Mobile app (TFLite)  
- ☐ Dataset expansion  
- ☐ Grammar modeling  

---

## 📜 License
MIT License — free for research and educational use.

---

**Made to support accessible Indian Sign Language communication. 🤟**
