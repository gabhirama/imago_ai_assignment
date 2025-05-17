# Phase 1: Basic RAG + Simple Image Handling
## Goal: Get a minimal end-to-end system working.
### Agents to build (simplified):

Document Ingester: Basic PDF text extraction, identify image placeholders.
Text Retriever: Basic TF-IDF or simple embeddings + FAISS.
Image "Agent" (very basic): If an image is mentioned, just note its presence. Maybe extract filename as metadata.
Simple Orchestrator: Linear flow.
Response Generator: LLM generating based on retrieved text, perhaps with a note like "[Image present here]".
Focus: Setting up the pipeline, basic data flow.

# Phase 2: Introducing Image Relevance and Basic Analysis
## Goal: Make the system "aware" of images.
### Enhance/Add Agents:

Image Analysis Agent (v1): Classify image type (heuristic based on filename, or simple classifier). Basic relevance (e.g., is it a chart/diagram?). Maybe OCR if it's simple text.
Image-Aware Routing Agent (v1): If image is "relevant" and OCRed, add text to context. If "relevant" but not OCRed, add a placeholder message.
Orchestrator: More sophisticated routing to handle image analysis.
Focus: Answering Key Research Question 2 (strategies for handling images without raw text conversion).

# Phase 3: Advanced Image Understanding & Hallucination Mitigation
## Goal: Implement sophisticated image processing and explicit hallucination checks.
### Enhance/Add Agents:

Image Analysis Agent (v2): Use VLMs for captioning, VQA if applicable. Better relevance.
Image-Aware Routing Agent (v2): Implement deferral mechanisms, flagging importance.
Context Aggregation & Verification Agent: Implement NLI-based checks, source attribution.
Response Generator: Explicitly acknowledge knowledge gaps based on verification/routing flags.
Focus: Answering Key Research Questions 1 & 4.