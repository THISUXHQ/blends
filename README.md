```markdown
# Documentation for Blend Modes in Svelte Application

This Svelte application provides a user interface for blending two images using various blend modes. The core functionality is implemented in TypeScript within a Svelte component. Below is the documentation for the key parts of the script.

### Variables

- `baseImage: HTMLImageElement`: A reference to the base image HTML element.
- `blendImage: HTMLImageElement`: A reference to the blend image HTML element.
- `blendedImageDataUrl: string`: A string that holds the data URL of the blended image result.

### Blend Mode Functions

Each blend mode function takes two parameters, `base` and `blend`, which represent the base and blend color values (ranging from 0 to 1) for a single color channel, and returns the result of the blend mode operation for that channel.

- `softLight(base, blend)`: Applies a soft light blend mode, which is a combination of darken and lighten modes depending on the blend color. The effect is similar to shining a diffused spotlight on the image.
- `multiply(base, blend)`: Multiplies the base color by the blend color, resulting in a darker image. This mode is useful for adding shadows and depth.
- `screen(base, blend)`: Combines the base and blend colors in a way that results in a brighter image. It's the opposite of multiply and is useful for creating highlights.
- `overlay(base, blend)`: Combines multiply and screen blend modes depending on the base color. It either multiplies or screens colors, resulting in a contrast increase.
- `darken(base, blend)`: Selects the darker of the base and blend colors for each channel, making the image darker.
- `lighten(base, blend)`: Selects the lighter of the base and blend colors for each channel, making the image lighter.
- `colorDodge(base, blend)`: Brightens the base color to reflect the blend color. Blending with black produces no change.
- `colorBurn(base, blend)`: Darkens the base color to reflect the blend color. Blending with white produces no change.

### blendImages Function

The `blendImages` function is responsible for creating the blended image. It accepts a blend mode function as a parameter and applies this blend mode to each pixel of the base and blend images to produce a blended image.

**Parameters:**
- `blendModeFunction`: A function that represents the blend mode to apply.

**Functionality:**
- Creates canvas elements for the base, blend, and blended images.
- Draws the base and blend images onto their respective canvases.
- Retrieves the image data from both canvases and creates a new image data object for the blended image.
- Loops through each pixel, applying the blend mode function to the color channels (RGB) and sets the alpha channel to 255 (fully opaque).
- Draws the blended image data onto the blended canvas and updates `blendedImageDataUrl` with the data URL of the blended image.

### blendWithMode Function

This helper function triggers the blending process using a specified blend mode function.

**Parameters:**
- `modeFunction`: The blend mode function to apply.

**Functionality:**
- Checks if both images are fully loaded.
- Calls `blendImages` with the specified blend mode function.

### User Interface

The user interface consists of two image elements for displaying the base and blend images and a series of buttons for each blend mode. Clicking a button applies the corresponding blend mode to the images and displays the result. There is also a button to download the blended image.

This documentation provides an overview of the blend modes functionality implemented in the Svelte application. Each blend mode offers a unique way to combine images, allowing for creative image processing directly in the browser.
```