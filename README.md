# SDXL checkpoint converter

This repository will try to provide a nice way to convert SDXL checkpoints to diffusers format.

Clone the repository and its kohya_ss submodule:

```bash
git clone --recursive https://github.com/waifu-diffusion/sdxl-ckpt-converter.git
cd sdxl-ckpt-converter
```

Make & activate a new conda environment if you feel like it:
```bash
conda create -n p311-sdxl-ckpt python=3.11
conda activate p311-sdxl-ckpt
```

Install dependencies:

```bash
pip install -r requirements.txt
```

## Converting checkpoints

Here we will load an OG SDXL checkpoint and output it into a directory.

We load `/mnt/wd-dataset/wdxl-dist/wdxl-step00006000.safetensors`,
and output a Diffusers distribution into `/mnt/wd-dataset/wdxl-dist-diffusers/wdxl-step00006000`.

Run (from the root of this repository):

```bash
mkdir -p /mnt/wd-dataset/wdxl-dist-diffusers/wdxl-step00006000
PYTHONPATH="lib/kohya_ss:$PYTHONPATH" python -m script.convert_sdxl_og_ckpt_to_diffusers \
--fp16 \
--save_variant fp16 \
--use_safetensors \
--reference_model stabilityai/stable-diffusion-xl-base-1.0 \
/mnt/wd-dataset/wdxl-dist/wdxl-step00006000.safetensors \
/mnt/wd-dataset/wdxl-dist-diffusers/wdxl-step00006000
```

## Verifying a converted checkpoint

Install inference dependencies:

```bash
pip install -r lib/sdxl-play/requirements_diffusers.txt
# can't specify --no-deps in a requirements file, so we have to do this part separately
pip install invisible-watermark --no-deps
# install flash attention
MAX_JOBS=4 pip install 'flash-attn>=2.1.0' --no-build-isolation
```

Make waifu (fixed prompt, varied seeds):

```bash
cd lib/sdxl-play
python -m scripts.sdxl_kdiff_play \
--base_unet /mnt/wd-dataset/wdxl-dist-diffusers/wdxl-step00006000
```

Make waifu (fixed seed, varied prompts):

```bash
cd lib/sdxl-play
python -m scripts.sdxl_kdiff_latent_walk \
--base_unet /mnt/wd-dataset/wdxl-dist-diffusers/wdxl-step00006000
```