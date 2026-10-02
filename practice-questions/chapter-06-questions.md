## Chapter 6: Image and Video Generation Solutions — Practice Questions

**Q1.** A company is building an application to generate product images from text descriptions. They need to ensure consistency across multiple generations for A/B testing. The generated images must be exactly reproducible when using the same prompt. Which parameter BEST achieves this requirement?

A) Set the temperature parameter to 0.0 to disable randomness
B) Enable deterministic_mode in the API request configuration
C) Use a fixed seed value with identical prompt and generation parameters
D) Cache the first generation result and return it for subsequent requests

<details>
<summary>Answer</summary>

**Correct: C**

Seed values control the random number generator in image generation models. Using the same seed with identical prompt, resolution, and other parameters produces bit-for-bit identical images. This is the standard reproducibility mechanism in DALL-E and Stable Diffusion.

**Why the others are wrong:**
- Temperature is a text generation parameter, not used in image generation models. Image models use seed for randomness control.
- There is no deterministic_mode parameter in Azure AI Vision or Foundry image generation APIs. Determinism is achieved through seed values.
- Caching doesn't address the requirement - they need to regenerate images reproducibly for testing, not return cached results. Also violates the spirit of A/B testing with fresh generations.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q2.** A marketing team uses Azure AI to generate advertisement images. They discover that some generations include brand logos that the company doesn't have rights to use. Which Azure AI feature should be implemented to prevent this issue?

A) Add brand names to the negative prompt parameter
B) Set the content_filter_level to 'strict' mode
C) Use custom vision training to detect and reject brand logos post-generation
D) Enable the prohibited-symbol detection filter in content safety settings

<details>
<summary>Answer</summary>

**Correct: D**

Azure AI Vision's content safety includes prohibited-symbol detection specifically designed to identify and block generation of copyrighted logos, trademarks, and brand symbols. This is a pre-generation filter that prevents the issue rather than detecting it after generation.

**Why the others are wrong:**
- Negative prompts reduce likelihood but don't guarantee prevention - the model may still generate brand elements from visual associations. Negative prompts are guidance, not hard filters.
- Content filter levels (low/medium/high) control violence, sexual content, hate, and self-harm categories, not brand/trademark detection. Different filter system.
- Post-generation detection is inefficient and still requires wasted API calls. Also requires training custom models for every brand. Prohibited-symbol detection is built-in and preventive.

</details>

---

**Q3.** An e-commerce platform needs to modify existing product photos by replacing backgrounds while keeping the product unchanged. The system must process 10,000 images per day. Which Azure AI approach BEST meets this requirement?

A) Use DALL-E outpainting to extend the canvas with new background
B) Use inpainting with a mask covering the background region only
C) Generate new images with the product description and manually composite
D) Use Azure Computer Vision segmentation API followed by background replacement

<details>
<summary>Answer</summary>

**Correct: B**

Inpainting with background masking is the exact use case - you provide the original image and a mask defining which pixels to regenerate (background), while preserving unmasked regions (product). This is efficient, purpose-built, and handles scale via API batching.

**Why the others are wrong:**
- Outpainting extends beyond current borders to add new content around edges. It doesn't replace existing background within the image bounds - wrong operation for the scenario.
- Regenerating entire images doesn't preserve the exact product appearance/lighting from original photos. Manual compositing doesn't scale to 10,000 images/day and defeats the purpose of using generation AI.
- Segmentation + replacement requires two API calls plus custom compositing logic. Inpainting does this in one operation with better context-aware generation. Less efficient and more complex.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q4.** A video production company is using Azure AI Foundry to generate marketing videos from text scripts. They want to ensure the generated video matches their brand's visual style consistently. Which combination provides the MOST control over visual consistency?

A) Use reference image conditioning with brand style guide images plus detailed style descriptors in the prompt
B) Set high seed values and enable style_lock parameter in generation config
C) Generate multiple videos and use ensemble averaging to blend outputs
D) Train a custom Stable Diffusion model on brand assets then deploy to Foundry

<details>
<summary>Answer</summary>

**Correct: A**

Reference image conditioning allows feeding example images that define the target visual style, while text prompts provide scene-specific instructions. This multimodal approach (image + text input) gives maximum control and is supported in Foundry's text-to-video models.

**Why the others are wrong:**
- There is no style_lock parameter in Azure AI Foundry video generation APIs. Seed controls randomness but doesn't define style - two different concepts.
- Ensemble averaging produces blurred, incoherent results with video. Video frames must be temporally consistent - averaging destroys this. Not a valid video generation technique.
- Custom model training is expensive, requires ML expertise, and Foundry's model deployment doesn't support custom video generation models (only inference of catalog models). Reference conditioning is the built-in solution.

