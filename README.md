# ShopSmart LLM – End-to-End AWS Retail Q&A Demo

![Build](https://github.com/you/aws-shopsmart-llm/actions/workflows/ci.yml/badge.svg)
![License](https://img.shields.io/github/license/you/aws-shopsmart-llm)

⚡️ Fine-tuned Llama-2 that answers customer questions and writes product copy,
built entirely on AWS (Glue → SageMaker Pipelines → Amplify React UI).

## Quick Start (one-click deploy)
```bash
git clone https://github.com/you/aws-shopsmart-llm && cd infra
npm i && cdk deploy  # or cdk deploy -c env=dev
