# AWS LLM Fine-Tuning Showcase&nbsp;![AWS ML Badge](https://img.shields.io/badge/AWS%20ML-Certified-green)&nbsp;![GitHub Repo](https://img.shields.io/badge/GitHub-Repo-brightgreen)

This repository demonstrates an **end-to-end MLOps workflow on AWS**.  
We fine-tune a Large Language Model (LLM) with **Amazon SageMaker JumpStart**, process Amazon customer-review data, track experiments, monitor for bias, deploy via pipelines, and surface interactive results in a **React app served by AWS Amplify**.

> **Key AWS services:** S3 · Athena · Glue · SageMaker (Data Wrangler, JumpStart, Experiments, Debugger, Clarify, Pipelines, Endpoints) · Amplify

---

## Table of Contents
- [Project Overview](#project-overview)
- [Dataset Description](#dataset-description)
- [Prerequisites](#prerequisites)
- [Phase 1: Project Setup & Data Preparation](#phase-1-project-setup-and-data-preparation)
- [Phase 2: Model Selection & Fine-Tuning](#phase-2-model-selection-and-fine-tuning-coming-soon)
- [Phase 3: Validation & Performance Evaluation](#phase-3-validation-and-performance-evaluation-coming-soon)
- [Phase 4: Pipeline Orchestration & Deployment](#phase-4-pipeline-orchestration-and-deployment-coming-soon)
- [Phase 5: Front-End Development & Presentation](#phase-5-frontend-development-and-presentation-coming-soon)
- [Results & Learnings](#results-and-learnings-coming-soon)
- [How to Run](#how-to-run)
- [Contributing](#contributing)
- [License](#license)

---

## Project Overview
This project builds a **production-style MLOps pipeline** to fine-tune an LLM on Amazon customer-review text.  
The resulting model can classify sentiment, generate summaries, or power other NLP features.

**Why this project?**
- Showcases *scalable* ML on AWS.
- Tackles *real-world* challenges ( GB-scale text, noisy inputs ).
- Bakes in ethics & monitoring (SageMaker Clarify, Debugger).
- Truly end-to-end: raw data in S3 ➜ deployed model ➜ web demo.

### Architecture (High-Level)
[S3] → [Athena / Glue ETL] → [SageMaker Data Wrangler]
→ [JumpStart LLM Fine-Tune] → [Experiments & Debugger]
→ [Pipelines] → [SageMaker Endpoint] → [React + Amplify]
*(Diagram source: `docs/architecture.png`, exported from draw.io.)*

**Expected Outcomes**
- Fine-tuned LLM with **F1 ≥ 0.85** on target task.  
- Managed SageMaker endpoint.  
- Mobile-friendly React demo.

---

## Dataset Description
We use the **Amazon US Customer Reviews** dataset.

| Property | Details |
|----------|---------|
| **Source** | [Kaggle link](https://www.kaggle.com/datasets/cynthiarempel/amazon-us-customer-reviews-dataset) (mirror of UCSD data by Julian McAuley) |
| **Size** | ≈ 20–30 GB unzipped, category-specific TSV files |
| **Subset** | **Electronics** (~5 M reviews) to reduce cost |
| **Use-case** | Sentiment classification / summarization |

### Key Columns

| Column          | Description                                   | Type    |
|-----------------|-----------------------------------------------|---------|
| `marketplace`   | Marketplace code (e.g. `US`)                  | string  |
| `customer_id`   | Unique customer ID                            | string  |
| `review_id`     | Unique review ID                              | string  |
| `product_id`    | ASIN                                          | string  |
| `product_title` | Product name                                  | string  |
| `star_rating`   | 1-to-5 stars                                  | int     |
| `helpful_votes` | Helpful-vote count                            | int     |
| `review_body`   | Full review text                              | string  |
| `review_date`   | Review date                                   | date    |

### Download & Upload

```bash
# 1 Install Kaggle CLI (once)
pip install kaggle

# 2 Ensure you have ~/.kaggle/kaggle.json

# 3 Download & unzip
kaggle datasets download -d cynthiarempel/amazon-us-customer-reviews-dataset \
  -p /tmp/reviews --unzip

# 4 Copy to S3
aws s3 cp /tmp/reviews/ s3://shopsmart-data/raw/amazon_reviews/ --recursive
