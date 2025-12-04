# Indian Sign Language – Non-Manual Feature Interpreter (MVP)
A lightweight browser-based system that reads facial expressions, head movements and shoulder posture to generate meaningful ISL cues.

---

## 🌟 Overview
This project demonstrates a minimal but functional prototype that uses a webcam to capture non-manual features (NMFs) used in Indian Sign Language — such as eyebrow movements, mouth openness, nodding, shaking, and head/shoulder tilt — and converts them into readable text outputs.

The current version uses **rule-based detection**, while future versions will integrate **machine learning models**.

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

### 3️⃣ Try the Expressions

| Action | Interpretation |
|-------|----------------|
| Raise eyebrows | Question / Emphasis |
| Open mouth | Surprise / Exclamation |
| Tilt head | Curiosity / Doubt |
| Nod up/down | Agreement / Thinking |
| Shake head | No / Disagreement |
| Lean shoulders | Direction / Emphasis |

---

## 🔍 Features Detected

| Feature | Method Used | Meaning |
|---------|--------------|---------|
| Eyebrow Raise | Brow–eye distance | questioning, emphasis |
| Mouth Open | Lip gap | surprise, exclamation |
| Head Tilt | Roll angle | doubt, curiosity |
| Head Nod | Vertical movement | yes, thinking |
| Head Shake | Horizontal movement | no, disagreement |
| Shoulder Lean | Shoulder tilt | emphasis, direction |

All features are extracted using **MediaPipe FaceMesh + Pose landmarks**.

---

## 🧩 System Architecture

### Phase A (Current): Rule-Based Layer
- MediaPipe FaceMesh (478 points)
- MediaPipe Pose (33 points)
- Threshold evaluation
- Simple if-else mapping to text

### Phase B (Upcoming): ML Classifier
- Build labeled CSV dataset
- Train a small classifier (LogReg / tiny neural net)
- Export and run in browser with TensorFlow.js

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

### Face Mesh Landmarks
- Eyebrows: 70, 300  
- Eyes: 159, 386  
- Mouth: 13, 14  
- Nose: 1, 168  
- Chin: 152  
- Ears: 234, 454  

### Pose Landmarks
- Shoulders: 11, 12  

### Threshold Values

```javascript
eyebrowRaise: avgBrowDist > 0.04;
mouthOpen: mouthHeight > 0.02;
headTilt: Math.abs(angle) > 0.15;
headNod: Math.abs(deltaY) > 0.03;
headShake: Math.abs(horizontalOffset) > 0.05;
shoulderLean: Math.abs(tiltAngle) > 0.2;
```

---

## 🤖 Phase B: ML Pipeline

### 1. Dataset Creation
Sample CSV format:

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
Run training script:

```
python train/train_classifier.py
```

### 4. Export to TF.js
Load trained model into browser.

---

## 🛠️ Customization

### Add New Feature
```javascript
currentFeatures.newFeature = computeFeature(landmarks) > limit ? 1 : 0;
```

### Modify Rules
```javascript
if (features.headTilt === 1) output.push("curiosity");
```

### Adjust Sensitivity
Tune thresholds depending on lighting or camera distance.

---

## 🐞 Troubleshooting

| Issue | Fix |
|-------|------|
| Webcam blocked | Allow permissions, use localhost |
| Laggy detection | Reduce model complexity |
| Wrong detection | Increase thresholds |
| Mediapipe not loading | Check CDN |

---

## 📌 Roadmap
- ✔ MVP – Rule based  
- ☐ ML classifier  
- ☐ Regional ISL variants  
- ☐ Mobile (TFLite)  
- ☐ Dataset expansion  
- ☐ Grammar integration  

---

## 📜 License
MIT License — free for research and educational use.

---

**Made for accessible ISL communication. 🤟**