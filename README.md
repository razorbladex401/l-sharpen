# Enhanced Luminance Sharpen

A PixInsight script for advanced luminance sharpening of astrophotography images. This script combines Unsharp Mask and Multiscale Linear Transform techniques to enhance fine detail while protecting dark areas and minimizing artifacts.

## What is This?

Enhanced Luminance Sharpen is an automated sharpening workflow script based on the [Chaotic Nebula sharpening methodology](https://chaoticnebula.com/pixinsight-sharpening/). It provides a single-click solution for professional-grade image sharpening that would normally require multiple manual steps.

### Key Features

- **Automatic Luminance Extraction**: Works with both color and grayscale images - automatically extracts the luminance channel from color images
- **Dual Sharpening Methods**: 
  - Unsharp Mask (USM) for edge sharpening
  - Multiscale Linear Transform (MLT) for wavelet-based detail enhancement
- **Protective Masking**: Optional mask creation to protect dark areas from over-sharpening
- **Smart Blending**: Intelligently combines original image with both sharpening methods
- **Non-Destructive**: Creates a new sharpened image while preserving your original
- **Automatic Cleanup**: Removes all intermediate images, leaving only the final result
- **Full Color Preservation**: For color images, recombines sharpened luminance with original color information using CIE L*a*b* color space

### How It Works

1. Extracts the luminance channel from your image (if color)
2. Optionally creates a protective mask from dark areas
3. Applies Unsharp Mask sharpening with mask protection
4. Applies Multiscale Linear Transform sharpening with mask protection
5. Blends the original and both sharpened versions using configurable weights
6. For color images, recombines the sharpened luminance with original color channels
7. Cleans up all intermediate files automatically
8. Returns a single "_Sharpened" image ready to use

## Installation

### Requirements

- PixInsight 1.8.9 or later (tested with 1.9.3)
- No additional dependencies required

### Installation Steps

1. **Download the Script**
   - Download `EnhancedLuminanceSharpen.js` to your computer

2. **Install in PixInsight**
   
   **Option A: Using the Scripts Menu (Recommended)**
   - In PixInsight, go to `Script > Feature Scripts...`
   - Click `Add` button
   - Navigate to and select `EnhancedLuminanceSharpen.js`
   - Click `OK`
   - The script will now appear in `Script > Image Processing > EnhancedLuminanceSharpen`

   **Option B: Copy to Scripts Directory**
   - Locate your PixInsight scripts directory:
     - **Windows**: `C:\Program Files\PixInsight\src\scripts\`
     - **macOS**: `/Applications/PixInsight/src/scripts/`
     - **Linux**: `/opt/PixInsight/src/scripts/`
   - Copy `EnhancedLuminanceSharpen.js` to this directory
   - Restart PixInsight
   - The script will appear in `Script > Image Processing > EnhancedLuminanceSharpen`

3. **Verify Installation**
   - Go to `Script > Image Processing`
   - You should see "EnhancedLuminanceSharpen" in the menu

## How to Use

### Basic Workflow

1. **Open Your Image** in PixInsight (color or grayscale)

2. **Launch the Script**
   - Go to `Script > Image Processing > EnhancedLuminanceSharpen`
   - The Enhanced Luminance Sharpen dialog will open

3. **Select Target Image**
   - Click the target image dropdown
   - Select the image window you want to sharpen

4. **Adjust Parameters** (optional - defaults work well for most images)
   - See "Parameters Guide" section below for details

5. **Click OK**
   - The script will process your image
   - Watch the console for progress updates
   - When complete, a new window named `[YourImage]_Sharpened` will appear

6. **Review Results**
   - Compare the sharpened image with your original
   - If needed, close the sharpened image and run again with adjusted parameters

### Parameters Guide

The dialog is organized into four collapsible sections:

#### 1. Target Image
- **Target image**: Select which image window to sharpen (required)

#### 2. Unsharp Mask Settings
- **Amount** (default: 0.80)
  - Strength of the Unsharp Mask effect
  - Range: 0.00 to 2.00
  - Higher values = stronger edge sharpening
  - Start with 0.80 and adjust to taste

- **Standard Deviation** (default: 2.50)
  - Radius of the Unsharp Mask blur kernel
  - Range: 0.10 to 10.00
  - Smaller values sharpen finer details
  - Larger values sharpen broader features
  - 2.50 is a good general-purpose value

#### 3. Multiscale Linear Transform Settings
- **Layer 2 Bias** (default: 0.10)
  - Controls sharpening at 4-pixel scale (2x2)
  - Fine detail enhancement

- **Layer 3 Bias** (default: 0.05)
  - Controls sharpening at 9-pixel scale (3x3)
  - Medium detail enhancement

- **Layer 4 Bias** (default: 0.02)
  - Controls sharpening at 16-pixel scale (4x4)
  - Broader features

- **Layer 5 Bias** (default: 0.00)
  - Controls sharpening at 25-pixel scale (5x5)
  - Even larger scale features
  - Set to 0 by default (inactive)
  - Increase to add more fine detail sharpening

For all layer biases:
- Range: -1.00 to 1.00
- Positive values = sharpen
- Negative values = blur
- 0 = no effect

#### 4. Blending Settings
Controls how the original and sharpened versions are combined:

- **Original Weight** (default: 0.30)
  - Weight of the unsharpened original image
  
- **USM Weight** (default: 0.35)
  - Weight of the Unsharp Mask result
  
- **MLT Weight** (default: 0.35)
  - Weight of the Multiscale Linear Transform result

**Note**: Weights don't need to sum to 1.0 - they're automatically normalized.

#### 5. Mask Settings
- **Create sharpening mask** (checkbox, default: ON)
  - When enabled, creates a protective mask to prevent over-sharpening dark areas
  - Recommended to keep enabled for most images
  - Disable for images where you want to sharpen the entire frame equally

- **Auto Clip Shadows** (default: 0.01)
  - Shadow clipping point for mask creation
  - Range: 0.00 to 0.50
  - Higher values protect more dark areas

- **Midtones Balance** (default: 0.25)
  - Midtone adjustment for mask
  - Range: 0.00 to 1.00
  - Adjust how the mask transitions from protected to unprotected areas

### Tips for Best Results

#### General Guidelines
- **Start with defaults**: The default parameters work well for most astrophotography images
- **Use the mask**: Keep "Create sharpening mask" enabled to protect dark areas
- **Subtle is better**: Over-sharpening creates artifacts - when in doubt, use less
- **Process stretched images**: Apply sharpening after histogram stretch, not on linear data

#### For Different Image Types

**Deep Sky Objects (Galaxies, Nebulae)**
- Defaults work well
- Consider increasing USM Amount to 1.0 for more detail
- Keep Layer 2 Bias at 0.10 for fine detail

**Star Fields**
- Reduce USM Amount to 0.60 to avoid overly sharp stars
- Reduce Layer 2 Bias to 0.05 to prevent star artifacts
- Consider masking stars before running (using StarNet++ or similar)

**Planetary Images**
- Increase USM Amount to 1.2-1.5
- Increase Layer 2 and 3 Biases for fine detail
- Reduce MLT Weight to 0.20 and increase USM Weight to 0.50

**Moon/Solar**
- Similar to planetary settings
- May want to disable mask (uncheck "Create sharpening mask")

#### Troubleshooting

**Problem: Sharpening too strong**
- Reduce USM Amount (try 0.60)
- Reduce Layer 2 and 3 Biases
- Increase Original Weight, decrease USM and MLT Weights

**Problem: Not enough sharpening**
- Increase USM Amount (try 1.0-1.2)
- Increase Layer 2 and 3 Biases
- Decrease Original Weight, increase USM and MLT Weights

**Problem: Halos around bright objects**
- Reduce USM Standard Deviation (try 1.5-2.0)
- Reduce USM Amount
- Ensure mask is enabled

**Problem: Dark areas over-sharpened**
- Ensure "Create sharpening mask" is enabled
- Increase Auto Clip Shadows value (try 0.02-0.05)

**Problem: Artifacts or noise amplification**
- Run noise reduction before sharpening
- Reduce Layer 2 Bias (finest scale)
- Reduce overall sharpening strength

## Technical Details

### Color Space Handling
- Color images: Converted to CIE L*a*b* color space
  - Sharpening applied only to L* (luminance) channel
  - Original a* and b* (color) channels preserved
  - Prevents color fringing and false color artifacts
- Grayscale images: Processed directly

### Processing Pipeline
1. Channel extraction (color images only)
2. Mask creation (optional)
3. Unsharp Mask application
4. Multiscale Linear Transform application
5. Blending using direct pixel operations
6. Channel recombination (color images only)
7. Automatic cleanup

### File Output
- Original image: Unchanged
- Output image: Named `[OriginalName]_Sharpened`
- All intermediate images: Automatically deleted

## Credits

- **Methodology**: Based on [Chaotic Nebula's Sharpening Workflow](https://chaoticnebula.com/pixinsight-sharpening/)
- **Implementation**: Custom PixInsight JavaScript Runtime (PJSR) script
- **License**: MIT License (see LICENSE file)

## Support

For issues, questions, or suggestions:
- Check the Tips and Troubleshooting sections above
- Refer to PixInsight documentation for core processes
- Review the Chaotic Nebula tutorial for methodology details

## Version History

**Current Version**: 1.0.0
- Initial release
- Automatic luminance extraction and color recombination
- Dual sharpening methods (USM + MLT)
- Protective masking
- Configurable blending
- Support for 5 MLT layers (2-6)
- Automatic cleanup

---

**Enjoy sharpening your astrophotography images!**
