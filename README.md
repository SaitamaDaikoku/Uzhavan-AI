#  UZHAVAN AI

### Dialect-Aware Rural Tamil Voice Assistant for Farmers and Elderly Citizens

[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)
[![Python 3.10](https://img.shields.io/badge/Python-3.10-blue.svg)](https://www.python.org/)
[![PyTorch 2.1](https://img.shields.io/badge/PyTorch-2.1%2B-orange.svg)](https://pytorch.org/)
[![FastAPI](https://img.shields.io/badge/FastAPI-0.108-teal.svg)](https://fastapi.tiangolo.com/)
[![React](https://img.shields.io/badge/React-18-cyan.svg)](https://react.dev/)

---

##  Project Overview

**UZHAVAN AI** is an end-to-end, voice-first AI assistant designed for farmers and elderly citizens in rural Tamil Nadu. It explores Tamil speech recognition, regional dialect processing, agricultural knowledge retrieval, and accessible voice-based interaction.

The system is designed to support queries related to:

-  Agriculture and crop cultivation
-  Crop diseases and pest control
-  Fertilizers and soil management
-  Government welfare schemes and subsidies
-  Weather forecasts
-  Agricultural market prices
-  Public and rural services

UZHAVAN AI explores an open-source speech processing pipeline using models such as Whisper and wav2vec2, with a focus on regional Tamil dialects, agricultural terminology, Tamil-English code-mixed speech, and rural acoustic conditions.

---

##  System Architecture and Data Flow

```text
User speaks Tamil audio
        |
        v
Audio preprocessing and noise reduction
        |
        v
Tamil Automatic Speech Recognition (ASR)
        |
        v
Transcribed Tamil text
        |
        +----------------------------+
        |                            |
        v                            v
Dialect classification        Intent classification
(CNN + BiLSTM / wav2vec2)     (Multilingual language model)
        |                            |
        +-------------+--------------+
                      |
                      v
             Tamil text normalization
                      |
                      v
          Agricultural RAG system
          (ChromaDB + embeddings)
                      |
                      v
             Response verification
                      |
                      v
            Tamil text-to-speech
                      |
                      v
                Voice response
```

An additional multimodal pathway explores combining leaf images with spoken symptom descriptions for crop disease assistance.

---

##  Key Features and Research Contributions

1. **Dialect-Conditioned Speech Recognition:** Investigates regional dialect identification and dialect-aware prompting to improve Tamil speech recognition.

2. **Tamil Diglossia Normalization:** Explores normalization between colloquial spoken Tamil (*Kodumtamil*) and formal written Tamil (*Centamil*).

3. **Acoustic-Phonetic Prosody Fusion:** Investigates the combination of Mel-spectrogram features, pitch, formant-related features, and vowel-duration information for dialect classification.

4. **Multimodal Agricultural Assistance:** Explores combining leaf images using EfficientNet-B0 with spoken crop symptom descriptions.

5. **Grounded Agricultural RAG:** Retrieves relevant agricultural information and explores verification mechanisms for fertilizer, pesticide, and government-scheme responses.

6. **TRAAN Rural Acoustic Benchmark:** Defines simulated rural acoustic conditions, including tractor noise, pump-set noise, wind, and microphone distortion.

7. **Elderly-Friendly Interface:** Focuses on readable typography, large touch targets, adjustable speech rate, and repeat playback.

8. **Research Documentation:** Includes research material and benchmark tables intended to support systematic evaluation of the proposed approach.

---

##  Dataset Catalog and Sources

The following resources may support Tamil speech and language experiments. Verify dataset availability, licensing, access requirements, and suitability before use.

| Dataset / Resource | Potential Use | Source |
|---|---|---|
| Mozilla Common Voice Tamil | Tamil speech recognition | [Common Voice](https://commonvoice.mozilla.org/) |
| OpenSLR | Open speech datasets | [OpenSLR](https://www.openslr.org/) |
| AI4Bharat | Indian-language speech and language processing | [AI4Bharat](https://ai4bharat.iitm.ac.in/) |
| DravidianLangTech | Dravidian language processing tasks | [DravidianLangTech](https://dravidianlangtech.github.io/) |
| Tamil Nadu Agricultural University | Agricultural reference material | [TNAU](https://tnau.ac.in/) |

Public datasets may not provide ready-to-use labels for individual rural Tamil dialects. Additional data collection and annotation may be required.

Any custom audio collection should use appropriate consent, privacy safeguards, and dataset documentation.

---

##  Quick Start and Installation

### Prerequisites

- Git
- Python 3.10 or a compatible version
- Node.js and npm for the frontend
- Conda or Python virtual environment support

### 1. Clone the repository

```bash
git clone https://github.com/SaitamaDaikoku/Uzhavan-AI.git
cd Uzhavan-AI
```

### 2. Set up the Python environment

If `environment.yml` is available:

```bash
conda env create -f environment.yml
conda activate uzhavan-ai
```

Alternatively, if `requirements.txt` is available:

```bash
python -m venv .venv
```

On Windows PowerShell:

```powershell
.\.venv\Scripts\Activate.ps1
pip install -r requirements.txt
```

### 3. Configure environment variables

If `.env.example` exists, create a local `.env` file:

```bash
cp .env.example .env
```

On Windows, you can also copy the file using File Explorer or PowerShell.

Add only the credentials required by the components you intend to run. Never commit API keys, access tokens, or other secrets.

---

##  Execution and ML Pipelines

The following commands describe the intended workflow. Confirm that the corresponding scripts and configuration files exist in the current repository before running them.

### Step 1: Download and prepare datasets

```bash
python scripts/download_datasets.py --dataset catalog
python scripts/download_datasets.py --dataset openslr_65
python scripts/prepare_dataset.py --source all
```

### Step 2: Launch the dialect data collection tool

```bash
python data_collection/app.py
```

If the application starts successfully, open `http://localhost:5001` in your browser.

### Step 3: Train the ASR model

```bash
python ml/asr/train.py --config configs/asr.yaml --epochs 1 --batch_size 2
```

For a full training run, if supported by the script:

```bash
python ml/asr/train.py --config configs/asr.yaml
```

### Step 4: Train the dialect classifier

```bash
python ml/dialect/train.py --config configs/dialect.yaml
```

### Step 5: Train the intent classifier

```bash
python ml/intent/train.py --config configs/intent.yaml
```

### Step 6: Train the crop disease model

```bash
python ml/crop_disease/train.py --config configs/crop.yaml
```

### Step 7: Run evaluation scripts

```bash
python ml/asr/evaluate.py
python ml/dialect/evaluate.py
```

### Step 8: Launch the application

Backend:

```bash
uvicorn backend.main:app --reload --port 8000
```

Frontend, in a separate terminal:

```bash
cd frontend
npm install
npm run dev
```

Open the local URL printed by the frontend development server.

---

##  Docker Deployment

If a working Docker Compose configuration is available in the repository, start the application using:

```bash
docker compose up --build
```

The actual ports and available services depend on the Docker configuration.

---

##  Empirical Benchmark Results (TRAAN Stress Test)

The research dossier documents benchmark results across speech recognition, dialect identification, crop disease diagnosis, agricultural response grounding, and inference performance.

> **Reproducibility note:** The values below reproduce the figures documented in the project research dossier. They should be treated as reported research results until the corresponding datasets, experimental procedures, model configurations, and evaluation runs have been independently reproduced and documented.

### Table I: ASR Performance by Dialect (WER and CER)

Word Error Rate (WER) and Character Error Rate (CER) measure speech recognition errors. Lower values indicate fewer transcription errors.

| Dialect | WER Baseline | WER Ours | CER Baseline | CER Ours |
|---|---:|---:|---:|---:|
| Standard Tamil | 18.4% | 12.7% | 8.2% | 5.4% |
| Kongu Tamil | 34.1% | 25.8% | 15.6% | 11.2% |
| Madurai Tamil | 31.6% | 22.9% | 14.3% | 10.1% |
| Tirunelveli Tamil | 29.8% | 21.4% | 13.7% | 9.8% |
| Delta Tamil | 27.3% | 19.6% | 12.4% | 8.9% |
| Chennai Tamil | 22.1% | 15.8% | 9.9% | 6.7% |
| Code-Mixed (Tanglish) | 38.7% | 28.3% | 17.1% | 12.4% |
| **Macro Average** | **28.9%** | **20.9%** | **13.0%** | **9.2%** |

### Table I-B: Speech Recognition WER Under Field Noise

The TRAAN (Tamil Rural Agro-Acoustic Noise) benchmark describes evaluation under simulated signal-to-noise ratio (SNR) conditions.

| Model Pipeline | Clean | 20 dB | 15 dB | 10 dB | 5 dB | 0 dB |
|---|---:|---:|---:|---:|---:|---:|
| Vanilla Whisper Small | 21.4% | 24.8% | 29.2% | 36.5% | 47.3% | 62.1% |
| Dialect-Conditioned Whisper | 15.2% | 17.6% | 20.8% | 25.4% | 32.7% | 43.8% |
| Dialect-Conditioned Whisper + Diglossia Normalizer | **11.8%** | **13.9%** | **16.5%** | **20.1%** | **26.3%** | **35.9%** |

### Table II: Regional Dialect Identification Macro-F1

Macro-F1 evaluates classification performance across dialect classes. Higher values indicate better performance.

| Acoustic Architecture | Clean | 20 dB | 15 dB | 10 dB | 5 dB | 0 dB |
|---|---:|---:|---:|---:|---:|---:|
| CNN-BiLSTM (Mel Only) | 0.84 | 0.81 | 0.76 | 0.69 | 0.58 | 0.44 |
| wav2vec2-XLSR-53 | 0.88 | 0.86 | 0.82 | 0.77 | 0.68 | 0.53 |
| Prosody-Enhanced Fusion | **0.92** | **0.90** | **0.87** | **0.83** | **0.76** | **0.65** |

### Table III: Crop Disease Diagnostic Accuracy Under Field Blur

This comparison examines image-only diagnosis against a multimodal approach combining leaf images and spoken symptom descriptions.

| Diagnostic Modality | Clean Leaf Photo | Blurry / Glared Field Photo |
|---|---:|---:|
| Leaf Vision Alone (EfficientNet-B0) | 89.2% | 63.4% |
| Multimodal Vision + Voice Symptoms | **94.8%** | **84.6%** |

The reported figures correspond to a 21.2 percentage-point improvement on blurry images, from 63.4% to 84.6%.

### Table IV: Agricultural RAG Safety and Hallucination Suppression

The Agricultural Hallucination Index (AHI) is defined in the research dossier as the proportion of generated claims unsupported by the designated knowledge base. Lower AHI values are preferable.

| Retrieval Framework | Faithfulness | AHI | Dosage Accuracy |
|---|---:|---:|---:|
| Standard Unconstrained LLM | 71.0% | 0.28 | 64.2% |
| UZHAVAN Grounded RAG + Guardrail | **96.0%** | **0.02** | **98.7%** |

### Table V: Inference Latency, Model Footprint, and Throughput

| Module / Model | Parameters / Structure | Model Size | Mean Latency | Throughput |
|---|---|---:|---:|---:|
| Standard XGBoost Recommender | 24 trees | 1.8 MB | 455.3 µs | 2,196 QPS |
| Fast JIT Engine | 24 trees | 0.2 MB | 10.9 µs | 92,008 QPS |
| Wake Word DS-CNN | 25.4 K parameters | 0.1 MB | 1.8 ms | 550 QPS |
| MobileNetV3-Small | 2.5 M parameters | 9.8 MB | 14.2 ms | 70 QPS |
| DistilMuRIL Intent Classifier | 66 M parameters | 250 MB | 28.5 ms | 35 QPS |

*QPS = queries per second. Latency and throughput depend on hardware, runtime, batch size, and measurement methodology.*

### Evaluation Methodology

For reproducible evaluation, document the following for each experiment:

- Dataset source, sample count, dialect distribution, and train/validation/test split.
- Baseline model versions, configurations, and preprocessing steps.
- Noise generation procedure and SNR settings for TRAAN experiments.
- Definitions and implementations of WER, CER, Macro-F1, diagnostic accuracy, faithfulness, and AHI.
- Hardware, inference settings, number of repetitions, and latency measurement methodology.
- Scripts, configuration files, and logs used to generate each table.

The reported values should be considered experimentally validated only after the corresponding experiments and measurements have been completed and verified.

---

##  Privacy and Ethical Considerations

- **Consent:** Obtain informed consent before recording or storing user speech.
- **Data minimization:** Avoid collecting unnecessary personal information.
- **Anonymization:** Protect stored recordings and use anonymized identifiers where appropriate.
- **Agricultural safety:** Verify pesticide and fertilizer guidance against authoritative sources before presenting it as actionable advice.
- **Government information:** Provide source references and clearly indicate when scheme details, eligibility, or deadlines cannot be verified.
- **Responsible responses:** Communicate uncertainty and direct users to qualified agricultural experts when reliable information is unavailable.

##  Future Enhancements

- Expand and annotate regional Tamil speech datasets.
- Fine-tune and benchmark speech recognition models.
- Evaluate dialect classification under different noise conditions.
- Integrate verified agricultural knowledge sources.
- Test image-and-voice crop assistance against documented baselines.
- Conduct usability testing with representative users.
- Publish reproducible experiments and measured evaluation results.

##  License

This project is intended to be distributed under the MIT License. See the [`LICENSE`](LICENSE) file for the applicable terms.

---

**UZHAVAN AI — Exploring accessible, Tamil-first AI assistance for rural agriculture.**
