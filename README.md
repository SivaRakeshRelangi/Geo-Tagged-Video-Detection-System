# Geo-Tagged-Video-Detection-System
** Taken fps as #1 and epochs as #2 because I train this in CPU, not GPU **

# Banner-OCR + Motion-AutoLabel → FasterRCNN (GPS-Joined)

This project provides an **end-to-end, Colab-ready pipeline** to convert a video into a dataset, auto-label motion below a banner, train a Faster R-CNN object detector, and join detections with OCR-extracted GPS coordinates.

---

## 🚀 Features
1. **Frame Extraction**  
   - Time-based seeking for stable frame sampling (robust to variable-FPS videos).
   - ![fpsandepochsettingshere](fpsandepochsettingshere.png) 
   - Fixed normalization to 848×448 resolution.  

2. **OCR (LAT/LON from Banner)**  
   - Crops the top `H_BANNER` pixels of each frame.  
   - Tries **3 preprocessing routes**:  
     - Otsu threshold on grayscale  
     - Morphological open/close  
     - White-on-blue HSV mask + Otsu  
   - Extracts LAT/LON (if present) + saves raw OCR text.  

3. **Motion Auto-Labeling**  
   - Finds moving blobs between consecutive frames.  
   - Clips boxes below the banner.  
   - Writes YOLO TXT files (class `0=object`).  

4. **Dataset Split**  
   - Train/Val split with option to include only frames containing labels.  

5. **Training (Faster R-CNN ResNet50 FPN)**  
   - Uses PyTorch torchvision detection API.  
   - Proper handling of **empty-label images** (zero-length tensors).  
   - Option to skip all-empty batches.  

6. **Inference + Join with GPS**  
   - Runs trained detector.  
   - Joins results with OCR metadata (time, LAT, LON).  
   - Outputs a `detections_with_gps.csv`.  

7. **Visualization (Optional)**  
   - Draws bounding boxes on sample frames.  
   - Titles include timecode, LAT, and LON.  

---

## 📂 Output Artifacts
- `/images` — extracted frames.  
- `/labels` — YOLO TXT auto-labels.  
- `metadata.csv` — per-frame timestamp, LAT, LON, raw OCR text.  
- `fasterrcnn_object.pth` — trained detector weights.  
- `detections_with_gps.csv` — joined predictions + GPS/time.  
- `/viz_val` — visualization frames with bounding boxes.  

---

## ⚙️ Setup (Google Colab)
1. Open the provided notebook:  
   `Video2Detections_GPS_Pipeline.ipynb`
2. Upload your video to Colab and set `VIDEO_PATH`.  
3. Run cells sequentially:
   - **0) Setup** — installs dependencies.  
   - **3) Extract Frames** — extracts images & OCR metadata.  
   - **4) Auto-Label Motion** — produces YOLO TXT labels.  
   - **5–7)** — Dataset split & FasterRCNN training.  
   - **8) Inference** — saves detections with GPS.  
   - **9) Visualization** — (optional) view sample outputs.  

---

## 🛠 Troubleshooting
- **OCR Issues**:  
  - Banner text may blur due to compression or motion.  
  - Use HSV mask route for better extraction of **white on blue**.  
  - Raw OCR is stored in `metadata.csv` for debugging.  

- **Motion Labels Mismatched**:  
  - Shadows or UI elements can be misclassified as motion.  
  - Adjust `MIN_AREA` to filter small blobs.  

---

## 📌 Suggested Task Name
**Banner-OCR + Motion-AutoLabel → FasterRCNN (GPS-Joined) Pipeline**  


---


