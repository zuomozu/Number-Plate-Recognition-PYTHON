
# License Plate Recognition System

## Overview

This project implements a License Plate Recognition (LPR) system using computer vision techniques and Optical Character Recognition (OCR). The system processes video footage to detect, track, and recognize license plates, utilizing OpenCV for computer vision tasks and EasyOCR for text extraction. The project is designed to process and analyze video frames to detect license plates, extract their images, and recognize the text on them.

## Libraries Used

- **EasyOCR**: For Optical Character Recognition (OCR) to extract text from images.
- **OpenCV**: For image processing, object detection, and computer vision tasks.
- **NumPy**: For numerical operations and array manipulations.
- **Matplotlib**: For visualizing data (optional, as it's not used in the current script).
- **Imutils**: For additional image processing utilities, such as contour manipulation.

## Setup

### Prerequisites

Ensure Python is installed on your system. Install the required libraries using the following pip command:

```
pip install easyocr opencv-python numpy matplotlib imutils
```

### Project Structure

- **`main.py`**: Main script that handles video processing, license plate detection, and OCR.
- **`CLIIP.mp4`**: Example video file for testing the license plate detection.
- **`ENTRIES/`**: Directory for saving detected license plate images.
- **`saved_model/`**: Directory for saving model files (if applicable).

### Configuration

1. **Paths**: Update `basepath` and `CLIIP.mp4` to point to your directories and files in `main.py`.

2. **Video File**: Ensure `CLIIP.mp4` exists in the project directory or update the path to point to your video file.

## Usage

Run the `main.py` script to start processing the video. The script will detect and recognize license plates, saving images and printing detected information to the console.

```
python main.py
```

### Output

- **Detected License Plate Images**: Saved in the `ENTRIES/` directory.
- **Console Output**: Prints detected license plate numbers and other relevant information.

## Detailed Code Explanation

### Imports

```python
import easyocr  # For Optical Character Recognition (OCR)
import re       # For regular expression operations
import unicodedata  # For Unicode data processing (not used in the current script)
import cv2      # For computer vision tasks
import math     # For mathematical operations
import imutils  # For additional image processing utilities
import numpy as np  # For numerical operations
from matplotlib import pyplot as plt  # For data visualization (optional)
from datetime import datetime  # For date and time operations
import os       # For file and directory operations
from os import listdir  # For listing directory contents (not used in the current script)
```

### `EuclideanDistTracker` Class

This class tracks objects (license plates) across frames using their center points and Euclidean distance to avoid assigning new IDs to the same object.

```python
class EuclideanDistTracker:
    def __init__(self):
        self.center_points = {}
        self.id_count = 0

    def update(self, objects_rect):
        objects_bbs_ids = []
        for rect in objects_rect:
            x, y, w, h = rect
            cx = (x + x + w) // 2
            cy = (y + y + h) // 2
            same_object_detected = False
            for id, pt in self.center_points.items():
                dist = math.hypot(cx - pt[0], cy - pt[1])
                if dist < 25:
                    self.center_points[id] = (cx, cy)
                    objects_bbs_ids.append([x, y, w, h, id])
                    same_object_detected = True
                    break
            if not same_object_detected:
                self.center_points[self.id_count] = (cx, cy)
                objects_bbs_ids.append([x, y, w, h, self.id_count])
                self.id_count += 1
        new_center_points = {}
        for obj_bb_id in objects_bbs_ids:
            _, _, _, _, object_id = obj_bb_id
            center = self.center_points[object_id]
            new_center_points[object_id] = center
        self.center_points = new_center_points.copy()
        return objects_bbs_ids
```

### `OCR(img)` Function

This function performs OCR on the given image to extract and return the license plate number. 

```python
def OCR(img):
    reader = easyocr.Reader(['en'])
    result = reader.readtext(img)
    if not result:
        Number = None
    else:
        s_array = [sublist[1] for sublist in result]
        for z in s_array:
            if len(z) > 4:
                Number = re.findall('\d+', z)
            else:
                Number = z
            if len(z) == 5 and z.isdigit():
                break
    try:
        Number = [character for character in Number if character.isalnum()]
        Number = "".join(Number)
    except Exception:
        pass
    return (Number, c, e)
```

### Main Processing Loop

1. **Capture Video**: Opens and processes the video file frame by frame.
2. **Region of Interest (ROI)**: Defines the area in each frame where the license plate is likely to be located.
3. **Image Processing**: Converts the image to grayscale, applies a bilateral filter, and detects edges.
4. **Contour Detection**: Finds contours in the processed image and sorts them to identify potential license plate regions.
5. **Tracking**: Updates object positions and assigns IDs using `EuclideanDistTracker`.
6. **License Plate Extraction**: Saves images of detected license plates and performs OCR to extract text.
7. **Display and Output**: Displays the video feed with annotations and prints detected license plate information.




