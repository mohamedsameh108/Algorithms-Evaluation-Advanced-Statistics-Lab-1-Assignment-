# 🔍 Image Segmentation Evaluation Pipeline

## 📌 Project Overview
This repository contains a robust and fully custom-built evaluation pipeline for binary image segmentation algorithms. The goal of this project is to compare the performance of 6 different segmentation models against Ground Truth (GT) masks. 

Instead of relying solely on built-in libraries, the core evaluation metrics were implemented **from scratch** using vectorized NumPy operations and KD-Trees to ensure deep understanding, high execution speed, and high precision.

---

## ⚙️ Features & Methodology
The pipeline evaluates the models based on two main categories: **Spatial Overlap** and **Boundary Accuracy**.

### 1. ROC Curve & AUC (Area Under the Curve)
* **Implementation:** A custom, fast algorithm to calculate True Positive Rates (TPR) and False Positive Rates (FPR) by extracting optimal thresholds without slow `for` loops.
* **Purpose:** Evaluates the overall pixel-wise classification capability.

### 2. Jaccard Index (IoU) & Dice Coefficient
* **Implementation:** Dynamically searches for the best threshold that maximizes the Jaccard Index.
* **Purpose:** Measures the area overlap between the predicted mask and the ground truth.

### 3. Hausdorff Distance (HD)
* **Implementation:** Uses **Boundary Extraction** (Morphological Erosion) and **K-Dimensional Trees (KD-Tree)** via `scipy.spatial` to reduce time complexity from `O(N * M)` to a fraction of a second.
* **Purpose:** Evaluates the maximum boundary mismatch (worst-case error) between shapes.

### 4. Visual Sanity Checks
* **Implementation:** Automated generation of `6x4` image grids.
* **Purpose:** Allows for side-by-side visual comparisons of all algorithms against the original ground truth.

---

## 📊 Key Findings & Conclusion
Evaluating segmentation models requires looking beyond a single metric. Our comprehensive analysis of the 6 algorithms revealed the following insights:

* **The AUC Paradox (Algorithm 6):** Algorithm 6 achieved the highest AUC (**0.9044**) but had a terrible Jaccard Index (**0.0864**). This highlights the flaw of relying on AUC in highly imbalanced datasets (where the background dominates the image). The model learned to predict the background well but failed at detecting actual objects.
  
* **The "Stable but Poor" Model (Algorithm 4):** Algorithm 4 had the best "Average" Hausdorff distance only because it never outputted a completely blank mask. However, its boundaries were consistently wrong by a large margin (~470+ pixels minimum error) with a very low Jaccard Index.

* 🏆 **The Winning Model (Algorithm 3):** **Algorithm 3 is the superior model.** At its optimal threshold (**0.0941**), it achieved the highest Jaccard Index (**0.4904**) and Dice Coefficient (**0.6581**), showing excellent spatial overlap. Excluding one isolated edge case (an image where it predicted a blank mask), its minimum Hausdorff Distance was extremely accurate (**28 pixels**). It is highly recommended to deploy this model and handle the single edge-case through targeted image preprocessing.

---

## 🚀 Technologies Used
* **Python 3**
* **NumPy** (For fast, vectorized mathematical operations)
* **OpenCV (`cv2`)** (For image reading and morphological operations)
* **SciPy (`cKDTree`)** (For fast spatial distance calculations)
* **Matplotlib** (For plotting ROC curves and image comparison grids)
* **Scikit-Learn** (Used strictly for output validation and sanity checks against our custom implementations)
