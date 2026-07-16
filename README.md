# Moral Exemplar Effect of Virtual Idols in Chinese Higher Education

**A Quantitative Evaluation Framework Based on Attention-Weighted Multimodal Fusion**

This repository provides the complete research package accompanying the primary publication of the three-arm randomized controlled trial conducted at Hainan Vocational University between April and December 2024. It includes the raw dataset, all supporting research materials, and the full analysis codebase.

**Ethics approval.** HVU-2024032, Research Ethics Committee of Hainan Vocational University, approved 15 March 2024. All procedures conform to the Declaration of Helsinki.

**Principal Investigator.** Xinggan Fu, Associate Professor, School of Marxism, Hainan Vocational University (fxg0630@163.com).

---

## Study at a glance

| Field | Value |
|---|---|
| Design | Prospective, three-arm parallel-group randomized controlled trial |
| Arms | Control / Virtual Idol / Traditional (66 randomized per arm) |
| Total randomized | 198 undergraduate students |
| Completers | 186 (62 Control, 63 Virtual Idol, 61 Traditional) |
| Multimodal pilot corpus | 24 undergraduates, 57,600 two-second labeled affect windows |
| Instrument pilot validation | 156 undergraduates |
| Data collection period | 16 September – 23 December 2024 (pilot April – May 2024) |
| Analysis lock date | 31 January 2025 |
| Primary outcome | Moral Cognition (60-item Moral Knowledge Test) |
| Secondary outcomes | Moral Emotion (MES-R), Behavioral Intention (PTM + MBIQ composite), engagement, physiology, retention |

---

## Repository contents

The repository is distributed as three archives, each self-contained and independently downloadable.

### `data.zip`

Complete raw dataset released at the analysis lock date.

- `Raw_Dataset.xlsx` — twelve-sheet workbook covering baseline demographics, primary and secondary outcomes across five assessment time points, engagement indicators, physiological measures, anchor-item retention at 2 / 4 / 8 weeks, the multimodal pilot corpus, the instrument-pilot roster, thematic qualitative-feedback codes, and the two public pre-training datasets consulted.
- `Qualitative_and_Multimodal_Samples.xlsx` — representative qualitative-feedback records and six two-second multimodal derived-feature samples spanning all three arms; the full derived-feature corpus at native sampling rates is approximately 14 GB and is available on request.

### `materials.zip`

Regulatory and instrument documentation covering the trial.

- `Ethical_Approval.docx` — signed ethics approval issued by the Research Ethics Committee of Hainan Vocational University (Ref. HVU-2024032).
- `Informed_Consent_Statement.docx` — informed consent statement given to every participant, covering multimodal data acquisition, on-device processing with 24-hour deletion of raw video and speech, and the parasocial-dependence safeguards.
- `Assessment_Instruments.docx` — full text and scoring rules of the four measurement instruments (Moral Knowledge Test, Revised Moral Emotion Scale, Prosocial Tendencies Measure, Moral Behavioral Intention Questionnaire).
- `Moral_Content_Library.docx` — curated moral-education content library used across all three arms.

### `code.zip`

Companion analysis codebase (`meem-marl/`) implementing the full pipeline end-to-end.

```
meem-marl/
├── README.md
├── requirements.txt
├── data/                 # Raw dataset consumed by the analyses
├── src/
│   ├── config.py         # Global configuration and hyperparameters
│   ├── utils.py          # Cronbach's alpha, Cohen's d, mediation bootstrap
│   ├── preprocessing.py  # Median filter, z-score, sliding window, PCA, SMOTE
│   └── model.py          # MEEM-MARL model (encoders, attention fusion, Q-learning)
├── analysis/
│   ├── descriptive.py            # Baseline characteristics, internal consistency
│   ├── rct_analysis.py           # Primary outcomes over time, effect sizes
│   ├── engagement.py             # Engagement, physiology, exposure adjustment
│   ├── mediation.py              # Mediation, SEM, moderation
│   ├── retention_ablation.py     # Long-term retention, component ablation
│   └── sensitivity.py            # Six-way sensitivity analysis
├── figures/
│   └── figure_generator.py       # Full manuscript figure set
├── main.py                       # One-shot pipeline
└── run_model_demo.py             # End-to-end model forward pass on demo inputs
```

---

## Quick start

```bash
# Clone and enter the code archive
unzip code.zip && cd meem-marl

# Install requirements (Python >= 3.9)
pip install -r requirements.txt

# Run the full analysis pipeline (all tables and figures written to outputs/)
python main.py

# Run the model forward-pass demonstration
python run_model_demo.py

# Regenerate a specific figure
python figures/figure_generator.py --figure 7
```

Runtime on a laptop-class CPU is approximately 10 seconds for the full analysis pipeline and 4 seconds for the model demonstration.

---

## Method summary

**Multimodal acquisition** (six channels resampled to a common 50 Hz analysis grid):

- Eye tracking (Tobii Pro Fusion, 120 Hz)
- Facial video (1080p webcam, 30 Hz)
- Speech prosody (16 kHz microphone)
- Heart rate variability (chest-strap ECG monitor, 1000 Hz)
- Electrodermal activity (wrist-worn GSR sensor, 128 Hz)
- Event-driven interaction logs (native cadence)

**Preprocessing pipeline.** Median filter → z-score → sliding window (2 s, 50 % overlap) → PCA retaining 95 % variance → SMOTE (k = 5) applied within the training fold only.

**Per-modality encoders.** 1-D CNN over eye-tracking channels, CNN over 68 facial landmarks and 17 action units, Transformer over 88 acoustic features, LSTM over HRV and EDA, and MLP over aggregated interaction-log features.

**Attention-weighted late fusion.** Learned end-to-end via alpha_m = softmax(w_a^T F_m + b_a).

**Latent state estimation.** CNN-LSTM affect head producing seven-class emotion plus valence-arousal, together with engagement, social-presence, and cognitive-load heads.

**Three-stage moral modeling.** Attention → Retention → Reproduction, each producing a latent representation feeding a Q-learning policy over the action space {expression, tone, content, pacing}.

**Reward function.** A weighted combination of moral-cognition, moral-emotion, and behavioral-intention gains, penalized for cognitive load and for a parasocial-dependence index. Safeguards include a 30-minute reflective-prompt threshold, a 45-minute hard session ceiling, and automated referral of any participant whose socio-emotional dialogue ratio exceeds 0.60.

**Public datasets consulted for pre-training only.**

- CH-SIMS (2,281 Chinese multimodal video segments): <https://github.com/thuiar/MMSA>
- DEAP (32-participant peripheral physiological signals): <http://www.eecs.qmul.ac.uk/mmv/datasets/deap/>

---

## Reproducibility

Every analysis is deterministic given the raw dataset in `data.zip`. Table and figure outputs written to `meem-marl/outputs/` should match the values reported in the primary publication to within routine floating-point tolerance.

---

## Data protection

- Raw facial video and speech recordings were processed on the acquisition workstation and deleted within 24 hours of feature extraction. Only derived features are retained.
- The identifier-to-code linkage table was AES-256 encrypted and stored on an offline machine accessible only to the designated data manager, who took no part in the analysis.
- De-identified data are retained for five years after publication and then securely destroyed.

---

## Licensing and citation

All research materials are released for academic use. When using this repository, please cite the primary publication together with the two public pre-training datasets referenced above.

---

## Contact

Xinggan Fu — Principal Investigator, School of Marxism, Hainan Vocational University, Haikou, Hainan, 570216, China. Email: fxg0630@163.com.

Ximeng Wen — Corresponding Investigator, Hainan Vocational University.
