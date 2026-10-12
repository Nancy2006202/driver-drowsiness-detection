# Driver Drowsiness & Distraction Detection

A real-time computer vision system that watches the driver through a webcam and sounds an alarm when it detects signs of drowsiness or distraction.

Built with **Python, OpenCV and MediaPipe**.

## Why this project?
Drowsy driving, especially on highways, is a major cause of road accidents. This project detects early warning signs and alerts the driver before an accident can happen.

## Features
- **Eyes closed** for too long: drowsiness alert
- **Yawning** detection: take-a-break alert
- **Head drop**: head position changed from normal
- **Looking away**: head turned away from the road
- **Face not visible**: alarm if the face is missing for 2 seconds
- **Per-user calibration**: the first 5 seconds set a baseline for each person
- **Audio alarm** with a red warning border on screen
- **Alert logging**: every alert is saved to `alerts_log.csv` with a timestamp

## How it works
1. **MediaPipe Face Mesh** finds facial landmarks in every video frame.
2. **Eye Aspect Ratio (EAR)** measures how open the eyes are. If it drops below 70% of the user's calibrated value for 1.5 seconds, the eyes are treated as closed.
3. **Mouth Aspect Ratio (MAR)** detects a wide-open mouth held for over a second (a yawn).
4. **Head position ratios** (nose position relative to the eyes and chin) detect head dropping or turning.
5. **Timers** make sure normal blinks and quick movements do not trigger false alarms.

## Installation
Requires Python 3.9 to 3.12 and a webcam.

```
pip install -r requirements.txt
```

## Usage
1. Install the requirements (above).
2. Open `project.ipynb` in VS Code or Jupyter and run the code cell.
3. A camera window opens (check the taskbar if it is hidden behind other windows).

- Look straight at the camera with your eyes open for the first 5 seconds (calibration).
- Press **Q** in the camera window to quit.

## Settings you can tune
| Setting | Meaning |
|---|---|
| `EYE_CLOSED_SECS` | How long closed eyes trigger an alert |
| `EAR_FACTOR` | How much the eyes must close, relative to your baseline |
| `MAR_YAWN` | How wide the mouth must open to count as a yawn |
| `HEAD_DELTA` | Sensitivity of head-drop detection |
| `NO_FACE_SECS` | How long a missing face triggers an alert |

## What you see on screen
| Situation | Message shown | Alarm |
|---|---|---|
| Eyes closed for 1.5 s | `DROWSY: EYES CLOSED` | Yes |
| Mouth wide open for 1.2 s | `POSSIBLE YAWN: TAKE A BREAK` | Yes |
| Head position changed for 2 s | `CHECK HEAD POSITION` | Yes |
| Head turned away for 1.5 s | `DISTRACTED: LOOK FORWARD` | Yes |
| Face missing for 2 s | `FACE NOT VISIBLE: EYES ON ROAD!` | Yes |

Each alert also turns the screen border red and is written to `alerts_log.csv`.

## Limitations
- Accuracy drops in low light, with sunglasses or heavy glasses glare, and with extreme camera angles.
- Looking down at a phone or dashboard can lower the eye ratio and cause false alerts.
- Head turn and head drop use simple heuristics, not full head-pose estimation.
- The built-in beep uses `winsound`, so audio works on Windows only.
- Tested informally on the main scenarios (eyes closed, yawn, looking away, face hidden), not on a large dataset.

## Future improvements
- Train a small CNN for open/closed eye classification and compare it with EAR
- Test on public datasets (NTHU-DDD, YawDD) and report accuracy
- Run on a Raspberry Pi or phone inside a vehicle
- Add a dashboard to view logged alerts

## Author
Nancy Sharma, BCA, Biyani Girls College, Jaipur
