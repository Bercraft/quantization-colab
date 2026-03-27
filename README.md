# 🚀 GGUF Model Quantizer for llama.cpp & whisper.cpp

Accessible Google Colab notebook for quantizing Hugging Face models to GGUF format for llama.cpp and whisper.cpp, based on the work of [llama.cpp](https://github.com/ggerganov/llama.cpp) and [whisper.cpp](https://github.com/ggml-org/whisper.cpp).

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/Bercraft/gguf-quantizer-colab/blob/main/gguf-quantizer-colab.ipynb)

---

## 🚀 GGUF Quantizer - Features

* **One-click installation** - Automatically installs all dependencies and compiles both llama.cpp and whisper.cpp
* **Dual model support** - Quantize both LLM (text) models AND Whisper (audio) models
* **Simple configuration** - Just paste your Hugging Face model URL and select quantization method
* **Single quantization per run** - Memory-optimized to handle one model at a time on T4 GPU
* **Read/Write token support** - Separate tokens for downloading private models and uploading to Hub
* **Google Drive integration** - Automatically saves all quantized models to your Google Drive
* **Smart caching** - Skips already generated quantizations to save time and bandwidth
* **Resume capability** - Can resume from interruptions, keeping track of completed quantizations
* **Real-time progress** - Clear output and progress bars for better visibility
* **Comprehensive reporting** - Shows file sizes, total time, and success statistics
* **Auto-upload to Hub** - Option to automatically upload quantized models to Hugging Face Hub

---

## 📊 Quantization Guide

GGUF (GGUF Universal Format) is the native format for llama.cpp and whisper.cpp. Quantization reduces model size while sacrificing minimal quality.

### Quality vs Size Trade-off

| Type | Bits | Quality | Relative Size | Recommended Use |
|------|------|---------|---------------|-----------------|
| **F16** | 16 | ★★★★★ | 50% | Lossless, modern GPUs |
| **Q8_0** | 8.0 | ★★★★☆ | 25% | Almost lossless, great compromise |
| **Q6_K** | 6.5 | ★★★★☆ | 20% | Excellent for most cases |
| **Q5_K_M** | 5.5 | ★★★★☆ | 17% | Very high quality, good compression |
| **Q5_K_S** | 5.5 | ★★★☆☆ | 17% | High quality, slight savings |
| **Q4_K_M** | 4.5 | ★★★★☆ | 14% | 🏆 **BEST BALANCE** quality/size |
| **Q4_K_S** | 4.5 | ★★★☆☆ | 14% | Good balance for general use |
| **IQ4_XS** | 4.25 | ★★★☆☆ | 13% | Better quality for size with I-Matrix |
| **Q3_K_M** | 3.5 | ★★★☆☆ | 11% | Good compression for limited devices |
| **Q3_K_S** | 3.5 | ★★☆☆☆ | 11% | High compression for very limited devices |
| **IQ3_M** | 3.66 | ★★☆☆☆ | 11% | Medium compression with I-Matrix |
| **IQ3_XS** | 3.3 | ★★☆☆☆ | 10% | High compression with I-Matrix |
| **Q2_K** | 2.5 | ★☆☆☆☆ | 8% | Maximum compression, minimum quality |

### How to Choose:

1. **For professional/scientific use**: F16 or Q8_0
2. **Best quality/size balance**: Q4_K_M (recommended)
3. **Limited RAM devices (8-16GB)**: Q4_K_M or Q4_K_S
4. **Very limited devices (4-8GB)**: Q3_K_M or Q3_K_S
5. **Testing/experimentation only**: Q2_K

### Notes on Versions:
- **K-means** (Q*_K_*): K-means optimizations for better quality
- **I-Matrix** (IQ*_*): Uses Importance Matrix to preserve most important weights
- **S/M/L**: Small, Medium, Large indicate different optimization configurations

### 💡 Pro Tip:
For most users, **Q4_K_M** offers the best balance between quality and size.

---

## 🚀 How to Use

### Step 1: Open in Colab
Click the "Open in Colab" button above to launch the notebook.

### Step 2: Run Installation Cell
Execute the first cell to install all dependencies. This will:
- Install Python packages (torch, transformers, huggingface_hub, gguf)
- Clone and compile llama.cpp (for LLM models)
- Clone and compile whisper.cpp (for Whisper models)
- Verify installations and GPU availability

### Step 3: Configure Model & Quantization
Run the configuration cell where you can:
- **Model URL**: Paste your Hugging Face model URL or ID
  - Example: `mistralai/Mistral-7B-Instruct-v0.1`
  - Example: `openai/whisper-large-v3`
- **Model Type**: Select between "LLM (Text Models)" or "Whisper (Audio Models)"
- **Tokens** (optional):
  - **Read Token**: For downloading private/gated models
  - **Write Token**: For uploading quantized models to Hub
- **Quantization Method**: Select from 13 different quantization types
- **Storage Settings**: Choose Google Drive or local Colab storage
- **Upload Settings**: Option to auto-upload to Hugging Face Hub

### Step 4: Run Quantization
Execute the quantization cell. The notebook will automatically:
1. Download the model from Hugging Face (using read token if provided)
2. Convert the model to FP16 GGUF format
3. Quantize to your selected method
4. Save the GGUF file to Google Drive or local storage
5. Upload to Hugging Face Hub (if enabled and write token provided)
6. Show a final report with file size and location

### Step 5: Use Your Model
Once complete, you can:
- Download the GGUF files from Google Drive
- Use with llama.cpp for LLM models: `./llama.cpp/llama-cli -m model.gguf -p "Your prompt"`
- Use with whisper.cpp for Whisper models: `./whisper.cpp/build/bin/whisper-cli -m model.gguf -f audio.wav`
- Share on Hugging Face Hub if uploaded

---

## 🛠️ Requirements

- **Google Account** - To use Google Colab and Google Drive
- **Colab Runtime** - T4 GPU recommended (free tier works)
- **Storage Space** - At least 2x the model size for intermediate files
- **Internet Connection** - For downloading models and dependencies
- **Hugging Face Token** (optional) - For private models or uploading

---

## 📝 Examples

### LLM Model Quantization

```python
# Model URL
model_url = "mistralai/Mistral-7B-Instruct-v0.1"

# Model Type
model_type = "LLM (Text Models)"

# Quantization
quantization_method = "Q4_K_M"

# Output
output_folder = "GGUF_Models"
