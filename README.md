# Automatic Number Plate Recognition (ANPR) System

A real-time Automatic Number Plate Recognition (ANPR) system built using **YOLOv8**, **EasyOCR**, **OpenCV**, and **Tkinter**. The system detects vehicles, identifies license plates, extracts the plate text using Optical Character Recognition (OCR), tracks vehicles across frames, and generates annotated video and CSV outputs.

---

## Features

- Real-time vehicle detection using YOLOv8
- License plate detection using a custom YOLOv8 model
- OCR-based license plate text recognition with EasyOCR
- IoU-based vehicle tracking across frames
- Interactive GUI with live video feed and detection statistics
- Automatic generation of annotated output video
- Automatic CSV logging of detected license plates
- Periodic screenshot capture during processing

---

## Project Workflow

```
Input Video / Webcam
          │
          ▼
Vehicle Detection (YOLOv8)
          │
          ▼
Vehicle Tracking
          │
          ▼
License Plate Detection
          │
          ▼
Crop Plate Region
          │
          ▼
Image Preprocessing
          │
          ▼
EasyOCR
          │
          ▼
Recognized Plate Text
          │
          ▼
Display Results
          │
          ├── Output Video
          ├── CSV Log
          └── Screenshots
```

---

# Technologies Used

| Technology | Purpose |
|------------|---------|
| Python | Core programming language |
| YOLOv8 | Vehicle and license plate detection |
| EasyOCR | Optical Character Recognition (OCR) |
| OpenCV | Video processing, drawing annotations and image preprocessing |
| Tkinter | Graphical User Interface |
| NumPy | Image and numerical operations |
| Pandas | Data handling |
| Pillow | Display OpenCV frames in Tkinter |
| TQDM | Progress bar |
| Colorama | Colored terminal output |

---

# Project Structure

```
ANPR-System/
│
├── plate-recognition.py
├── requirements.txt
├── yolov8n.pt
├── license_plate_detector.pt
│
└── output_anpr/
    ├── output.mp4
    ├── plates.csv
    └── screenshots/
```

---
# How It Works

### 1. Vehicle Detection

YOLOv8 detects supported vehicle classes:

- Car
- Bus
- Truck
- Motorcycle

Each detected vehicle receives a bounding box and confidence score.

---

### 2. Vehicle Tracking

Detected vehicles are assigned a unique Vehicle ID using an IoU-based tracker.

Tracking prevents the same vehicle from being treated as a new object every frame.

---

### 3. License Plate Detection

A second YOLOv8 model identifies license plates within detected vehicles.

The detected plate region is cropped from the original frame.

---

### 4. Image Preprocessing

Before OCR, the cropped license plate undergoes:

- Resize
- Grayscale conversion
- Bilateral filtering
- Adaptive thresholding

These steps improve text visibility and OCR accuracy.

---

### 5. OCR

EasyOCR extracts alphanumeric characters from the processed license plate image.

Only high-confidence OCR results are accepted.

---

### 6. Visualization

For every processed frame, the system displays:

- Vehicle bounding boxes
- License plate bounding boxes
- Detected plate numbers
- Vehicle IDs
- FPS
- Frame count
- Total detected vehicles
- Number of detected plates

---

### 7. Output Generation

After the entire video has been processed, the system automatically generates:

- Annotated output video
- CSV containing detected license plates
- Screenshots captured during processing

---

# Output

## 1. Annotated Video

```
output_anpr/output.mp4
```

Contains:

- Vehicle detections
- License plate detections
- Recognized plate text
- Dashboard statistics

---

## 2. CSV File

```
output_anpr/plates.csv
```

The CSV is generated **after the complete video has been processed**.

It contains:

| Column | Description |
|---------|-------------|
| plate_text | Detected license plate number |
| confidence | OCR confidence score |
| vehicle_id | Unique tracked vehicle ID |
| timestamp | Detection timestamp |
| frame_number | Frame in which the plate was detected |

