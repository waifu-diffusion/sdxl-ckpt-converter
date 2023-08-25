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

## Usage

```bash
python -m script.convert_sdxl_og_ckpt_to_diffusers
```