# 🖼️ Laptop Image‑Generation Node (MSI Raider GE66 + Pop!_OS)

This repo documents how I turned my secondary laptop into a dedicated **ComfyUI image generation node**, offloading GPU strain from my main PC to run Stable Diffusion workflows more efficiently.

Rather than let my laptop collect dust, I made it pull its weight — headless, silent, and focused entirely on one task: **rendering high-quality AI images remotely**.

---

## ⚙️ System Architecture

| Node        | CPU           | GPU                         | RAM / Drives                  | OS           | Primary Role                           |
|-------------|----------------|------------------------------|-------------------------------|--------------|----------------------------------------|
| **Laptop**  | i7‑12700H     | RTX 3070 Ti (8 GB)           | 16 GB DDR5‑4800 / 1 TB NVMe   | Pop!_OS 22.04 | ComfyUI + Secondary LLM Inference      |
| **Main PC** | i7‑14700KF    | RTX 3070 (OC +180 / +675 Mem) | 64 GB DDR4 + 4TB SSD/HDD mix | Win 11 Pro    | LLM Core Orchestrator + Remote Access  |

🧠 **Access Method**: Laptop runs ComfyUI and exposes its canvas at `http://192.168.X.X:8188`, accessed via browser on main PC.

🖥️ **Monitoring Tools**: `nvtop`, `nvidia-smi`, and on-desk thermal tools to ensure safe usage.

---

## 💡 Why This Setup?

My main PC runs dozens of containers (LLMs, tools, workflows).  
Stable Diffusion + ComfyUI is GPU-intensive and would interrupt high-VRAM models or slow down inference elsewhere.

The **solution**: repurpose the laptop as a **dedicated rendering node**.
- No display output needed
- Minimal Linux overhead
- Efficient and silent image generation
- Clean pipeline: *prompt on PC → render on laptop → view output via LAN*

---

## 🔧 Clean ComfyUI Install on Linux (Pop!_OS 22.04)

```bash
# 1. Install dependencies
sudo apt install git python3-venv python-is-python3

# 2. Set up environment
mkdir ~/comfy && cd ~/comfy
python -m venv comfy-env
source comfy-env/bin/activate
pip install --upgrade pip

# 3. Install comfy-cli wrapper
pip install comfy-cli
comfy install               # clones ComfyUI + installs dependencies

# 4. First-time test
cd ComfyUI
python main.py --listen 0.0.0.0 --port 8188
