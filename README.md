# Local LLM + Stable Diffusion Image Generator

This project connects a **local LLM (via Ollama)** with **Stable Diffusion (via sd.cpp)**  
to create an entirely **offline text-to-image generator**.

The LLM (e.g. `gemma3:4b`) generates a detailed text prompt,  
and Stable Diffusion uses that prompt to produce the image.  All running locally on your machine.

---

## Build Instructions

```bash
# Clone and enter the repository
git clone https://github.com/yourusername/local-llm-image-generator.git
cd local-llm-image-generator

# Install build dependencies
sudo apt update
sudo apt install -y git build-essential cmake

# Build stable-diffusion.cpp (with CUDA if available)
cd stable-diffusion.cpp
mkdir build && cd build
cmake .. -DSD_CUDA=ON
make -j$(nproc)
````

Place your Stable Diffusion model (e.g. `sd-v1-4.ckpt` or `v1-5-pruned-emaonly.safetensors`)
inside the `models/` folder:

```
stable-diffusion.cpp/models/
```

---

## Run

1. Generate a text prompt with the local LLM (Gemma 3)

```bash
PROMPT="$(ollama run gemma3:4b 'Write one detailed Stable Diffusion prompt about an underwater robot inspecting a pipeline at dusk, cinematic lighting, realistic textures.')"
```

2. Generate an image with Stable Diffusion

```bash
cd stable-diffusion.cpp/build
./bin/sd -m ../models/sd-v1-4.ckpt -p "$PROMPT" \
  --steps 25 --cfg-scale 7.5 -W 768 -H 512 -o rov.png
```

The image will be saved as `rov.png` in the `build` folder.

---


