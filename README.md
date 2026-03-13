# Drawing Recognition Model

A deep learning model I built for recognizing handwritten digits, letters (A–Z, a–z), and simple shapes from drawings. The model is trained to high accuracy and can be exported to **TensorFlow.js** for use in a browser—ideal for a Next.js portfolio where users draw on a canvas and get real-time predictions.

---

## What This Project Does

- **Recognizes 72 classes:** digits 0–9, uppercase A–Z, lowercase a–z, and 10 shapes (circle, square, triangle, star, line, zigzag, hexagon, diamond, sun, moon).
- **Training pipeline:** Loads MNIST, EMNIST (digits + by-class letters), and Quick Draw shape data; preprocesses and augments; trains a custom CNN with ResNet-style blocks and Squeeze-and-Excitation attention.
- **Export for the web:** Exports to TensorFlow.js (full and quantized), ONNX, SavedModel, and TFLite so you can run inference in the browser, on the edge, or on mobile.

---

## Method Overview

1. **Data:** Combined MNIST (70k digits), EMNIST Digits (280k), EMNIST ByClass (62 character classes), and Quick Draw (10 shape classes, 10k each). Images are 28×28 grayscale; unified to 72 classes.
2. **Preprocessing:** Resize to 32×32, normalize to [0, 1], center the drawing with a bounding box, ensure white-on-black (stroke=1, background=0), and apply contrast normalization.
3. **Augmentation (training only):** Rotation ±15°, shift/zoom ±10%, shear, elastic distortion, brightness/contrast jitter, Gaussian noise, GridDistortion, and optional erosion/dilation. No horizontal flip (to preserve d/b, p/q, 6/9).
4. **Model:** Custom CNN—stem block, then residual blocks with Squeeze-and-Excitation, then a dense head with dropout. Trained with AdamW, label smoothing, and cosine LR schedule with warmup.
5. **Evaluation:** Per-class accuracy, confusion matrix, and optional fine-tuning on the worst classes if accuracy is below 99.5%.

---

## How to Use This Project

- **Reproduce training:** Install dependencies (see below), open `model_training.ipynb` in Jupyter, and run all cells. Training can take a while depending on your hardware.
- **Use the trained model in an app:** After training, the notebook exports the model and config into `exports/`. Copy `exports/tfjs_model/`, `label_map.json`, and `model_config.json` into your Next.js app (e.g. under `public/models/`) and use the integration steps printed in the notebook (Cell 13).

---

## Installation

You need **Python 3.10, 3.11, or 3.12** (TensorFlow does not support 3.14 yet). Use a virtual environment so dependencies stay isolated.

### macOS

```bash
# Install Python 3.12 (if needed)
brew install python@3.12

# Clone and enter the project
git clone https://github.com/mravariya/Drawing-Recognition-Model.git
cd Drawing-Recognition-Model

# Create virtual environment and activate
python3.12 -m venv venv
source venv/bin/activate

# Install dependencies
pip install -r requirements.txt

# Run Jupyter and open the notebook
jupyter notebook
```

Then open `model_training.ipynb` and run the cells.

### Linux

```bash
# Ensure Python 3.10–3.12 is installed (e.g. Ubuntu/Debian)
sudo apt update
sudo apt install python3.12 python3.12-venv python3-pip

# Clone and enter the project
git clone https://github.com/mravariya/Drawing-Recognition-Model.git
cd Drawing-Recognition-Model

# Create virtual environment and activate
python3.12 -m venv venv
source venv/bin/activate

# Install dependencies
pip install -r requirements.txt

# Run Jupyter
jupyter notebook
```

### Windows

```bash
# Install Python 3.12 from https://www.python.org/downloads/
# During setup, check "Add Python to PATH"

# Open Command Prompt or PowerShell, then:
git clone https://github.com/mravariya/Drawing-Recognition-Model.git
cd Drawing-Recognition-Model

# Create virtual environment and activate
py -3.12 -m venv venv
venv\Scripts\activate

# Install dependencies
pip install -r requirements.txt

# Run Jupyter
jupyter notebook
```

Then open `model_training.ipynb` in your browser and run the cells.

---

## Project Structure

- **`model_training.ipynb`** — Full pipeline: data load, EDA, preprocessing, augmentation, model definition, training, evaluation, export, and Next.js instructions.
- **`requirements.txt`** — Python dependencies (TensorFlow, Jupyter, albumentations, etc.).
- **`exports/`** — Created after running the notebook: TF.js model(s), ONNX, SavedModel, TFLite, `label_map.json`, `model_config.json`.
- **`best_model.h5`** — Best checkpoint by validation accuracy (saved during training).
- **`best_model_finetuned.h5`** — Optional fine-tuned model if the fine-tuning cell was run.

---

## Using the Model in Next.js

1. Copy from `exports/` into your Next.js app:
   - `tfjs_model/` (or `tfjs_model_quantized/` for smaller size) → `public/models/tfjs_model/`
   - `label_map.json` → `public/models/label_map.json`
   - `model_config.json` → `public/models/model_config.json`

2. Install TensorFlow.js:  
   `npm install @tensorflow/tfjs`

3. In a client component, load the model and config from `/models/...`, preprocess the canvas (grayscale, resize to 32×32, normalize, invert to white-on-black), and run `model.predict()`. The notebook (Cell 13) prints full code snippets for loading and inference.

---

## Requirements

- Python 3.10, 3.11, or 3.12
- Enough RAM and disk for the datasets (MNIST, EMNIST, Quick Draw); a GPU is optional but speeds up training.

---

## License

This project is open source. Use and adapt it as you like. If you use the datasets (MNIST, EMNIST, Quick Draw), follow their respective terms and citations.

---

**Author:** Mahesh — Drawing Recognition Model for digits, letters, and shapes with export to TensorFlow.js for web apps.
