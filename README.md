# Vehicle Detection & Adaptive Traffic Signal Simulation Using MOG2

A computer vision project that detects and tracks vehicles in traffic footage using **OpenCV's MOG2 background subtraction algorithm**. It also simulates adaptive green-light timing based on the cumulative number of detected vehicles.

The project uses traditional computer vision techniques **without deep learning** and exports four videos to visualize each processing stage.

## Features

- **Region of Interest (ROI):** Focuses processing on a selected portion of the road.
- **MOG2 background subtraction:** Separates moving foreground objects from the background.
- **Morphological filtering:** Reduces noise using opening and closing operations.
- **Contour-based detection:** Identifies candidate vehicles using contour area and bounding-box aspect ratio.
- **Centroid-based tracking:** Assigns IDs to detected vehicles and matches detections across frames by centroid distance.
- **Adaptive traffic-light simulation:** Displays a calculated green-light duration based on the cumulative vehicle ID count.
- **Four-stage video export:** Saves separate videos for ROI visualization, raw MOG2, cleaned masks, and final tracking.

## Tech Stack

- Python
- OpenCV (`cv2`)
- NumPy
- Python standard library (`math`)

## Processing Pipeline

1. **ROI selection:** Select the road area to analyze.
2. **Background subtraction:** Apply MOG2 to produce a foreground mask.
3. **Noise reduction:** Apply morphological opening and closing.
4. **Vehicle detection:** Extract contours and filter candidates by area and aspect ratio.
5. **Vehicle tracking:** Match object centroids to previously detected vehicles and assign IDs to new detections.
6. **Traffic signal simulation:** Estimate the green-light duration using the cumulative number of assigned vehicle IDs.
7. **Visualization and export:** Show each processing stage and save four MP4 files.

## Project Structure

```text
Vehicle-Detecion---MOG/
├── detect_mog2.py
├── VideoTest/
│   └── traffic_video.mp4       # Expected input video path
├── MOG/                      # Create before running; stores output videos
│   ├── layer1_roi.mp4
│   ├── layer2_raw_mog.mp4
│   ├── layer3_morphological.mp4
│   └── layer4_final_tracking.mp4
└── README.md
```

> **Note:** The `MOG/` directory is an output directory and may not exist in a fresh clone. Create it before running. The input video must be available at `VideoTest/traffic_video.mp4`, or you must change `VIDEO_PATH` in the script.

## Installation & Usage

**1. Clone the repository**

```bash
git clone https://github.com/Jozioo/Vehicle-Detecion---MOG.git
cd Vehicle-Detecion---MOG
```

**2. Install dependencies**

```bash
python -m pip install opencv-python numpy
```

**3. Prepare the input and output directories**

Place the input traffic video at `VideoTest/traffic_video.mp4`. Then create the output folder:

```bash
mkdir MOG
```

**4. Run the detection script**

```bash
python detect_mog2.py
```

The program displays four OpenCV windows. Press **Q** to stop processing. The generated videos are saved to `MOG/`.

> The script uses `cv2.imshow()`, so it requires a graphical desktop environment.

## Output Videos

| File | Description |
| --- | --- |
| `layer1_roi.mp4` | Original video with the selected ROI highlighted |
| `layer2_raw_mog.mp4` | Foreground mask produced by MOG2 |
| `layer3_morphological.mp4` | Foreground mask after morphological filtering |
| `layer4_final_tracking.mp4` | Bounding boxes, tracking IDs, cumulative count, and simulated green-light duration |

## Adaptive Green-Light Timing

The script estimates green-light duration using:

```text
Estimated green time = min(60, int(15 + cumulative_vehicle_count × 2.5))
```

- Base green-light duration: **15 seconds**
- Added time per counted vehicle: **2.5 seconds**
- Maximum green-light duration: **60 seconds**

**Important:** In the current implementation, `cumulative_vehicle_count` is based on the number of vehicle IDs assigned since processing started (`next_vehicle_id`), not the number of vehicles currently waiting at an intersection. Consequently, the timing is a demonstration of adaptive signal logic, not a validated real-world traffic controller.

## Limitations & Future Improvements

- **Counting accuracy:** A single vehicle may be assigned multiple IDs if tracking is interrupted.
- **Stationary vehicles:** Background subtraction may struggle with vehicles that stop for extended periods.
- **ROI configuration:** The ROI is defined using fixed frame proportions and may require adjustment for other camera angles.
- **Traffic timing:** A more realistic controller would use currently queued vehicles, lane-level density, or a virtual counting line instead of cumulative assigned IDs.
- **Evaluation:** Detection and counting accuracy have not yet been documented with a labeled benchmark.

Potential future improvements include more robust object tracking, lane-specific vehicle counts, quantitative evaluation, and adaptive timing based on real-time traffic density.
