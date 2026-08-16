# Project Aurora — Self-Learning Notification Orchestrator

## Overview

**Project Aurora** is a domain-agnostic, AI-powered notification orchestration platform that automatically learns how, when, and why users should be engaged. Instead of relying on manually designed notification campaigns, Aurora combines Knowledge Bank intelligence, user behavioral analytics, machine learning, and reinforcement learning to continuously optimize communication strategies.

The system ingests:

* A company Knowledge Bank (`.md` / `.txt`)
* User behavioral data (`.csv`)
* Historical campaign performance data

Aurora then generates personalized notification schedules, evaluates user responses, learns from outcomes, and iteratively improves future communication strategies.

---

## Architecture

```text
Knowledge Bank + User Data + Experiment Results
                    │
                    ▼
        ┌─────────────────────┐
        │      Task 1         │
        │ Intelligence Layer  │
        └─────────────────────┘
                    │
                    ▼
        ┌─────────────────────┐
        │      Task 2         │
        │ Communication Layer │
        └─────────────────────┘
                    │
                    ▼
        ┌─────────────────────┐
        │      Task 3         │
        │ Learning Layer      │
        └─────────────────────┘
                    │
                    ▼
          Optimized Notification
                 Strategy
```

---

# Task 1 — System Architecture & Intelligence Design

Task 1 transforms raw business knowledge and behavioral data into actionable user intelligence.

---

## 1. Knowledge Bank Engine

**Model:** LLM integration

The Knowledge Bank Engine extracts strategic business understanding from any company documentation.

### Extracted Components

* North Star Metric
* Feature → Goal Mapping
* Tone Matrix
* Hook Matrix
* User Propensity Dimensions
* Journey Templates
* Goal Templates

### Example

```text
Feature: AI Tutor

Mapped Goal:
Increase daily learning consistency

Relevant Propensity:
AI Tutor Affinity

Tone:
Motivational

Hook:
Progress Tracking
```

---

## 2. User Data Ingestion Engine

Aurora performs a multi-stage validation pipeline before any modeling begins.

### Validation Layers

#### Layer 1 — Schema Validation

Verifies:

```python
required_columns = [
    "user_id",
    "lifecycle_stage",
    "sessions_per_week",
    "feature_usage"
]
```

---

#### Layer 2 — Type Validation

```python
sessions_per_week -> int
engagement_score -> float
```

---

#### Layer 3 — Range Validation

```python
0 <= engagement_score <= 100
```

---

#### Layer 4 — Missing Value Imputation

```python
median_imputation()
mode_imputation()
```

---

#### Layer 5 — Duplicate Removal

```python
df.drop_duplicates()
```

---

## 3. MECE Segmentation Engine

### Objective

Create mutually exclusive and collectively exhaustive user segments.

### Propensity Space

Generated from KB-derived dimensions:

```text
Gamification Affinity
AI Tutor Affinity
Leaderboard Affinity
Social Affinity
```

---

### Clustering

Aurora performs silhouette analysis across multiple values of K.

```python
for k in range(6,13):
    model = KMeans(k)
```

Range:

```text
k = 6 → 12
```

Best cluster count selected via:

```text
Maximum Silhouette Score
```

---

### Lifecycle-Normalized Clustering

Percentile normalization is applied within lifecycle groups to prevent clusters from becoming lifecycle labels.

```math
NormalizedValue =
\frac{x - min(x)}
{max(x)-min(x)}
```

performed independently for each lifecycle stage.

---

### Persona Generation

Personas are created using:

```text
Dominant Propensity
Activity Level
Churn Risk
```

Example:

```text
Competitive Active Loyalists
Social Passive Churners
AI-Powered Growth Seekers
```

---

## 4. Goal & Journey Builder

Generates:

### Primary Goals

```text
Complete first lesson
```

### Sub Goals

```text
Finish chapter 1
Take quiz
Review mistakes
```

### Journey Progression

```text
Day 1 → Onboarding
Day 3 → Habit Formation
Day 7 → Consistency
Day 14 → Retention
```

---

