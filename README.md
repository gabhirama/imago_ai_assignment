# Project Overview
This project is a research initiative to explore the integration of image understanding into a Retrieval-Augmented Generation (RAG) system. The goal is to enhance the capabilities of RAG systems by incorporating image analysis and understanding, particularly in the context of document retrieval and generation.

We have decided to split the project into three phases, each focusing on different aspects of the integration process. The phases are designed to build upon each other, gradually increasing the complexity and sophistication of the system.

Initial Setup
- Set up a Python environment with the necessary libraries.
- pip install -r requirements.txt
- Install Tesseract OCR for image processing from the command line (if an ubuntu system, use apt-get install tesseract-ocr).

# Project Phases
## Phase 1: Basic RAG + Simple Image Handling
### Goal: Get a minimal end-to-end system working.
### Agents to build (simplified):

Document Ingester: Basic PDF text extraction, identify image placeholders.
Text Retriever: Basic HF Space Sentence Transformer Embedding which can work on CPU
Image "Agent" (very basic): If an image is mentioned, just note its presence. Maybe extract filename as metadata.
Simple Orchestrator: Linear flow.
Response Generator: LLM generating based on retrieved text, perhaps with a note like "[Image present here]".
Focus: Setting up the pipeline, basic data flow.

## Phase 2: Introducing Image Relevance and Basic Analysis
### Goal: Make the system "aware" of images.
### Enhance/Add Agents:

Image Analysis Agent (v1): Classify image type (heuristic based on filename, or simple classifier). Basic relevance (e.g., is it a chart/diagram?). Maybe OCR if it's simple text.
Image-Aware Routing Agent (v1): If image is "relevant" and OCRed, add text to context. If "relevant" but not OCRed, add a placeholder message.
Orchestrator: More sophisticated routing to handle image analysis.
Focus: Answering Key Research Question 2 (strategies for handling images without raw text conversion).

Tesseract OCR Installation (for the OCR step):

This project requires Tesseract OCR to be installed on your system.

    Download the Windows installer from the official releases page.

    Install Tesseract and note the installation path (e.g., C:\Program Files\Tesseract-OCR).

    Add the installation directory to your system PATH so that the tesseract command is accessible from the command line.

Alternatively, you can specify the path to tesseract.exe directly in your code (see below).

## Phase 3: Advanced Image Understanding & Hallucination Mitigation
### Goal: Implement sophisticated image processing and explicit hallucination checks.
### Enhance/Add Agents:

Image Analysis Agent (v2): Use VLMs for captioning, VQA if applicable. Better relevance.
Image-Aware Routing Agent (v2): Implement deferral mechanisms, flagging importance.
Context Aggregation & Verification Agent: Implement NLI-based checks, source attribution.
Response Generator: Explicitly acknowledge knowledge gaps based on verification/routing flags.
Focus: Answering Key Research Questions 1 & 4.
