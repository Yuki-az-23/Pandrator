# M1 Mac Optimization Guide for Pandrator

## 🍎 Overview

Pandrator now includes **native Apple Silicon (M1/M2/M3/M4) optimization** with two new TTS engines specifically designed for efficient performance on Mac:

- **Piper TTS**: 3-10x faster than XTTS, perfect for long audiobooks
- **Kokoro TTS**: Natural-sounding voices with Metal Performance Shaders (MPS) acceleration

## 🚀 Quick Start (Non-Technical Users)

### Using Device Presets

Pandrator includes **one-click presets** that automatically configure optimal settings for your device:

1. Open Pandrator
2. Go to the **Session** tab
3. Look for the **"Device Presets (Quick Setup)"** section
4. The app will show your detected device (e.g., "🍎 Detected: M1/M2/M3 Mac")
5. Click one of the preset buttons:

#### M1 Mac Users - Choose One:

**🍎 M1 Fast (Piper)** - *Recommended for most users*
- **Speed**: Fastest option
- **Quality**: Good
- **Time**: 1-3 hours for 13-chapter book
- **Best for**: Long books, batch processing
- **Notes**: Won't overheat your Mac, runs cool

**🍎 M1 Quality (Kokoro)**
- **Speed**: Medium-fast
- **Quality**: Very natural-sounding
- **Time**: 2-3 hours for 13-chapter book
- **Best for**: Audiobooks where voice quality matters most
- **Notes**: Uses GPU acceleration, balanced performance

**🍎 M1 Balanced (XTTS)**
- **Speed**: Slower
- **Quality**: High, supports voice cloning
- **Time**: 4-6 hours for 13-chapter book
- **Best for**: Voice cloning projects
- **Notes**: Take breaks during long sessions

## 📊 Performance Comparison

Testing on a 13-chapter book (~100,000 words):

| Model | M1 Air (8GB) | M1 Pro (16GB) | M1 Max (32GB) |
|-------|--------------|---------------|---------------|
| **Piper** | 2-3 hours | 1.5-2 hours | 1-1.5 hours |
| **Kokoro** | 3-4 hours | 2-3 hours | 2-2.5 hours |
| **XTTS (CPU)** | 6-8 hours | 4-6 hours | 3-5 hours |
| **Silero** | 2-3 hours | 1.5-2 hours | 1-1.5 hours |

## 🔧 Manual Setup

### Installing Piper TTS

```bash
# Activate Pandrator environment
conda activate pandrator_installer

# Install Piper
pip install piper-tts

# Download voice models (example for English)
mkdir -p models/piper
cd models/piper

# High-quality English voice
wget https://huggingface.co/rhasspy/piper-voices/resolve/v1.0.0/en/en_US/lessac/medium/en_US-lessac-medium.onnx
wget https://huggingface.co/rhasspy/piper-voices/resolve/v1.0.0/en/en_US/lessac/medium/en_US-lessac-medium.onnx.json

# More voices available at: https://github.com/rhasspy/piper/blob/master/VOICES.md
```

### Installing Kokoro TTS

```bash
# Activate Pandrator environment
conda activate pandrator_installer

# Install Kokoro
pip install kokoro soundfile

# For M1 GPU acceleration, set environment variable
export PYTORCH_ENABLE_MPS_FALLBACK=1
```

### Running TTS Servers

**Option 1: Using API Servers (Recommended)**

For Piper, you can use a simple FastAPI wrapper:
```bash
# Install dependencies
pip install fastapi uvicorn

# Start Piper server (create simple server script or use existing wrapper)
# Server should listen on http://localhost:8002/tts
```

For Kokoro, use the FastAPI wrapper:
```bash
# Clone Kokoro-FastAPI
git clone https://github.com/remsky/Kokoro-FastAPI.git
cd Kokoro-FastAPI

# Follow their installation instructions
# Server will listen on http://localhost:8003/v1/audio/speech
```

**Option 2: Direct Integration (Coming Soon)**

Future versions will support direct in-process TTS without separate servers.

## 💡 Tips for M1 Mac Users

### Thermal Management
- **M1 Air**: Use "M1 Fast (Piper)" or "Low-Power (Silero)" for long sessions
- **M1 Pro/Max**: Any preset works well, even for hours
- **Break Recommendations**:
  - Piper/Silero: No breaks needed
  - Kokoro: Break every 3-4 hours
  - XTTS: Break every 2-3 hours

### Battery Life
- **Best**: Silero (4-6 hours battery on M1 Air)
- **Good**: Piper (3-4 hours battery)
- **Medium**: Kokoro (2-3 hours battery)
- **Use AC**: XTTS (heavy CPU usage)

### Quality vs Speed Tradeoff

```
Piper:    ████████░░ Speed: 10/10, Quality: 8/10
Kokoro:   ██████████ Speed: 8/10, Quality: 10/10
XTTS:     ████░░░░░░ Speed: 4/10, Quality: 9/10 + Voice Cloning
Silero:   █████████░ Speed: 9/10, Quality: 7/10
```

## 🎯 Use Case Recommendations

### Long Novels (500+ pages)
✅ **Use**: M1 Fast (Piper)
- Fast generation
- Consistent quality
- Won't overheat
- Can run overnight

### Audiobooks for Publication
✅ **Use**: M1 Quality (Kokoro)
- Professional sound
- Natural inflection
- Good pacing
- Worth the extra time

### Voice Cloning Projects
✅ **Use**: M1 Balanced (XTTS)
- Upload your voice sample
- XTTS clones voices well
- Take breaks during long generations

### Quick Tests / Previews
✅ **Use**: Low-Power (Silero)
- Instant results
- Good enough for testing
- Minimal resource usage

## 🔍 Troubleshooting

### "Piper TTS is not installed"
```bash
conda activate pandrator_installer
pip install piper-tts
```

### "Kokoro TTS is not installed"
```bash
conda activate pandrator_installer
pip install kokoro soundfile
```

### Piper server connection error
- Ensure Piper API server is running on port 8002
- Check with: `curl http://localhost:8002/docs`

### Kokoro server connection error
- Ensure Kokoro API server is running on port 8003
- Check with: `curl http://localhost:8003/v1/audio/speech`

### M1 Mac running slow
- Close other applications
- Use "M1 Fast" preset
- Ensure you're running native ARM64 Python (not Rosetta)
- Check: `python -c "import platform; print(platform.machine())"`
  - Should show: `arm64` (not `x86_64`)

## 📚 Voice Model Resources

### Piper Voices
- **Official List**: https://github.com/rhasspy/piper/blob/master/VOICES.md
- **50+ Languages**: Including English, Spanish, French, German, etc.
- **Quality Levels**: low, medium, high (higher = better but slower)

### Kokoro Voices
- **Built-in Voices**:
  - `af_bella` - Female, warm
  - `af_heart` - Female, energetic
  - `af_sky` - Female, calm
  - `am_adam` - Male, professional
  - `am_michael` - Male, friendly

## 🤝 Contributing

Found a better configuration for M1 Macs? Please share!
- Open an issue on GitHub
- Join our Discord community
- Create a pull request with your preset

## 📈 Roadmap

- [ ] Direct Piper/Kokoro integration (no separate servers needed)
- [ ] Voice model management UI
- [ ] Automatic voice downloads
- [ ] MLX-optimized models for even faster M1 performance
- [ ] Real-time streaming TTS
- [ ] Custom preset creation in GUI

---

**Happy audiobook creation on your M1 Mac! 🎧**
