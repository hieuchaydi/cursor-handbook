# 📘 CURSOR PROMPTS – Python AI (RAG, LLM, Computer Vision, ML/DL)

**Rules • Prompts • Debug cho AI/ML trong Cursor IDE**

> File này thuộc thư mục [python/](./README.md).

---

## Mục lục

- [1. Rules cho AI Projects](#1-rules-cho-ai-projects)
- [2. RAG (Retrieval-Augmented Generation)](#2-rag-retrieval-augmented-generation)
- [3. LLM Integration & Applications](#3-llm-integration--applications)
- [4. Computer Vision](#4-computer-vision)
- [5. Machine Learning (Traditional)](#5-machine-learning-traditional)
- [6. Deep Learning (PyTorch / TensorFlow)](#6-deep-learning-pytorch--tensorflow)
- [7. Debug Prompts](#7-debug-prompts)
- [8. Best Practices & Anti-patterns](#8-best-practices--anti-patterns)

---

## 1. Rules cho AI Projects

### 1.1 File `.cursor/rules/ai-ml.mdc`

```
---
description: Rules for AI/ML Python files
globs: *.py, notebooks/*.ipynb
---

- Type hints for all functions (Tensor, np.ndarray, pd.DataFrame, etc.)
- Docstrings explaining model architecture, input/output shapes, parameters
- Set random seeds everywhere (torch.manual_seed, np.random.seed, random.seed)
- Use configuration files (YAML/JSON) for hyperparameters, not hardcoded values
- Separate concerns: data loading, preprocessing, model, training, evaluation, inference
- Log metrics (loss, accuracy, etc.) with proper experiment tracking
- Handle GPU/CPU device automatically (torch.device)
- Save/load model checkpoints properly
- Use proper train/val/test splits (no data leakage)
- Memory management: del unused tensors, torch.cuda.empty_cache()
- Use .env for API keys (OpenAI, HuggingFace, etc.)
- Pin all library versions for reproducibility
```

---

## 2. RAG (Retrieval-Augmented Generation)

### 2.1 RAG Pipeline cơ bản

```text
@codebase

Create a RAG pipeline for [mô tả: Q&A over documents, knowledge base, chatbot]:

Architecture:
1. Document Loading → 2. Chunking → 3. Embedding → 4. Vector Store → 5. Retrieval → 6. Generation

Requirements:
- Document loader: [PDF / HTML / Markdown / DOCX / txt]
- Chunking strategy: [fixed-size / recursive / semantic]
- Embedding model: [OpenAI / HuggingFace / sentence-transformers]
- Vector store: [ChromaDB / Pinecone / Weaviate / FAISS / Qdrant]
- LLM: [OpenAI GPT / Claude / local model via Ollama]
- Prompt template with context injection
- Source citation in response
- Configuration-driven (model, chunk_size, top_k, etc.)
- Type hints and docstrings
- Error handling (API failures, empty results)
```

### 2.2 Document Processing

```text
@codebase

Create document processing pipeline for RAG:

Requirements:
- Load documents from [directory / S3 / database / URLs]
- Support formats: PDF (PyPDF2/pdfplumber), DOCX, HTML, Markdown, TXT
- Text extraction with metadata preservation (title, page, source)
- Chunking with overlap:
  - RecursiveCharacterTextSplitter or custom
  - chunk_size and chunk_overlap configurable
- Metadata extraction (title, author, date, headings)
- Deduplication
- Progress tracking
- Type: list[Document] with Document dataclass
```

### 2.3 Vector Store management

```text
@codebase

Create vector store management module:

Requirements:
- Use [ChromaDB / FAISS / Pinecone / Qdrant / Weaviate]
- Create / update / delete collections
- Add documents with embeddings + metadata
- Similarity search with score threshold
- Metadata filtering
- Batch operations for efficiency
- Persistent storage
- Index management (rebuild, optimize)
- Type-safe wrapper class
- Configuration (embedding dimension, distance metric)
```

### 2.4 RAG with LangChain

```text
@codebase

Create RAG application using LangChain:

Requirements:
- LangChain document loaders + text splitters
- Embedding model (OpenAI / HuggingFace)
- Vector store integration
- RetrievalQA or ConversationalRetrievalChain
- Custom prompt template
- Memory for conversation history (ConversationBufferMemory)
- Streaming response
- Source documents in response
- Evaluation metrics (relevance, faithfulness)
```

### 2.5 RAG with LlamaIndex

```text
@codebase

Create RAG application using LlamaIndex:

Requirements:
- VectorStoreIndex or KnowledgeGraphIndex
- Custom node parser
- Query engine with response synthesizer
- Metadata filters
- Sub-question query engine (for complex queries)
- Evaluation with ragas or custom metrics
- Caching for embeddings
- Streaming support
```

### 2.6 Advanced RAG Patterns

```text
@codebase

Implement advanced RAG pattern: [chọn 1 hoặc nhiều]

Patterns:
- Hybrid search (dense + sparse / BM25 + semantic)
- Reranking (Cohere Rerank / cross-encoder)
- Query expansion / HyDE (Hypothetical Document Embedding)
- Multi-step retrieval (parent-child chunks)
- Self-RAG (self-reflective retrieval)
- Agentic RAG (tool-using retrieval)
- Graph RAG (knowledge graph + vector search)

Requirements:
- Modular implementation
- Configurable pipeline steps
- Evaluation metrics
- A/B testing capability
- Logging retrieval quality
```

### 2.7 RAG Evaluation

```text
@codebase

Create RAG evaluation pipeline:

Metrics:
- Retrieval: Precision@K, Recall@K, MRR, NDCG
- Generation: Faithfulness, Relevance, Answer correctness
- Use [ragas / deepeval / custom metrics]

Requirements:
- Test dataset (question, expected_answer, expected_sources)
- Automated evaluation pipeline
- Human evaluation interface (optional)
- Results dashboard / report
- Compare different configurations
```

---

## 3. LLM Integration & Applications

### 3.1 LLM API Client

```text
@codebase

Create LLM API client for [OpenAI / Anthropic Claude / Google Gemini / local Ollama]:

Requirements:
- Async client with retry logic
- Streaming support (token by token)
- Message formatting (system, user, assistant)
- Token counting and cost estimation
- Rate limiting
- Error handling (API errors, timeout, rate limit)
- Response parsing (text, JSON, function calls)
- Configurable (model, temperature, max_tokens, etc.)
- Logging requests/responses (sanitized)
- Type-safe with Pydantic models
```

### 3.2 Prompt Engineering module

```text
@codebase

Create a prompt engineering module:

Requirements:
- Prompt template class with variables
- System prompt management
- Few-shot example management
- Chain-of-thought prompting support
- Output format specification (JSON mode, structured output)
- Prompt versioning
- A/B testing support
- Token usage tracking
- Template validation (all variables filled)
```

### 3.3 Function Calling / Tool Use

```text
@codebase

Implement LLM function calling for [mô tả: API calls, database queries, calculations]:

Requirements:
- Tool/function definitions (JSON schema)
- Function registry with type safety
- Execution loop (LLM → tool call → result → LLM)
- Error handling for tool execution
- Parallel tool calls support
- Validation of tool arguments
- Logging tool usage
- Security: whitelist allowed functions
```

### 3.4 LLM Agent

```text
@codebase

Create an LLM agent for [mô tả task]:

Requirements:
- Agent loop: observe → think → act → repeat
- Tool definitions and registry
- Memory (conversation + working memory)
- Planning capability
- Self-reflection / error correction
- Max iterations limit (prevent infinite loops)
- Structured output at each step
- Logging and observability
- Human-in-the-loop option
- Use [LangChain Agent / LlamaIndex Agent / custom]
```

### 3.5 Structured Output / JSON mode

```text
@codebase

Create structured output extraction using LLM:

Requirements:
- Pydantic models for output schema
- JSON mode or function calling for structured output
- Validation and retry on parse failure
- Nested/complex object extraction
- List extraction
- Handle partial/malformed responses
- Fallback strategy
```

### 3.6 Chatbot with Memory

```text
@codebase

Create a chatbot with conversation memory:

Requirements:
- Chat history management (list of messages)
- Memory strategies: [buffer / summary / token window / vector store]
- Context window management (truncate old messages)
- System prompt with personality/instructions
- Streaming response
- Multi-turn conversation support
- Session management (save/load conversations)
- User-specific memory (if multi-user)
```

### 3.7 LLM Fine-tuning pipeline

```text
@codebase

Create fine-tuning pipeline for [model]:

Requirements:
- Dataset preparation (JSONL format, train/val split)
- Data validation and cleaning
- Fine-tuning API call or local training (LoRA/QLoRA)
- Hyperparameter configuration
- Training monitoring (loss curves)
- Model evaluation (before/after)
- Model deployment
- Version management
```

---

## 4. Computer Vision

### 4.1 Image Classification

```text
@codebase

Create image classification pipeline:

Requirements:
- Dataset loading (ImageFolder / custom dataset)
- Data augmentation (transforms: resize, flip, normalize, etc.)
- Model: [ResNet / EfficientNet / ViT / custom CNN]
- Transfer learning (pretrained backbone + custom head)
- Training loop with validation
- Metrics: accuracy, precision, recall, F1, confusion matrix
- Model checkpoint saving (best val accuracy)
- Inference function with preprocessing
- Use [PyTorch / torchvision / timm]
- GPU support (device-agnostic code)
```

### 4.2 Object Detection

```text
@codebase

Create object detection pipeline:

Requirements:
- Dataset: [COCO format / Pascal VOC / YOLO format / custom]
- Model: [YOLOv8 / YOLO11 / Faster R-CNN / DETR / RT-DETR]
- Training with data augmentation (Albumentations)
- Evaluation: mAP, mAP@50, mAP@75
- Inference with bounding box visualization
- Non-maximum suppression
- Export model (ONNX / TensorRT / CoreML)
- Real-time inference (webcam/video)
- Use [ultralytics / detectron2 / torchvision]
```

### 4.3 Image Segmentation

```text
@codebase

Create image segmentation pipeline:

Requirements:
- Task: [semantic / instance / panoptic] segmentation
- Model: [U-Net / DeepLab / Mask R-CNN / SAM / SegFormer]
- Dataset with mask annotations
- Data augmentation (geometric + color, consistent masks)
- Loss function: [CrossEntropy / Dice / Focal / combined]
- Metrics: IoU, mIoU, Dice coefficient
- Visualization of predictions (overlay masks)
- Post-processing (CRF, morphological operations)
```

### 4.4 OpenCV image processing

```text
@codebase

Create image processing module using OpenCV:

Requirements:
- Operations: [resize, crop, rotate, filter, edge detection, contour, threshold, morphology, color space conversion]
- Input/output: file, numpy array, video frame
- Type hints (np.ndarray)
- Handle color spaces (BGR/RGB/Grayscale/HSV)
- Error handling (invalid image, wrong dimensions)
- Batch processing support
- Display utilities (show with matplotlib, not cv2.imshow for notebooks)
```

### 4.5 OCR (Optical Character Recognition)

```text
@codebase

Create OCR pipeline for [mô tả: receipts, documents, license plates, handwriting]:

Requirements:
- Use [Tesseract / EasyOCR / PaddleOCR / TrOCR]
- Image preprocessing (deskew, denoise, binarize, resize)
- Text detection + recognition
- Layout analysis (if structured document)
- Post-processing (spell check, format correction)
- Confidence scores
- Multi-language support if needed
- Batch processing
```

### 4.6 Video Processing

```text
@codebase

Create video processing pipeline for [mô tả: tracking, counting, action recognition]:

Requirements:
- Video capture (file / webcam / RTSP stream)
- Frame-by-frame processing
- Object tracking (DeepSORT / ByteTrack / BoT-SORT)
- FPS-aware processing (skip frames if needed)
- Output: annotated video / CSV metrics / real-time display
- Memory efficient (don't store all frames)
- GPU acceleration where possible
```

---

## 5. Machine Learning (Traditional)

### 5.1 ML Pipeline (scikit-learn)

```text
@codebase

Create ML pipeline for [classification / regression / clustering]:

Requirements:
- Data loading and EDA
- Feature engineering
- Train/val/test split (stratified if classification)
- Preprocessing pipeline (Pipeline + ColumnTransformer)
- Model selection: [specify or try multiple]
- Hyperparameter tuning (GridSearchCV / RandomizedSearchCV / Optuna)
- Cross-validation
- Evaluation metrics
- Feature importance analysis
- Model persistence (joblib)
- Prediction function with preprocessing
```

### 5.2 Feature Engineering

```text
@codebase

Create feature engineering pipeline for [dataset]:

Requirements:
- Numerical: scaling, binning, polynomial features, log transform
- Categorical: one-hot, target encoding, frequency encoding
- Text: TF-IDF, word count, sentiment score
- Time: cyclical encoding, lag features, rolling stats
- Missing values: imputation strategy per feature
- Feature selection (mutual info, chi2, LASSO)
- Pipeline compatible (fit/transform pattern)
- Type hints and documentation
```

### 5.3 Model Evaluation & Comparison

```text
@codebase

Create model evaluation framework:

Requirements:
- Multiple model comparison
- Cross-validation with multiple metrics
- Learning curves
- Calibration curves (for classification)
- SHAP values for interpretability
- ROC/PR curves (classification)
- Residual analysis (regression)
- Results table (model, metric, std)
- Export results and plots
```

---

## 6. Deep Learning (PyTorch / TensorFlow)

### 6.1 PyTorch Training Loop

```text
@codebase

Create PyTorch training pipeline:

Requirements:
- Dataset class (torch.utils.data.Dataset)
- DataLoader with proper workers, pin_memory
- Model definition (nn.Module)
- Training loop: forward → loss → backward → step
- Validation loop (torch.no_grad)
- Learning rate scheduler
- Gradient clipping (if needed)
- Mixed precision training (torch.cuda.amp)
- Checkpointing (save best model)
- TensorBoard / WandB logging
- Early stopping
- Device-agnostic (CPU/GPU/MPS)
- Reproducibility (seeds, deterministic)
```

### 6.2 Custom Dataset & DataLoader

```text
@codebase

Create custom Dataset for [mô tả data]:

Requirements:
- Inherit torch.utils.data.Dataset
- __len__ and __getitem__ methods
- Data augmentation (torchvision.transforms / albumentations)
- Lazy loading (don't load all into memory)
- Caching strategy if needed
- Proper type hints
- Collate function for variable-length data
- Train/val/test split
```

### 6.3 Custom Neural Network

```text
@codebase

Create a neural network for [task]:

Architecture: [describe or let AI choose]

Requirements:
- nn.Module subclass
- Forward method with proper shapes
- Weight initialization
- Regularization (dropout, batch norm, weight decay)
- Input/output shape documentation
- Torchinfo summary
- GPU compatible
```

### 6.4 Model Export & Deployment

```text
@codebase

Export model for deployment:

Requirements:
- Export to [ONNX / TorchScript / TensorRT / CoreML / TFLite]
- Input/output specification
- Optimize for inference (quantization, pruning)
- Inference wrapper class with preprocessing
- Batch inference support
- Latency measurement
- FastAPI/Flask serving endpoint
- Docker containerization
```

---

## 7. Debug Prompts

### 7.1 CUDA / GPU Error

```text
@terminal
@file [filepath]

CUDA error:
[paste: out of memory, device mismatch, CUDA not available]

Please:
- Identify the GPU issue
- Fix: reduce batch size, gradient accumulation, mixed precision
- Check device consistency (all tensors on same device)
- Add proper device handling
```

### 7.2 Shape Mismatch

```text
@terminal
@file [filepath]

Tensor shape mismatch:
[paste error: RuntimeError: mat1 and mat2 shapes cannot be multiplied]

Please:
- Trace shapes through the forward pass
- Identify where shapes diverge
- Fix model architecture or data preprocessing
- Add shape assertions at key points
```

### 7.3 Training Not Converging

```text
@codebase

Model training issues:
[mô tả: loss not decreasing, NaN loss, oscillating, overfitting]

Please:
- Check learning rate (try scheduler, lower LR)
- Check data preprocessing (normalization, augmentation)
- Check loss function
- Check for data leakage
- Add gradient monitoring
- Try gradient clipping
- Check batch size
- Verify data labels are correct
```

### 7.4 LLM API Error

```text
@terminal
@file [filepath]

LLM API error:
[paste: rate limit, timeout, invalid response, token limit exceeded]

Please:
- Handle the specific API error
- Add retry with exponential backoff
- Add token counting before request
- Add fallback strategy
- Implement proper error types
```

### 7.5 RAG Quality Issue

```text
@codebase

RAG producing bad results:
[mô tả: irrelevant retrieval, hallucination, missing context, wrong answers]

Please:
- Check chunking strategy (size, overlap)
- Check embedding model quality
- Check retrieval (top_k, score threshold)
- Improve prompt template (include instructions for using context)
- Add reranking step
- Check if relevant content exists in the knowledge base
- Add evaluation metrics to measure improvement
```

---

## 8. Best Practices & Anti-patterns

### ✅ DO

- Set random seeds cho reproducibility (torch, numpy, random, transformers)
- Dùng configuration files (YAML/JSON) cho hyperparameters
- Dùng experiment tracking (WandB, MLflow, TensorBoard)
- Dùng `.env` cho API keys
- Dùng type hints (Tensor, np.ndarray, pd.DataFrame)
- Dùng `torch.no_grad()` cho inference
- Dùng mixed precision training cho GPU
- Dùng data augmentation
- Dùng proper train/val/test splits (no leakage)
- Dùng model checkpointing
- Dùng `async` cho LLM API calls (batch parallel)
- Version control models và datasets

### ❌ DON'T

- Không hardcode API keys trong source code
- Không train trên test set (data leakage)
- Không ignore reproducibility (random seeds, deterministic algorithms)
- Không dùng CPU khi GPU available (check device)
- Không load toàn bộ dataset vào memory
- Không ignore memory management (del tensors, empty cache)
- Không dùng `model.eval()` mà quên `torch.no_grad()`
- Không trust LLM output without validation
- Không skip evaluation metrics
- Không deploy model chưa test đầy đủ

---

> 📌 Quay lại [Python Core](./README.md) | [Handbook chính](../cursor-ide-handbook.md) | Xem thêm: [Web](./web.md) | [Data](./data.md)
