# Blend Modes in Svelte 

A Svelte app to blend images using various modes. Core logic in TypeScript.

### Variables

- `baseImage`: Base image element
- `blendImage`: Blend image element  
- `blendedImageDataUrl`: Data URL of blended result

### Blend Mode Functions

Functions that blend base and blend color values:

- `softLight`, `multiply`, `screen`, `overlay`
- `darken`, `lighten`, `colorDodge`, `colorBurn`

### blendImages(blendModeFunction)

- Creates canvases for base, blend, blended images
- Draws base and blend images on canvases
- Applies blend mode function to each pixel's RGB channels
- Sets blended image data URL

### blendWithMode(modeFunction)  

- Checks if images loaded
- Calls `blendImages` with given mode function

### UI

- Image elements for base and blend
- Buttons for each blend mode
- Button to download blended image

Blend modes allow creative image processing in the browser.
```

This version:

- Uses shorter descriptions and bullet points
- Groups related information together
- Removes redundant explanations
- Focuses on the key components and functionality

The goal is to provide a high-level overview of the blend modes functionality in a more concise and scannable format.