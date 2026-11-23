# ✊✋✌️ Rock-Paper-Scissors AI: Build Your Own Gesture Recognizer

A beginner-friendly machine learning project that runs entirely in **Google Colab**. Students record their own hand gestures with a webcam, train a neural network in minutes, and then watch it recognize **Rock, Paper, or Scissors** live, with a green circle tracking the hand and a confidence score on screen.

Built for classroom and club sessions to teach the core AI loop: **Collect → Train → Test → Improve**.

---

## Table of Contents
- [Features](#features)
- [How It Works](#how-it-works)
- [Tech Stack](#tech-stack)
- [Requirements](#requirements)
- [Quick Start](#quick-start)
- [Notebook Structure](#notebook-structure)
- [Usage Tips for Higher Accuracy](#usage-tips-for-higher-accuracy)
- [Troubleshooting](#troubleshooting)
- [Classroom Guide](#classroom-guide)
- [Key Concepts](#key-concepts)
- [Limitations](#limitations)
- [Future Improvements](#future-improvements)
- [Acknowledgements](#acknowledgements)

---

## Features
- **Webcam data collection** with a 3-2-1 countdown, a recording progress bar, and a live counter.
- **Smart capture:** photos are saved only when a hand is visible, so no empty or useless images enter the dataset.
- **Transfer learning** with MobileNetV2 and a two-round **fine-tuning** strategy for higher accuracy.
- **Data augmentation** (rotation, zoom, shift, brightness, contrast) so the model copes with new lighting and positions.
- **Report card:** accuracy graph and confusion matrix to see exactly where the model makes mistakes.
- **Live recognition** with a green circle on the hand, a gesture label, confidence bars, and smoothing over recent frames.
- **Automatic "NOTHING" state:** when no hand is on screen, the app shows NOTHING without training a separate class.
- **Honest uncertainty:** the label shows NOT SURE when the top two guesses are too close.

## How It Works

```
Webcam ──► Hand check (MediaPipe) ──► Photo saved only if a hand is visible
                                              │
                                              ▼
                       Dataset: Rock / Paper / Scissors photos (224×224)
                                              │
                                              ▼
              MobileNetV2 (pretrained) + augmentation + new classifier layer
                         Round 1: train top layer
                         Round 2: fine-tune last 30 layers
                                              │
                                              ▼
Live camera ──► MediaPipe finds hand ──► Photo sent to trained model ──► Label + confidence
                     │
                     └── No hand? Show "NOTHING"
```

Two tools share the work in live mode:
- **MediaPipe Hands** (Google) detects *where* the hand is and draws the green circle.
- **Our trained model** decides *which gesture* it is.

## Tech Stack
| Area | Tool |
|---|---|
| Environment | Google Colab (Chrome) |
| Deep learning | TensorFlow / Keras, MobileNetV2 |
| Hand detection | MediaPipe Hands (loaded in the browser) |
| Data handling | NumPy, Pillow |
| Visualization | Matplotlib, scikit-learn (confusion matrix) |
| Browser bridge | `google.colab.output` callbacks and JavaScript |

## Requirements
- A computer with a **webcam**
- **Google Chrome** (Safari and Firefox are not supported for webcam access in Colab)
- A free **Google account** for Colab
- A stable internet connection (the hand tracker loads from a CDN)
- Recommended: **T4 GPU** runtime (*Runtime → Change runtime type → T4 GPU*)

## Quick Start
1. Open [colab.research.google.com](https://colab.research.google.com) in **Chrome** and create a **New notebook**.
2. Select **Runtime → Change runtime type → T4 GPU**.
3. Paste each code block from the project into its own cell, in order.
4. Run the cells one by one from top to bottom.
5. When Chrome asks for camera access, click **Allow**.
6. Wait for the **✅ Captured** message after each recording before moving on.
7. After training, run the live camera cell and show your gestures.

> After changing the labels or switching versions, use **Runtime → Restart session** and re-run everything in order so old models don't stay in memory.

## Notebook Structure
| Step | Purpose |
|---|---|
| **1. Tools** | Imports libraries and sets `labels = ["rock", "paper", "scissors"]` |
| **2. Photo machine** | Webcam capture with countdown, progress bar, and hand check |
| **3–5. Record** | Capture ROCK, PAPER, and SCISSORS (up to 200 photos each) |
| **7. Check data** | Prints counts and previews sample photos |
| **8. Split** | 80% study photos, 20% hidden exam photos |
| **9. Build model** | MobileNetV2 + augmentation + softmax classifier |
| **10. Train** | Round 1 (top layer) then Round 2 (fine-tuning) with early stopping |
| **11. Report card** | Accuracy curves, confusion matrix, average confidence |
| **12. Live camera** | Connects the model to the webcam with circle, label, and bars |
| **13. Improve** | Add more photos for weak gestures, then retrain |

## Usage Tips for Higher Accuracy
1. **Move while recording.** Change distance, tilt, and position. Variety matters more than quantity.
2. **Keep gestures clearly different.** Rock is a tight fist, paper is five fingers spread wide, scissors is exactly two fingers with the others folded.
3. **Keep the whole hand in the frame**, not cut off by the edge.
4. **Record where you will test.** Use the same room, lighting, and background.
5. **Use good, even light** and avoid a bright window behind you.
6. **Add more people.** Record a friend's hand so the model works for everyone.
7. **Retrain after adding photos**, then restart the live camera cell.

## Troubleshooting
| Problem | Fix |
|---|---|
| Camera does not start | Use Chrome and click **Allow** in the address bar camera icon |
| Camera busy | Close Zoom, Meet, or other tabs using the camera |
| Hand tracker fails to load | The network may block it. Try another Wi-Fi or a phone hotspot |
| `model is not defined` | Run the build and training cells first |
| Bars show 4 gestures | An old model is in memory. Restart the session and re-run in order |
| Often shows NOT SURE | Record more varied photos for that gesture and retrain |
| Training is slow | Switch to a T4 GPU or record fewer photos (for example 100 each) |
| Yellow text error on camera | Re-run the live cell. If it persists, note the exact text |

## Classroom Guide
**Suggested timing (45–60 minutes):**

| Time | Activity |
|---|---|
| 0–5 min | Hook: "How would you teach a computer to tell rock from paper?" |
| 5–10 min | Run setup cells and explain the AI loop |
| 10–20 min | Record the three gestures |
| 20–30 min | Train and read the report card |
| 30–40 min | Live test: try tricky cases (far away, other hand, thumbs up) |
| 40–50 min | Improve with a friend's hand and retrain |
| 50–60 min | Discussion |

**Discussion questions**
- Why did the AI make mistakes, and what data would fix them?
- Why does adding a friend's hand help?
- Why does the AI sometimes sound confident but wrong?
- What would happen if we trained only on one person in one room?

## Key Concepts
| Term | Meaning |
|---|---|
| **Data** | The examples shown to the AI (our photos) |
| **Label** | The correct answer for each example |
| **Model** | The AI that learns patterns from data |
| **Epoch** | One full pass through all study photos |
| **Augmentation** | Slightly altered copies of photos so the model copes with change |
| **Transfer learning** | Reusing a model that already learned general vision skills |
| **Fine-tuning** | Gently adjusting that pretrained model to our task |
| **Accuracy** | Percent of answers it got right |
| **Confidence** | How sure the model is about its answer |
| **Confusion matrix** | A table showing which gestures get mixed up |
| **Bias** | Wrong or unfair results caused by limited training examples |

## Limitations
- **No honest 100% guarantee.** Lighting, blur, partly formed gestures, and new backgrounds can still cause errors. Well-recorded gestures typically reach roughly 93–98% accuracy.
- The model learns from **pixels**, so it is sensitive to background and lighting different from the training photos.
- Only **one hand** is tracked at a time.
- Webcam access works in **Chrome** only, and depends on the network allowing the MediaPipe CDN.
- Training on CPU is slow; a GPU runtime is recommended.

## Future Improvements
- **Hand-landmark version:** learn from the 21 MediaPipe hand points instead of photos. It trains in about a second and is less affected by background and lighting.
- Add more gestures (thumbs up, peace, OK) and a game mode that plays against the computer.
- Export the model to TensorFlow.js to run entirely in the browser.
- Two-player mode with score tracking.

## Acknowledgements
- [TensorFlow / Keras](https://www.tensorflow.org/) and the MobileNetV2 architecture
- [MediaPipe Hands](https://developers.google.com/mediapipe) by Google
- [Google Colab](https://colab.research.google.com) for free, accessible compute
- Inspired by Google's Teachable Machine and the "build your own AI" teaching approach

## License
Released for educational use. Add your preferred license (for example MIT) here before publishing.
