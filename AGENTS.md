# WiFi-3D-Fusion Agent Guidelines

## Build/Lint/Test Commands

### Installation & Setup
```bash
# Install all dependencies and setup environment
make install
# or
bash scripts/install_all.sh
```

### Training Commands
```bash
# Quick training (50 epochs)
./train_wifi3d.sh --quick

# Full training with continuous learning
./train_wifi3d.sh --continuous --auto-improve

# Custom training
./train_wifi3d.sh --source esp32 --epochs 200 --device cuda --continuous

# Manual training script
python train_model.py --config configs/fusion.yaml --device cuda --epochs 100
```

### Running the System
```bash
# Web-based real-time visualization (recommended)
python run_js_visualizer.py
python run_js_visualizer.py --source esp32
python run_js_visualizer.py --source nexmon

# Traditional pipeline
./run_wifi3d.sh esp32
./run_wifi3d.sh nexmon

# Monitor mode (requires sudo)
sudo -E env PATH="$PWD/venv/bin:$PATH" IFACE=mon0 python run_realtime_hop.py
```

### Testing & Evaluation
```bash
# Evaluate trained model
python tools/eval_reid.py --checkpoint env/weights/best_model.pth

# Record test sequences
python tools/record_reid_sequences.py --duration 60

# Simulate CSI data for testing
python tools/simulate_csi.py --samples 1000

# Run single test (example - adapt for specific test)
python -m pytest tools/eval_reid.py -v
```

### Docker
```bash
# Build and run with Docker
docker compose build
docker compose run --rm fusion
```

## Code Style Guidelines

### Imports
- Group imports: standard library, third-party packages, local modules
- Use absolute imports for local modules
- One import per line, except for related imports from same module
```python
import os
import sys
import numpy as np
import torch
import torch.nn as nn
from pathlib import Path
from src.common.config import load_cfg
```

### Naming Conventions
- **Variables/Functions**: `snake_case`
- **Classes**: `CamelCase`
- **Constants**: `UPPER_CASE`
- **Modules**: `snake_case`
- **Private methods**: `_leading_underscore`

### Type Hints
- Use type hints for function parameters and return values
- Import from `typing` module when needed
```python
from typing import Dict, List, Tuple, Optional

def process_csi_data(data: np.ndarray, config: Dict[str, any]) -> Tuple[np.ndarray, float]:
    pass
```

### Error Handling
- Use loguru for logging (already configured in common/config.py)
- Log important events and errors appropriately
- Handle exceptions gracefully with meaningful error messages
```python
from loguru import logger

try:
    # risky operation
    pass
except Exception as e:
    logger.error(f"Operation failed: {e}")
    raise
```

### Code Formatting
- Use 4 spaces for indentation
- Line length: aim for 100 characters or less
- Add docstrings for classes and functions
- Use meaningful variable names
- Add comments for complex logic

### File Structure
- Keep related functionality in appropriate modules under `src/`
- Use clear, descriptive filenames
- Follow existing patterns in the codebase

### Best Practices
- Write modular, reusable code
- Add proper error handling and logging
- Test changes before committing
- Follow security best practices (no hardcoded secrets, validate inputs)
- Use existing utilities and patterns from the codebase

### Dependencies
- Check existing imports before adding new dependencies
- Prefer packages already in requirements.txt
- For optional features, check if they're already conditionally imported</content>
<parameter name="filePath">AGENTS.md