</details>

---

**Q5.** A healthcare application generates medical illustration images for patient education. Regulations require all AI-generated images to be identifiable as synthetic. What is the MOST reliable Azure AI mechanism to meet this requirement?

A) Add visible watermark text in the corner of each generated image
B) Include metadata in the image EXIF tags marking it as AI-generated
C) Enable embedded watermarking in the generation API request
D) Generate images at lower resolution to distinguish from real photos

<details>
<summary>Answer</summary>

**Correct: C**

Azure AI's embedded watermarking uses imperceptible patterns encoded in the pixel data that survive image transformations (cropping, compression, screenshots). This is cryptographically robust and meets regulatory requirements for synthetic media disclosure.

**Why the others are wrong:**
- Visible watermarks can be cropped out or covered. Not reliable for regulatory compliance where users might intentionally remove identifiers.
- EXIF metadata is trivially stripped - any image editor or screenshot removes it. Not suitable for regulatory requirement of persistent identification.
- Resolution is not a reliable indicator - real photos can be low resolution, AI can generate high resolution. This doesn't meet regulatory requirement of marking content as synthetic.

</details>

---

**Q6.** A digital art platform needs to generate images while preventing violent or disturbing content. Users sometimes intentionally craft prompts to bypass filters. The platform must reject these attempts before generation. Which configuration provides the STRONGEST protection?

A) Set content safety filters to high sensitivity for violence category only
B) Use negative prompts to list prohibited content types
C) Analyze generated images post-generation and delete violations
D) Enable all content safety categories (violence, sexual, hate, self-harm) at high sensitivity with prompt analysis

<details>
<summary>Answer</summary>

**Correct: D**

Azure AI content safety for image generation includes pre-generation prompt analysis across all categories. High sensitivity on all categories catches attempts to generate problematic content including adversarial prompts. This blocks API calls before wasting resources on generation.

**Why the others are wrong:**
- Violence-only filtering misses sexual, hate, or self-harm content. The question requires preventing disturbing content broadly - need all categories enabled.
- Negative prompts are user guidance, not security controls. Sophisticated users can circumvent through indirect phrasing or visual associations. Not a protection mechanism.
- Post-generation analysis wastes API quota on content that will be rejected, and there's a window where inappropriate content exists in system before deletion. Pre-generation blocking is required.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q7.** An architectural firm is generating building concept images. They need to generate variations of a design while maintaining the same structural layout but changing materials and lighting. Which approach BEST achieves this?

A) Use the same seed value with modified prompts describing different materials
B) Generate completely new images and use inpainting to fix structural elements
C) Use outpainting to extend the building design with new material specifications
D) Use image-to-image generation with the base design as input and strength parameter set to 0.3-0.5

<details>
<summary>Answer</summary>

**Correct: D**

Image-to-image generation with low-to-medium strength (0.3-0.5) preserves structural composition from the input image while allowing variation in details like materials and lighting based on the text prompt. This is the purpose-built workflow for controlled variations.

**Why the others are wrong:**
- Same seed with different prompts produces completely different compositions - seed controls randomness, but different prompts generate different structures. Won't maintain layout.
- Inpainting works region-by-region. Fixing structures across an entire image is inefficient and produces inconsistent results. Wrong tool for global variations.
- Outpainting extends canvas beyond borders, doesn't modify existing content. Not applicable to changing materials/lighting in current design.

</details>

---

**Q8.** A social media company generates user avatars from text descriptions. They must comply with a regulation requiring that all generated facial images be detectable as synthetic. The detection must work even if users screenshot and re-upload images. Which Azure AI capability addresses this requirement?

A) Content safety's face detection API to verify synthetic origin
B) Digital signature stored in image metadata
C) Embedded watermarking with face-specific encoding patterns
D) Blockchain-based image provenance tracking

<details>
<summary>Answer</summary>

**Correct: C**

Azure AI's embedded watermarking for generated images includes specialized encoding that survives screenshots, social media compression, and re-uploads. Face-specific patterns are encoded imperceptibly across facial features, meeting the persistence requirement for regulatory compliance.

**Why the others are wrong:**
- Face detection identifies faces but doesn't determine if they're synthetic or real. It's a detection capability, not a watermarking/verification system.
- Digital signatures in metadata are lost during screenshots and re-uploads. Metadata is not embedded in pixels - fails the screenshot requirement.
- Blockchain isn't an Azure AI capability and doesn't solve the technical problem of marking image pixels as synthetic. Also requires external verification infrastructure not tied to the image file itself.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

