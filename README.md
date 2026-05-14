CAFE — Context-Aware Flow Embeddings
Adaptive AI-Based Network Traffic Classification

Overview

Modern internet traffic is heavily encrypted using protocols such as TLS 1.3 and QUIC, making traditional Deep Packet Inspection (DPI) approaches increasingly ineffective, computationally expensive, and privacy invasive.

CAFE solves this problem by learning behavioural embeddings from flow-level network statistics without accessing payload data.

Instead of inspecting packet contents, the system analyzes:

Packet timing
Inter-arrival times
Jitter
RTT patterns
Packet-size distributions
Directionality behaviour

The model learns meaningful traffic representations and classifies encrypted traffic flows in real time.

Key Features
DPI-free encrypted traffic classification
Real-time inference with sub-3ms latency
Transformer-based embedding architecture
Contrastive learning for traffic separation
Source-agnostic feature extraction
Live traffic classification support
Extendable to unseen traffic classes
Works with Wireshark CSVs, CESNET flows, and tshark captures
Performance Metrics
KPI	Target	Achieved
Classification Accuracy	≥ 90%	94.04%
Intra-class Similarity	> 0.70	0.914
Inter-class Similarity	< 0.30	0.173
P99 Inference Latency	< 100ms	2.7ms
Pipeline Architecture
Raw Network Packets
        ↓
Feature Extraction
        ↓
60-Dimensional Behavioural Vector
        ↓
FlowTransformer Encoder
        ↓
128-Dimensional Embedding Space
        ↓
Contrastive Training
        ↓
k-NN / SVM Classification
        ↓
Traffic Prediction + Confidence Score
FlowTransformer Architecture
Input Features (60)
        ↓
Feature Embedding Layer
        ↓
Positional Encoding
        ↓
Transformer Encoder (3 Layers)
        ↓
Global Mean Pooling
        ↓
Projection Head
        ↓
128-Dimensional L2-Normalized Embedding
Encoder Specifications
Component	Configuration
Transformer Layers	3
Attention Heads	4
Hidden Dimension	64
FFN Dimension	256
Embedding Size	128
Dropout	0.1
Total Parameters	178,880
Training Objective

The model combines three losses:

Loss =
0.3 × Supervised Contrastive Loss
+ 0.5 × Cross Entropy Loss
+ 0.2 × Margin Loss
Purpose of Each Loss
Supervised Contrastive Loss
Pulls same-class embeddings closer together.
Cross Entropy Loss
Improves classification decision boundaries.
Margin Loss
Pushes different traffic classes apart in embedding space.
Feature Engineering

The system extracts 60 behavioural features from encrypted traffic flows.

Feature Group	Count
Timing Statistics	12
Volume Statistics	6
Packet Size Histograms	16
Inter-Packet Timing Histograms	16
Directionality Metrics	10
Total	60
Example Features
Inter-arrival time mean/std
Jitter
RTT estimation
Packet count
Byte rate
Burst score
TCP flag patterns
Packet size distributions
Datasets Used
1. 5G Traffic Dataset
Source: Kaggle
~190K extracted flows
Real 5G network captures
Includes:
Streaming
Gaming
Video conferencing
XR/Metaverse traffic
2. CESNET Advanced Flow Dataset
ISP backbone traffic dataset
~4.5M available flows
Real-world encrypted traffic
Includes:
Browsing
Streaming
Gaming
Video calls
Cloud traffic
3. Live Traffic Capture

Traffic captured using:

tshark
WSL2
Windows Chrome activity

Used for validating the model on real-world encrypted traffic.

Training Results
Epoch	Accuracy	Intra-class Sim	Inter-class Sim
1	33.65%	0.996	0.995
50	88.15%	0.958	0.728
100	92.40%	0.948	0.396
200	93.10%	0.924	0.228
300	93.81%	0.914	0.173

The model successfully learned to create tightly grouped embeddings for similar traffic while separating unrelated traffic classes.

Per-Class Performance
Class	Precision	Recall	F1-Score
Gaming	0.98	0.98	0.98
Video Call	0.96	0.96	0.96
Streaming	0.92	0.92	0.92
Browsing	0.91	0.90	0.91
Latency Benchmark
Metric	Value
Mean Inference	0.99ms
P95 Latency	2.1ms
P99 Latency	2.7ms

Hardware Used:

NVIDIA RTX 3050 Laptop GPU
16GB DDR5 RAM
Ubuntu 22.04 (WSL2)
CUDA 12.1
Installation
Clone Repository
git clone https://github.com/Adithyaa-Kumar/cafe-flow-embeddings.git
cd cafe-flow-embeddings
Create Virtual Environment
python3 -m venv venv
source venv/bin/activate
Install Dependencies
pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu121

pip install scikit-learn pandas numpy matplotlib tqdm pyarrow

pip install scapy dpkt
Usage
Step 1 — Build Dataset
python3 build_dataset.py
Step 2 — Train Encoder
python3 train_encoder.py --epochs 300 --lr 3e-4 \
  --w-sc 0.3 --w-ce 0.5 --w-mg 0.2 \
  --margin 0.20 --temp 0.05
Step 3 — Evaluate Model
python3 evaluate.py
Step 4 — Run Live Demo
python3 live_demo.py --live
Project Structure
cafe-flow-embeddings/
│
├── build_dataset.py
├── train_encoder.py
├── evaluate.py
├── live_demo.py
│
├── data/
├── models/
├── results/
│
└── requirements.txt
Comparison with Existing Systems
System	DPI-Free	Encrypted Traffic	Real-Time	Embedding-Based
nPrintML	❌	❌	⚠️	❌
FlowPic	✅	⚠️	⚠️	❌
CESNET-TLS22	✅	✅	⚠️	❌
PacketCLIP	✅	✅	❌	✅
CAFE	✅	✅	✅	✅
Future Improvements
Multi-modal traffic embeddings
Federated learning support
Online incremental learning
Lightweight edge deployment
Self-supervised pretraining
Zero-shot traffic adaptation

Citation
@misc{cafe2025,
  title   = {CAFE: Context-Aware Flow Embeddings for Adaptive AI-Based Network Traffic Classification},
  author  = {Harshita K and Adithyaa K},
  year    = {2025},
  school  = {VIT Chennai},
  note    = {National Level Hackathon Project}
}

Authors
Adithyaa K
Harshita K
Students at VIT Chennai

Acknowledgements
CESNET Advanced Flow Dataset
Kaggle 5G Traffic Dataset
PyTorch
Supervised Contrastive Learning research by Khosla et al.
