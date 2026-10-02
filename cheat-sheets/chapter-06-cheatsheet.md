# Image and Video Generation Cheat Sheet

## Generation Parameters

| Parameter | Purpose | Exam-Critical Values |
|-----------|---------|----------------------|
| **Seed** | Controls randomness | Same seed + same inputs = identical output |
| **Resolution** | Output dimensions | Common: 1024×1024, 1792×1024, 1024×1792 |
| **Aspect Ratio** | Image shape | 1:1 (square), 16:9 (landscape), 9:16 (portrait) |
| **Strength** (img2img) | Deviation from input | 0.3-0.5 = variations, 0.7-0.9 = major changes |
| **Style Presets** | Visual style control | Photorealistic, anime, oil painting, etc. |

## Image Editing Operations

| Operation | Use Case | How It Works |
|-----------|----------|-------------|
| **Inpainting** | Replace region in image | Mask defines pixels to regenerate, preserves rest |
| **Outpainting** | Extend canvas borders | Adds content beyond current image edges |
| **Image-to-Image** | Generate variations | Input image + prompt → modified version |
| **Prompt-Driven Edit** | Text-based modifications | Describe changes in natural language |

## Content Safety Filters

| Category | Detects | Exam Trap |
|----------|---------|----------|
| **Violence** | Graphic, gory content | Must enable ALL categories for comprehensive protection |
| **Sexual** | Adult content | - |
| **Hate** | Hate symbols, slurs | - |
| **Self-Harm** | Self-injury content | - |
| **Prohibited Symbols** | Copyrighted logos, brands | Separate from content safety - IP protection |

**Sensitivity Levels**: Low, Medium, High  
**Timing**: Pre-generation prompt analysis (blocks before API call completes)

## Responsible AI Features

| Feature | Purpose | Persistence |
|---------|---------|------------|
| **Embedded Watermark** | Mark as AI-generated | Survives screenshots, cropping, compression |
| **Visible Watermark** | Branding overlay | Easily removed - not for compliance |
| **Metadata Tags** | EXIF data | Lost on screenshot/re-upload |
| **Content Filters** | Block unsafe generation | Pre-generation blocking |

## Video Generation Workflow

```
Text-to-Video:
1. Text prompt → video frames
2. Optional: Reference image conditioning (for style consistency)
3. Output: MP4/video format

Image-to-Video:
1. Input image + motion prompt
2. Generate video from static image
3. Control: duration, motion type
```

## Multimodal Input Combinations

| Input Types | Use Case | Example |
|-------------|----------|----------|
| **Text only** | From scratch | "A sunset over mountains" |
| **Text + Reference Image** | Style transfer | Brand image + "product photo in this style" |
| **Image + Mask + Text** | Inpainting | Photo + background mask + "tropical beach" |
| **Image + Text** | Variations | Design + "change material to glass" |

## Common Exam Scenarios

**Reproducibility**: Use seed value (not temperature - wrong parameter)  
**Background Replacement**: Inpainting with background mask  
**Canvas Extension**: Outpainting (not inpainting)  
**Brand Logo Prevention**: Prohibited-symbol detection (not negative prompts alone)  
**Regulatory Compliance**: Embedded watermarking (not metadata)  
**Filter Bypass Prevention**: Enable ALL content categories at high sensitivity  
**Controlled Variations**: Image-to-image with strength 0.3-0.5  
**Style Consistency**: Reference image conditioning (multimodal approach)  

## Models in Azure AI Foundry

- **DALL-E**: Via Azure OpenAI integration
- **Stable Diffusion**: Multiple variants in Foundry catalog
- **Text-to-Video Models**: Catalog deployments (not custom training)

**Key Distinction**: Foundry = deploy/inference pre-trained models, NOT custom model training platform

## Negative Prompts vs Content Filters

| Aspect | Negative Prompts | Content Filters |
|--------|------------------|------------------|
| Purpose | Guide generation away from concepts | Hard block unsafe content |
| Reliability | Reduces likelihood, not guaranteed | Guaranteed blocking |
| Use Case | Quality control ("blurry, low quality") | Safety/compliance |
| Exam Role | Guidance mechanism | Security control |
