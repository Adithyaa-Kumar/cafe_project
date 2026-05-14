# CAFE - Encrypted Traffic Classification System

A comprehensive traffic classification framework using deep learning to identify application types from network flows.

## Project Structure

```
cafe_project/
├── README.md                 # This file
├── requirements.txt          # Python dependencies
├── .gitignore               # Git ignore rules
│
├── train_encoder.py         # Phase 3: FlowTransformer Encoder training
├── flow_extractor.py        # Phase 1: Universal flow feature extractor
├── live_demo.py             # Live traffic classification demo
├── build_dataset.py         # Dataset preprocessing
│
├── models/                  # Trained models and metadata
│   ├── encoder_best.pt
│   ├── clf_head.pt
│   └── meta.json
│
├── results/                 # Results and KPI reports
│   └── kpi_report.txt
│
├── docs/                    # Documentation
├── utils/                   # Utility functions
├── screenshots/             # Demo screenshots
└── sample_data/             # Small reference samples
```

## Datasets Used

This project uses the following public datasets:

- **CESNET Advanced Similarity Flow Dataset**: https://data.cesnet.cz/cesnet/http/quic22/
- **5G Traffic Dataset (Kaggle)**: https://www.kaggle.com/datasets/felicegolden/5g-traffic-datasets

> **Note**: Datasets are not included in this repository due to their large size. Download them separately and place in the `data/raw/` directory.

## Installation

1. Clone the repository:
```bash
git clone https://github.com/Adithyaa-Kumar/cafe_project.git
cd cafe_project
```

2. Create a virtual environment:
```bash
python3 -m venv venv
source venv/bin/activate
```

3. Install dependencies:
```bash
pip install -r requirements.txt
```

## Usage

### Training the Encoder
```bash
python3 train_encoder.py
# Optional: python3 train_encoder.py --epochs 150 --batch 512
```

### Extracting Flow Features
```bash
python3 flow_extractor.py
```

### Live Traffic Classification
```bash
# Replay mode (no root required)
python3 live_demo.py --replay

# Fast mode for testing
python3 live_demo.py --replay --fast

# Live capture (requires sudo)
sudo python3 live_demo.py --live
# Optional: sudo python3 live_demo.py --live --iface wlan0
```

## System Architecture

**Phase 1**: Flow Feature Extraction
- Extracts 60-dimensional feature vectors from network flows
- Handles multiple data sources: CESNET QUIC22, 5G Kaggle, live packets

**Phase 2**: Dataset Preparation
- Preprocessing and normalization

**Phase 3**: FlowTransformer Encoder
- Self-attention based architecture
- Each of 60 features becomes a token
- Output: 128-dim L2-normalized embedding
- Hybrid loss: 0.3 × SupCon + 0.7 × CrossEntropy

## Performance Targets

- Intra-class cosine similarity: > 0.7
- Inter-class cosine similarity: < 0.3
- Test accuracy: ≥ 0.90
- Latency (P99): < 100ms

## License

[Add your license here]

## Contact

For questions or issues, please open a GitHub issue or contact the maintainers.
