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

**Model Information:**
- **Model URL**: Paste your Hugging Face model URL or ID
  - Example: `mistralai/Mistral-7B-Instruct-v0.1`
  - Example: `openai/whisper-large-v3`
  - Supports full URLs like `https://huggingface.co/google/medgemma-4b-it`
- **Model Type**: Select between "LLM (Text Models)" or "Whisper (Audio Models)"

**Hugging Face Tokens (Optional):**
- **Read Token**: Required for downloading gated/private models (starts with `hf_`)
- **Write Token**: Required for uploading quantized models to Hugging Face Hub
- *Leave empty for public models or if not uploading*

**Quantization Selection:**
- Choose one quantization method from 13 options
- See the guide above for quality/size trade-offs

**Storage Settings:**
- **Mount Google Drive**: Save models to your Google Drive (recommended)
- **Output Folder**: Folder name in Google Drive (default: "GGUF_Models")
- **Skip Existing**: Skip if quantization already exists

**Upload Settings (Optional):**
- **Upload to Hub**: Enable auto-upload to Hugging Face
- **Your HF Username**: Your Hugging Face username
- **Private Repo**: Make the uploaded repository private

### Step 4: Run Quantization
Execute the quantization cell. The notebook will automatically:
1. Download the model from Hugging Face (using read token if provided)
2. Convert the model to FP16 GGUF format
3. Quantize to your selected method
4. Save the GGUF file to Google Drive or local storage
5. Upload to Hugging Face Hub (if enabled and write token provided)
6. Show a final report with file size and location

📝 Examples

LLM Model Quantization
python

model_url = "mistralai/Mistral-7B-Instruct-v0.1"
model_type = "LLM (Text Models)"
quantization_method = "Q4_K_M"
output_folder = "GGUF_Models"
upload_to_hub = False

Whisper Model Quantization
python

model_url = "openai/whisper-large-v3"
model_type = "Whisper (Audio Models)"
quantization_method = "Q4_K_M"
mount_drive = True

Private Model with Upload
python

model_url = "meta-llama/Llama-2-7b-hf"
read_token = "hf_xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"
write_token = "hf_yyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyy"
upload_to_hub = True
your_hf_username = "your-username"
private_repo = False

🛠️ Requirements

    Google Account - To use Google Colab and Google Drive
    Colab Runtime - T4 GPU recommended (free tier works)
    Storage Space - At least 2x the model size for intermediate files
    Internet Connection - For downloading models and dependencies
    Hugging Face Token (optional) - For private models or uploading

🔧 Troubleshooting

"Model not found" error
    Ensure the model URL is correct (format: username/model-name)
    For private models, provide a valid read token
    Check if the model exists on Hugging Face Hub

"Token invalid" error
    Generate a new token at https://huggingface.co/settings/tokens
    Ensure read token has "read" permissions
    Ensure write token has "write" permissions for uploads
    Tokens should start with hf_

"Out of memory" error
    Use a smaller quantization method (Q3_K_M or Q2_K)
    Restart Colab runtime with T4 GPU
    Try a smaller model variant
    Free up RAM by restarting runtime

"Conversion failed" error
    Ensure the model is compatible with llama.cpp/whisper.cpp
    Check if the model uses supported architecture
    Try using FP16 first before quantizing
    For Whisper, ensure model is in the correct format

"401 Unauthorized" during download
    You're trying to download a private/gated model
    Provide a valid read token with access to the model
    Request access to gated models on Hugging Face first

"Upload failed" error
    Provide a valid write token with write permissions
    Ensure your Hugging Face username is correct
    Check if repository name already exists (auto-creates if not)

📂 Output Structure

After quantization, your files will be organized as:
text

Google Drive/GGUF_Models/
├── model-name-Q4_K_M.gguf          # Quantized model
├── quantization_info.json           # Configuration metadata
└── model_cache/                     # Cached downloads (if not cleaned)
    └── username_model-name/

If you enable upload to Hub, the model will be available at:
text

https://huggingface.co/your-username/model-name-Q4_K_M-GGUF

💡 Tips & Best Practices

    First time users: Start with a small public model to test the workflow
    Storage management: Models are saved to Google Drive - you can download and delete after use
    Caching: Downloaded models are cached to save time on subsequent runs
    Multiple quantizations: Run the notebook multiple times with different quantization methods
    Token security: Never share your tokens; they are only stored in memory during runtime
    Resume capability: If interrupted, re-run the quantization cell - it will resume from where it left off
    Disk space: Monitor disk usage with Colab's built-in storage indicator

📚 Additional Resources

    llama.cpp GitHub - The core library for LLM inference
    whisper.cpp GitHub - The core library for Whisper inference
    GGUF Format Documentation - Technical details of GGUF
    Hugging Face Hub Documentation - Model hosting and sharing
    llama.cpp Quantization Guide - Official quantization docs

🤝 Contributing

Contributions are welcome! Feel free to:
    Open issues for bugs or feature requests
    Submit pull requests to improve the notebook
    Share your quantization results and experiences
    Fork the repository for your own customizations

📄 License

This project is licensed under the Apache 2.0 License - see the LICENSE file for details.
text

Copyright 2024 GGUF Model Quantizer Contributors

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.

🙏 Acknowledgements

    ggerganov - For creating and maintaining llama.cpp and whisper.cpp
    Hugging Face - For model hosting, Hub integration, and the transformers library
    Google Colab - For providing free GPU resources
    PyTorch - For the deep learning framework
    OpenAI - For creating the Whisper model
    Meta - For creating the LLaMA model family
    All open-source contributors - Who make AI accessible to everyone

📊 Performance Notes

    T4 GPU (Colab Free): Can handle models up to 7-13B parameters with Q4_K_M quantization
    V100/A100 (Colab Pro): Can handle larger models (13-34B parameters)
    CPU-only: Works but significantly slower; GPU recommended
    Whisper models: All sizes (tiny to large-v3) work well on T4 GPU

⚠️ Important Notes

    The notebook downloads and processes models - ensure you have sufficient disk space (at least 2x model size)
    Quantization is computationally intensive; expect 10-30 minutes depending on model size
    For very large models (30B+ parameters), you may need Colab Pro or run on local hardware
    Some models may require additional permissions or have usage restrictions
    Always respect the model's license and terms of use

Built with ❤️ for the open-source community

Star ⭐ this repository if you find it useful!