Example:

| plate_text | confidence | vehicle_id | timestamp | frame_number |
|------------|------------|------------|------------|--------------|
| EY61NBG | 0.94 | 17 | 2026-07-01T18:20:14 | 612 |

---

## 3. Screenshots

```
output_anpr/screenshots/
```

Screenshots are captured automatically at fixed frame intervals during processing.

---

# Supported Vehicle Classes

- Car
- Bus
- Truck
- Motorcycle

---

# Models Used

### Vehicle Detection

```
yolov8n.pt
```

Pretrained YOLOv8 model used for detecting vehicles.

---

### License Plate Detection

Pretrained YOLOv8 license plate detection model sourced from the following repository:

[license_plate_detector.pt](https://github.com/Muhammad-Zeerak-Khan/Automatic-License-Plate-Recognition-using-YOLOv8)

---

### OCR

EasyOCR is used to recognize characters from the detected license plate.

---

# Running the Project

## 1. Clone the Repository

```bash
git clone https://github.com/Shriya-Guptaa/Licence-Plate-Recognition-System.git
cd Licence-Plate-Recognition-System
```

---

## 2. Install Dependencies

```bash
pip install -r requirements.txt
```

---

## 3. Ensure the Required Model Files Are Present

Place the following model files in the project root directory:

```
yolov8n.pt
license_plate_detector.pt
```

> **Note:** If `yolov8n.pt` is not found, Ultralytics will automatically download it on the first run.

---

## 4. Run the Application

Simply execute:

```bash
python plate-recognition.py
```

The application will:

- Open the GUI
- Process the input source
- Display detections in real time
- Generate the output video
- Save detected license plates to a CSV file after processing completes

---

## Input Source

By default, the project processes the sample video specified in the following line:

```python
ap.add_argument(
    "source",
    nargs="?",
    default="sample_plate.mp4",
    help="Video file or camera index (default: sample_plate.mp4)"
)
```

### Using the Sample Video (Default)

No changes are required. You can download the video from [here](https://drive.google.com/file/d/139Jzgh2o438vWkCkXoBn1l7RdTqTPpYt/view?usp=sharing)

```bash
python plate-recognition.py
```

---

### Using Your Webcam

Replace

```python
default="sample_plate.mp4"
```

with

```python
default="0"
```

or

```python
default=0
```

Then run:

```bash
python plate-recognition.py
```

The system will use your default webcam as the input source.

---

### Using Another Video

Replace

```python
default="sample_plate.mp4"
```

with

```python
default="your_video.mp4"
```

or specify the video directly when running the script:

```bash
python plate-recognition.py your_video.mp4
```

Supported formats include:

- `.mp4`
- `.avi`
- `.mov`
- Most video formats supported by OpenCV

---

## Generated Outputs

After the video has been completely processed, the following files are created inside the `output_anpr/` directory:

```
output_anpr/
├── output.mp4          # Annotated output video
├── plates.csv          # Detected license plate records
└── screenshots/        # Periodically captured frames
```

> **Note:** `plates.csv` is generated **after the entire video has finished processing**. During execution, detected plate information is stored in memory and written to the CSV only once processing is complete.

# Output Notes

- Vehicle detections are displayed in real time.
- OCR results appear during processing.
- Screenshots are captured periodically.
- The annotated output video (`output.mp4`) is finalized after processing completes.
- The CSV file (`plates.csv`) is populated only after the entire video has been processed and all detections have been logged.

---

# Future Improvements

- Export CSV incrementally during processing.
- Replace the IoU tracker with ByteTrack or DeepSORT.
- Improve OCR accuracy for low-light and angled license plates.
- GPU acceleration for faster inference.
- Support additional regional license plate formats.

---

# Requirements

- Python 3.11 or later
- Webcam or video input
- YOLOv8 model weights (`yolov8n.pt`)
- License plate detection model (`license_plate_detector.pt`)

---

## Author

Developed by **Shriya Gupta**.
