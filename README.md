# Vision Transformer (ViT) 

## Overview
Implementation of a Vision Transformer (ViT) from scratch in PyTorch, trained on a subset of CIFAR-10 for image classification.

## Setup
1. Clone the repository
2. Create a virtual environment:
   ```bash
   python -m venv venv
   source venv/bin/activate
   ```
3. Install dependencies:
   ```bash
   pip install -r requirements.txt
   ```
4. Set your student ID in `.env`:
   ```
   STUDENT_ID=your_id_here
   ```

## Running
Train baseline and run all experiments:
```bash
python vit_template.py --mode all
```

Train baseline only:
```bash
python vit_template.py --mode baseline
```

## Project Structure
```
├── vit_template.py        # Main implementation
├── report.md              # Analysis report
├── training_log.json      # Baseline training log
├── check-git.py           # Git history verification script
├── requirements.txt       # Dependencies
├── .gitignore             
├── checkpoints/           # Saved model checkpoints
├── ablation_logs/         # Ablation experiment logs
├── analysis/              # Quantitative analysis results
└── data/                  # CIFAR-10 dataset
```

## Results
- Baseline validation accuracy: 48%
- Best patch size: 4
- Best number of layers: 2


