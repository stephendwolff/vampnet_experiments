# VampNet Experiments

This repository contains experiments and tutorials for understanding VampNet, a masked acoustic token modeling approach for music generation.

## Overview

VampNet is a neural audio codec model that uses masked token modeling to generate, compress, and transform music. This project breaks down VampNet's operation into understandable components, showing both high-level usage and low-level implementation details.

## Contents

- `vampnet_tutorial.ipynb` - Main tutorial notebook demonstrating:
  - High-level VampNet interface usage
  - Step-by-step breakdown of the generation pipeline
  - Low-level implementation matching the high-level interface
  - Visualization of tokens, masks, and generation process

- `assets/` - Audio files for testing:
  - `stargazing.wav` - Example input audio
  - `example.wav` - Additional test audio
  - `vampnet.png` - VampNet architecture diagram

## Requirements

See `requirements.txt` for dependencies. Key packages include:
- `vampnet` - The VampNet model implementation
- `audiotools` - Audio processing utilities
- `torch` - PyTorch for model inference
- `matplotlib` - For visualizations
- `ipython` - For notebook audio playback

## Setup

1. Create a virtual environment:
   ```bash
   python -m venv venv
   source venv/bin/activate  # On Windows: venv\Scripts\activate
   ```

2. Install dependencies:
   ```bash
   pip install -r requirements.txt
   ```

3. Run the notebook:
   ```bash
   jupyter notebook vampnet_tutorial.ipynb
   ```

## Key Concepts

### Masked Token Modeling
VampNet operates on discrete audio tokens obtained from a neural codec. It uses masking strategies to:
- Preserve periodic prompts (e.g., every 13th token)
- Mask upper codebooks while preserving lower ones
- Generate new tokens in masked positions

### Generation Pipeline
1. **Preprocessing**: Normalize audio to -24 dBFS
2. **Encoding**: Convert audio to discrete tokens using neural codec
3. **Masking**: Apply strategic masking patterns
4. **Coarse Generation**: Generate coarse tokens with transformer
5. **Fine Generation**: Refine with coarse-to-fine model
6. **Decoding**: Convert tokens back to audio

### Parameters
- `periodic_prompt`: Interval for keeping unmasked tokens (e.g., 13)
- `upper_codebook_mask`: Number of lower codebooks to preserve
- `temperature`: Sampling temperature for generation
- `typical_filtering`: Whether to use typical sampling

## References

- Paper: [VAMPNET: Music generation via masked acoustic token modeling (García et al., 2023)](https://arxiv.org/abs/2307.04686)
- Original implementation: [https://github.com/hugofloresgarcia/vampnet](https://github.com/hugofloresgarcia/vampnet)