# Task 2 — Communication & Timing Intelligence

Task 2 determines what message should be sent and when.

---

## 1. Theme Engine

**Model:** LLM integration

Maps user personas to behavioral motivators using the Octalysis Framework.

### Octalysis Drives

```text
Epic Meaning
Accomplishment
Empowerment
Ownership
Social Influence
Scarcity
Unpredictability
Avoidance
```

---

### Output

Top 3 drives for each segment.

Example:

```text
Segment:
Competitive Learners

Top Drives:
1. Accomplishment
2. Social Influence
3. Scarcity
```

---

## 2. Template Generator

**Model:** LLM integraton

Generates:

```text
5 templates
× Segment
× Lifecycle
× Theme
```

---

### Characteristics

* Hindi + English
* Personalized
* Tone-aligned
* Hook-aware
* Concurrently generated

---

### Example

**English**

```text
You're just one quiz away from moving up the leaderboard.
Let's make it happen today.
```

**Hindi**

```text
बस एक क्विज़ और, और आप लीडरबोर्ड में ऊपर पहुंच सकते हैं।
आज ही प्रयास करें।
```

---

## 3. Timing Optimizer

Aurora predicts optimal communication windows.

### Candidate Models

```text
Random Forest
Gradient Boosting
Logistic Regression
Support Vector Machine
```

---

### Features

```text
Preferred Hour
Timezone
Activity Pattern
Usage Frequency
```

---

### Model Selection

Best-performing classifier selected automatically.

```python
best_model = max(models, key=model_accuracy)
```

---

### Output

For every user:

```text
Top 3 Preferred Time Zones
```

Example:

```text
8 AM
7 PM
10 PM
```

---

## 4. Schedule Generator

**Model:** 

Generates complete notification schedules.

### Inputs

```text
Segment
Lifecycle
Octalysis Drives
Templates
Preferred Timing
```

---

### Frequency Optimization

Based on activity level:

```text
Low Activity      → 3/day
Medium Activity   → 5/day
High Activity     → 9/day
```

---

### Channel Routing

```text
Push Notification
In-App Message
WhatsApp
SMS
```

---

### Final Output

```text
User → Notification
Time → Channel
Template → Goal
```

---

# Task 3 — Execution & Self-Learning Layer

This layer enables Aurora to continuously improve.

---

## 1. Self Learning Classification Engine

Aurora evaluates campaign performance using reinforcement learning principles.

### Reward Function

```math
Reward =
w_1(CTR)
+
w_2(Engagement)
-
w_3(Uninstall)
```

where:

```text
CTR = Click Through Rate
Engagement = Session Quality
Uninstall = User Loss Penalty
```

---

### Hyperparameter Optimization

Grid search is performed over reward weights.

```python
for ctr_w in ctr_weights:
    for engagement_w in engagement_weights:
        ...
```

---

### Bayesian Engagement Estimation

Aurora estimates true engagement probability.

```math
P(Engagement | Data)
```

allowing uncertainty-aware decisions.

---

### Classification

Campaigns are categorized using quantile thresholds.

```text
GOOD
NEUTRAL
BAD
```

with confidence gating.

---

## 2. Strategy Generator

Creates future traffic allocation strategies.

### Allocation

```text
GOOD     → 70%
NEUTRAL  → 25%
BAD      → 5%
```

---

### Safety Analysis

Checks:

```text
Uninstall Rate
Fatigue Risk
Over-notification
Segment Health
```

---

### Timing Analysis

Identifies:

```text
Best Hours
Worst Hours
High Conversion Windows
```

---

## 3. Goal Updater

---

### GOOD

```text
Preserve existing strategy
```

---

### NEUTRAL

```text
Generate A/B variants
```

---

### BAD

```text
Rewrite strategy completely
```

---

### Causal Reasoning

Example:

```text
Leaderboard notifications failed because
low-social users showed minimal response.

Recommended:
Achievement-focused messaging.
```

---

## 4. Iteration-1 Template Generator

Only modified templates are regenerated.

Benefits:

```text
Lower cost
Lower latency
Faster experimentation
```

