# VampNet Masking Experiments

This folder contains a series of Jupyter notebooks that systematically explore different masking strategies for VampNet, a masked acoustic token modeling system for music generation. These experiments investigate how various masking parameters affect the generation quality and characteristics.

## Overview

VampNet uses masking to control which parts of the audio are regenerated. The masking strategy significantly impacts:
- How much of the original audio is preserved vs. regenerated
- The coherence and musicality of the output
- The balance between creativity and fidelity
- The types of transformations applied to the audio

## Notebooks

### 1. [01_sparse_vs_dense_masking.ipynb](01_sparse_vs_dense_masking.ipynb)
**Tests mask density from 10% to 95%**

**Key Findings:**
- **10-30% density**: Minimal changes, preserves most original structure
- **40-60% density**: Balanced transformation, good for variations
- **70-90% density**: Major transformations, more creative but less coherent
- **95% density**: Near-complete regeneration, loses most original character

**Optimal Range:** 40-70% for musical variations that maintain coherence

### 2. [02_random_vs_regular_masking.ipynb](02_random_vs_regular_masking.ipynb)
**Compares random, periodic, block, and strided masking patterns**

**Key Findings:**
- **Random masking**: Natural-sounding variations, good general-purpose
- **Periodic masking**: Creates rhythmic patterns, useful for beat-aligned edits
- **Block masking**: Replaces entire sections, good for transitions
- **Strided masking**: Regular patterns, creates interesting textures

**Best Use Cases:**
- Random: General variations and remixing
- Periodic: Rhythm-aware transformations
- Block: Section replacement, mashups
- Strided: Texture and pattern generation

### 3. [03_periodic_prompt_intervals.ipynb](03_periodic_prompt_intervals.ipynb)
**Tests periodic prompt intervals from 2 to 100**

**Key Findings:**
- **Intervals 2-3**: Very dense preservation, minimal changes
- **Intervals 5-7**: Optimal for musical edits, preserves rhythm
- **Intervals 10-20**: Good for structural variations
- **Intervals 30-50**: Major transformations while maintaining some anchors
- **Intervals 70-100**: Sparse anchoring, very creative outputs

**Recommended:** Interval of 7 for most musical applications

### 4. [04_codebook_masking_depth.ipynb](04_codebook_masking_depth.ipynb)
**Explores masking different numbers of codebooks (0-4)**

**Key Findings:**
- **0 codebooks**: Only fine details masked, subtle variations
- **1-2 codebooks**: Low to mid-level features, timbral changes
- **3 codebooks**: High-level structure, melodic variations
- **4 codebooks**: Complete coarse structure, major transformations

**Hierarchy:** Lower codebooks = coarse features, Higher = fine details

### 5. [05_masking_summary.ipynb](05_masking_summary.ipynb)
**Synthesizes findings and demonstrates practical applications**

**Key Applications:**
1. **Subtle Variations**: Low density (30%), few codebooks
2. **Musical Remixing**: Medium density (50%), periodic masking
3. **Creative Generation**: High density (70%), all codebooks
4. **Texture Morphing**: Block masking with specific codebooks
5. **Rhythmic Variations**: Periodic masking aligned to tempo

## Key Parameters

### Core Parameters
- **mask_ratio**: Percentage of tokens to mask (0.0-1.0)
- **periodic_prompt**: Interval for periodic masking (typically 7)
- **upper_codebook_mask**: Number of lower codebooks to mask (0-14)

### Pattern Types
1. **Random**: Randomly distributed masks
2. **Periodic**: Regular intervals (every Nth token)
3. **Block**: Contiguous regions
4. **Strided**: Regular pattern with offset

## Practical Guidelines

### For Different Use Cases

**Subtle Variations:**
```python
mask_ratio = 0.3
periodic_prompt = 7
upper_codebook_mask = 2
```

**Balanced Remixing:**
```python
mask_ratio = 0.5
periodic_prompt = 7
upper_codebook_mask = 3
```

**Creative Transformation:**
```python
mask_ratio = 0.7
periodic_prompt = 10
upper_codebook_mask = 4
```

**Rhythm-Preserving Edit:**
```python
# Use periodic masking with interval matching musical bars
periodic_prompt = 7  # or tempo-derived value
upper_codebook_mask = 3
```

## Technical Notes

- VampNet uses 14 codebooks total (4 coarse + 10 fine)
- Token rate is approximately 57.27 tokens/second at 44.1kHz
- Each token represents ~17.4ms of audio
- Masking operates on discrete token positions

## Output Examples

The `outputs/` folder contains audio examples from each experiment:
- Original recordings for comparison
- Generated variations with different mask settings
- Visualizations of mask patterns and their effects

## Recommendations

1. **Start with defaults**: periodic_prompt=7, mask_ratio=0.5, upper_codebook_mask=3
2. **Adjust based on goal**: Lower values for subtle edits, higher for creative generation
3. **Consider audio content**: Rhythmic music benefits from periodic masking
4. **Test incrementally**: Small parameter changes can have significant effects
5. **Combine strategies**: Mix different mask patterns for complex transformations

## Future Experiments

Potential areas for further investigation:
- Tempo-aware masking aligned to musical beats
- Content-adaptive masking based on audio features
- Progressive masking strategies over multiple passes
- Combining multiple mask patterns in single generation
- Fine-tuning models for specific masking strategies