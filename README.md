# computer-vision

📈 ECG Image Digitization Pipeline: This repository contains a robust computer vision pipeline designed to solve the challenge of ECG Digitization: transforming scanned or photographed ECG paper records back into high-fidelity numerical waveforms.
🏆 Kaggle Competition Performance:the pipeline achieved a high-ranking performance by focusing on modularity and visual quality assurance.
Public Leaderboard Score: ~21.62 dB SNRRanking: Top 15 (consistent with the 13th-14th place solution methodology)MetricScore (SNR)Public LB21.62Private LB21.37
🛠 Methodology & ArchitectureThe solution implements a three-stage modular pipeline. This approach breaks down the complex task of "pixels-to-signals" into manageable geometric and semantic steps.
Stage 0: Geometric NormalizationThe first step addresses real-world variability (rotation, skew, and scaling).Model: ResNet18 + U-Net.Function: Detects 13 lead markers and classifies the image orientation (8 possible rotations).Result: A standardized, upright 3024×4032 image.
Stage 1: Grid RectificationPaper ECGs are often wrinkled or photographed at an angle.Model: ResNet34 + U-Net.Function: Detects the underlying grid lines (44 horizontal, 57 vertical classes) to compute a perspective transformation.Result: A "flattened" image where the grid is perfectly square, essential for accurate time-voltage mapping.
Stage 2: Waveform Segmentation & DigitizationThis is the core "vision" component that extracts the electrical signals.Architecture: HRNet (High-Resolution Net) or U-Net with CoordConv.Innovation: * CoordConv: Injects 2D coordinates into the decoder so the model knows where it is in the ECG strip.Soft-Argmax / Centroid Extraction: Instead of simple thresholding, the model calculates the weighted center of the predicted mask for each column to get a continuous signal.Post-processing: Application of Einthoven’s Law ($Lead II = Lead I + Lead III$) to correct minor signal drifts.
📂 Project StructureBash├── physionet-ecg-images-digitization.ipynb  # Core implementation
├── submission.csv                           # Final output
├── snapshots/
│   ├── pg_1.jpeg                            # Leaderboard Rank 
│   ├── pg_2.jpeg                            # Submission Proof
│   └── architecture_diagram.png             # Visual Pipeline
└── README.md                                # You are here
🚀 Key FeaturesVisual QA Utilities: Built-in functions to visualize every stage (Original -> Rectified -> Mask -> Signal).End-to-End Training: Differentiable centroid conversion allows gradients to flow from signal loss back to the pixels.Heavy Augmentation: Robustness to shadows, creases, and faded ink via simulated Perlin noise and JPEG compression.
