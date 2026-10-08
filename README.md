# Lane Detection System using Python and OpenCV

A computer vision project that detects and highlights **left and right lane markings** in road images using classical image processing techniques: Gaussian blur, Canny edge detection, a trapezoidal Region of Interest, and the probabilistic Hough transform. The notebook explains every stage with markdown notes and shows the full process in a 6-panel visualization.

---

## Features

- **Left and right lane detection** using Canny edges and the probabilistic Hough transform
- **Slope-based line classification** to separate left and right lane segments
- **Lane averaging**: multiple line segments are combined into a single lane per side
- **Fallback detection**: if no lines are found, white lane markings are detected using thresholding and contours
- **Red lane overlay** drawn on top of the original image
- **6-panel visualization** showing the original image, result, before/after comparison, lane highlights, Canny edges, and ROI mask
- **Beginner-friendly notebook** with an explanation before every code cell

---

## How It Works

The pipeline has seven stages:

### 1. Loading the Image
The user uploads a road image (in Google Colab, using `files.upload()`). The image is validated and read with OpenCV in BGR format, and its shape is printed to confirm it loaded correctly.

### 2. Grayscale Processing
The image is converted to grayscale with `cv2.cvtColor`, and a Gaussian blur with a 5×5 kernel is applied. Grayscale removes color information that is not needed, and the blur reduces noise so edge detection is more stable.

### 3. Edge Detection
Canny edge detection (`cv2.Canny`, thresholds 50 and 150) finds sharp brightness changes. Lane markings contrast with the road surface, so they show up as strong edges.

### 4. Region of Interest (ROI)
A trapezoid mask keeps only the road area in front of the vehicle and removes the sky, trees, and buildings. The trapezoid spans from 10% to 90% of the image width at the bottom, narrowing toward the middle of the image, which matches how lanes converge in perspective.

### 5. Hough Transform
The probabilistic Hough transform (`cv2.HoughLinesP`) detects straight line segments in the masked edge image. The parameters used are a threshold of 30, a minimum line length of 40 pixels, and a maximum line gap of 100 pixels.

### 6. Lane Classification
Detected segments are filtered and sorted by slope:
- Nearly horizontal lines (`|slope| ≤ 0.3`) and lines in the top part of the image are discarded
- **Negative slope** → left lane
- **Positive slope** → right lane

The slopes and intercepts of the segments on each side are averaged to produce one line per lane.

### 7. Overlay
Each averaged lane is extended from the middle of the image down to the bottom and drawn as a filled red band on a copy of the original image. If the Hough transform finds no lanes, a fallback detects bright white regions with thresholding and contours and highlights the two largest.

---

## Project Structure

```
lane-detection-opencv/
├── lane_detection.ipynb     # Main notebook with the full pipeline
├── test_images/             # Sample road images
├── test_videos/             # Sample road videos
├── output/                  # Lane detection results
├── requirements.txt         # Python dependencies
└── README.md
```

---

## Getting Started

### Option 1: Run in Google Colab (recommended)

1. Click the **Open in Google Colab** link at the top of this README.
2. Go to **Runtime → Run all**.
3. When the upload prompt appears, choose a road image (you can use one from `test_images/`).
4. View the 6-panel result at the bottom of the notebook.

### Option 2: Run locally

```bash
# Clone the repository
git clone https://github.com/varadjawkar25-oss/lane-detection-opencv.git
cd lane-detection-opencv

# Install dependencies
pip install -r requirements.txt

# Launch the notebook
jupyter notebook lane_detection.ipynb
```

The notebook uses `google.colab.files.upload()` for image selection. To run it locally, replace that upload code with a direct path:

```python
img_path = "test_images/solid_white_curve.jpg"
```

---

## Technologies Used

- **Python 3**
- **OpenCV**: grayscale conversion, Gaussian blur, Canny edge detection, Hough transform, contours
- **NumPy**: array operations, masks, slope and intercept averaging
- **Matplotlib**: 6-panel result visualization
- **Google Colab**: development and execution environment

---

## Limitations and Future Work

**Current limitations**
- Detects left and right lanes only; center lane detection is not implemented
- Lanes are modeled as straight lines, so sharp curves are only approximated
- The ROI and thresholds are fixed, so results depend on camera angle and image quality
- Shadows, glare, and worn lane markings can reduce accuracy
- Works on single images; the sample videos are not processed yet

**Planned improvements**
- Process video streams frame by frame
- Fit curved lanes with polynomial regression
- Add a perspective transform (bird's-eye view)
- Add temporal smoothing to reduce flicker across frames
- Explore deep-learning-based lane segmentation

---

## Author

**Varad Jawkar**
B.Tech, Electronics and Communication Engineering
Bharati Vidyapeeth (Deemed to be University), College of Engineering, Pune

- GitHub: [@varadjawkar25-oss](https://github.com/varadjawkar25-oss)
