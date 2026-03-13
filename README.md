# Drawing Recognition Model

Train a model for digit, letter, and shape recognition. Export to TensorFlow.js for Next.js.

## Setup

**TensorFlow supports Python 3.10–3.12 only.** If you see `No matching distribution found for tensorflow`, use Python 3.12:

### macOS (Homebrew)

```bash
# Install Python 3.12
brew install python@3.12

# Use it for this project
python3.12 -m venv venv
source venv/bin/activate

pip install -r requirements.txt
jupyter notebook
```

### Other options

- **Windows:** Install [Python 3.12](https://www.python.org/downloads/) and use `py -3.12 -m venv venv` then `venv\Scripts\activate`.
- **pyenv:** `pyenv install 3.12` then `pyenv local 3.12`.

### If you already have 3.10–3.12 as default

```bash
python3 -m venv venv
source venv/bin/activate   # Windows: venv\Scripts\activate
pip install -r requirements.txt
jupyter notebook
```

Then open `model_training.ipynb`.
