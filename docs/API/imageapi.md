# ImageAPI Documentation

The `ImageAPI` provides a set of functions to manipulate images using HTML canvas. The API supports various operations such as color adjustments, scaling, combining image layers, and processing individual pixels based on custom logic.

## Functions

### `setColorPreserveAlpha`
Sets the color of an image while preserving its alpha channel.

#### Parameters
- **url**: `string` - The URL of the image.
- **r**: `number` - The red component of the new color.
- **g**: `number` - The green component of the new color.
- **b**: `number` - The blue component of the new color.

#### Returns
- `Promise` - A promise that resolves when the color change is applied.

---

### `base64ToCanvas`
Converts a base64 encoded image to an HTML canvas element.

#### Parameters
- **base64**: `string` - The base64 encoded image string.
- **width**: `number` - The width of the canvas.
- **height**: `number` - The height of the canvas.

#### Returns
- `Promise` - A promise that resolves with the canvas element.

---

### `getImageDataCombined`
Retrieves image data from multiple layers within a bounding box and optionally merges the alpha channel from a mask.

#### Parameters
- **layers**: `Array` - The layers from which to retrieve image data.
- **x**: `number` - The x-coordinate of the bounding box.
- **y**: `number` - The y-coordinate of the bounding box.
- **width**: `number` - The width of the bounding box.
- **height**: `number` - The height of the bounding box.
- **document**: `Object` - An object containing the width and height of the document.
- **maskCanvas**: `Canvas` (optional) - The canvas element containing the mask alpha channel.
- **ignoreType**: `boolean` (optional) - If true, layer types are ignored.
- **ignoreSubType**: `boolean` (optional) - If true, layer subtypes are ignored.

#### Returns
- `Promise` - A promise that resolves with the base64 encoded image data.

---

### `scaleImageCanvas`
Scales an image using an HTML canvas element.

#### Parameters
- **url**: `string` - The URL of the image.
- **width**: `number` - The target width of the scaled image.
- **height**: `number` - The target height of the scaled image.

#### Returns
- `Promise` - A promise that resolves with the base64 encoded scaled image.

---

### `getImageSize`
Retrieves the dimensions of an image.

#### Parameters
- **url**: `string` - The URL of the image.

#### Returns
- `Promise` - A promise that resolves with an object containing the width and height of the image.

---

### `adjustDimensions`
Calculates image dimensions to fit within a maximum size and be divisible by a specified divider.

#### Parameters
- **maxWidth**: `number` - The maximum width.
- **maxHeight**: `number` - The maximum height.
- **targetWidth**: `number` - The target width.
- **targetHeight**: `number` - The target height.
- **devider**: `number` (optional) - The divider for the dimensions (default is 64).

#### Returns
- `Promise` - A promise that resolves with an object containing the adjusted width and height.

---

### `fillImageBackground`
Fills the transparent parts of an image with a specified color.

#### Parameters
- **url**: `string` - The URL of the image.
- **color**: `string` - The color to fill the transparent parts with.

#### Returns
- `Promise` - A promise that resolves with the base64 encoded image with the filled background.

---

### `canvasMakeTransparentWhite`
Makes the transparent parts in the alpha channel of a canvas white.

#### Parameters
- **canvas**: `Canvas` - The canvas element to modify.

---

### `canvasInvert`
Inverts the colors of an image on a canvas.

#### Parameters
- **canvas**: `Canvas` - The canvas element to modify.

---

### `canvasExpandMaskFast`
Performs auto dilation on a mask with a fixed size and returns a new canvas.

#### Parameters
- **canvas**: `Canvas` - The canvas element containing the mask.

#### Returns
- `Promise` - A promise that resolves with the new canvas element.

---

### `applyAlphaChannel`
Copies the alpha channel from one image to another.

#### Parameters
- **base64**: `string` - The base64 encoded image string (or an image URL) of the target image.
- **sourceImageBase64**: `string` - The base64 encoded image string (or an image URL) of the source image.

#### Returns
- `Promise` - A promise that resolves with the new image in base64 format.

---

### `processImage`
Processes the pixels of an image based on a condition and modification function.

#### Parameters
- **imageUrl**: `string` - The URL of the image to be processed.
- **modifyPixel**: `function` - A function that takes the pixel data and index, allowing for custom pixel manipulation.
- **imageUrl2**: `string` (optional) - The URL of a second image, if needed for comparison or additional data processing.

#### Returns
- `Promise` - A promise that resolves with the modified image as a base64 encoded string.

---

### `convertTransparentToWhite`
Converts all transparent pixels in an image to white.

#### Parameters
- **imageUrl**: `string` - The URL of the image.

#### Returns
- `Promise` - A promise that resolves with the image in base64 format with transparent pixels converted to white.

---

### `convertWhiteToTransparent`
Converts all white pixels in an image to transparent.

#### Parameters
- **imageUrl**: `string` - The URL of the image.

#### Returns
- `Promise` - A promise that resolves with the image in base64 format with white pixels converted to transparent.

---

### `loadImage`
Loads an image from a URL into an `Image` object.

#### Parameters
- **url**: `string` - The URL of the image to load.

#### Returns
- `Promise` - A promise that resolves with the loaded `Image` object.

---

## Example Usage

```javascript
// Example of how to use the setColorPreserveAlpha function
await gyre.imageAPI.setColorPreserveAlpha('image_url', 255, 0, 0)

// Example of how to use the getImageSize function
let size = await gyre.imageAPI.getImageSize('image_url')
console.log(size)

// Example of how to use processImage with a custom pixel modification
await gyre.imageAPI.processImage('image_url', (data, i) => {
  if (data[i + 3] === 0) {
    data[i] = 255 // Red
    data[i + 1] = 255 // Green
    data[i + 2] = 255 // Blue
    data[i + 3] = 255 // Fully opaque
  }
})
```