---

## 5. Iteration-1 Schedule Generator

Applies RL insights to scheduling.

### Improvements

```text
Template Re-ranking
Timing Re-ranking
Frequency Reduction
Channel Optimization
```

---

### Guardrails

High-uninstall segments automatically receive:

```text
Reduced Notification Frequency
```

to prevent churn.

---

## 6. Delta Report Generator

Generates a complete audit trail.

### Captures

```text
What Changed
Why It Changed
Expected Impact
Observed Impact
```

---

### Example

```text
Previous:
5 Notifications/Day

Updated:
3 Notifications/Day

Reason:
High uninstall probability detected.

Expected Impact:
Reduced fatigue and improved retention.
```

---

# Technology Stack

| Component           | Technology             |
| ------------------- | ---------------------- |
| LLM Extraction      | Gemini 2.5 Flash       |
| Template Generation | Gemini REST API        |
| Segmentation        | K-Means                |
| Timing Optimization | Random Forest          |
| Timing Optimization | Gradient Boosting      |
| Timing Optimization | Logistic Regression    |
| Timing Optimization | SVM                    |
| Learning Engine     | Reinforcement Learning |
| Data Processing     | Pandas                 |
| Numerical Computing | NumPy                  |
| Model Persistence   | Joblib                 |

---

# Models Used

## Gemini 2.5 Flash

Used for:

* Knowledge extraction
* Theme generation
* Goal optimization
* Template generation
* RL-informed strategy updates

---

## scikit-learn Models

### K-Means

```text
User Segmentation
```

### Random Forest

```text
Timing Classification
```

### Gradient Boosting

```text
Timing Classification
```

### Logistic Regression

```text
Timing Classification
```

### Support Vector Machine

```text
Timing Classification
```

---

# Repository Structure

```text
Aurora/
│
├── company_kb.md
├── user_behavioral_data.csv
├── experiment_results.csv
│
├── run_pipeline.py
│
├── codebase/
│   ├── task1_aurora.py
│   ├── theme_engine.py
│   ├── generate_templates.py
│   ├── timing_optimizer.py
│   ├── schedule_generator.py
│   ├── task3_learning_engine.py
│   └── generate_delta_report.py
│
├── outputs/
│   ├── segments.csv
│   ├── templates.json
│   ├── schedules.csv
│   ├── rl_analysis.csv
│   └── delta_report.md
│
└── README.md
```

---

# Installation

```bash
pip install pandas numpy scikit-learn scipy joblib
pip install google-genai google-generativeai
```

---

# Running the Complete Pipeline

```bash
python run_pipeline.py \
--data user_behavioral_data.csv \
--kb company_kb.md \
--experiment experiment_results.csv
```

---

# Running Individual Modules

```bash
python codebase/task1_aurora.py

python codebase/theme_engine.py

python codebase/generate_templates.py

python codebase/timing_optimizer.py

python codebase/schedule_generator.py

python codebase/task3_learning_engine.py

python codebase/generate_delta_report.py
```

---

# Inputs

### user_behavioral_data.csv

```text
1500 users
Behavioral Features
Lifecycle Information
Feature Usage Metrics
```

### company_kb.md

```text
Business Knowledge Base
Goals
Features
Tone Guidelines
Journey Definitions
```

### experiment_results.csv

```text
Historical Campaign Results
CTR
Engagement
Uninstall Metrics
```

---

# Key Features

* Domain-Agnostic Architecture
* Fully Automated Segmentation
* Personalized Notification Scheduling
* Octalysis-Based Motivation Modeling
* Multi-Model Timing Optimization
* Reinforcement Learning Feedback Loop
* Automatic Goal Evolution
* Continuous Self-Improvement
* Explainable Delta Reporting
* Production-Ready Modular Design

---

# Team

**Hostel 2613**
**Kriti 2026 — SpeakX Challenge Submission**

Project Aurora demonstrates how modern LLMs, classical machine learning, behavioral science can be combined to build a fully autonomous notification intelligence platform capable of learning and improving engagement strategies over time.
