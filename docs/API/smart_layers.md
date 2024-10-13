# Smart Layers

## Overview
Gyre’s Smart Layers API provides advanced, non-destructive layer handling similar to "Smart Objects" in other image editing software. Each layer in Gyre is a **Smart Layer**, allowing for embedding, linking, transformations, and seamless updates. This SDK guide explains the core functionality, methods, and best practices for leveraging Gyre's Smart Layers API in your applications.

## Table of Contents
- [Getting Started](#getting-started)
- [Classes Overview](#classes-overview)
  - [Layer Creation](#layer-creation)
  - [Extending the Layer Class](#extending-the-layer-class)
- [Layer Properties](#layer-properties)
- [Layer Methods](#layer-methods)
  - [Common Methods](#common-methods)
  - [Image Operations Methods](#image-operations-methods)
- [Layer Usage](#layer-usage)
  - [Creating and Manipulating Layers](#creating-and-manipulating-layers)
  - [Non-Destructive Editing](#non-destructive-editing)
  - [Rendering Layers](#rendering-layers)
- [Best Practices](#best-practices)

## Getting Started
To use Gyre Smart Layers, use the `createLayerInstance` method from Gyre's global API to create instances of layers directly.

```javascript
globalThis.gyre.createLayerInstance(name, type, layerFrom);
```

### Parameters:
- **`name`** (`string`): The name of the new layer.
- **`type`** (`string`): The type of the layer (e.g., image, mask).
- **`layerFrom`** (`object`, optional): A predefined object containing layer data. This allows initializing the new layer with existing properties.

The result will be an instance of `layerBase`, which provides access to the full set of methods for layer manipulation.

## Classes Overview

### Layer Creation
Instead of importing `layerBase` directly, you create layers using the `globalThis.gyre.createLayerInstance()` method. This method abstracts the creation of new layers and ensures that all references and dependencies are properly initialized.

```javascript
let layer = globalThis.gyre.createLayerInstance('New Layer', 'image');
```

### Extending the Layer Class
For creating custom layer types or special plugins, you should create a new class that extends from the base layer class provided by Gyre. This ensures your custom layers inherit all the non-destructive features while allowing you to extend the functionality as needed.

To extend the base class, use:

```javascript
class MyCustomLayer extends globalThis.gyre.getLayerBaseClass() {
    constructor(name, type) {
        super(name, type);
        // Custom initialization logic here
    }

    customMethod() {
        // Define your custom behavior for the layer
    }
}
```

Using this approach, you can develop custom layers that integrate seamlessly with Gyre's core functionality.

## Layer Properties
Here are some essential properties that are part of the `layerBase`:

- **Dimensions**:
  - `width`, `height` - External get/set dimensions that may include referenced values.

- **Position and Transformation**:
  - `x`, `y` - Set or get the layer’s position within the document.
  - `rotation` - Set or get the layer’s rotation angle.

- **Layer Metadata**:
  - `name` - Custom name for the layer.
  - `type`, `subType` - Specifies the type (e.g., mask, image).
  - `opacity` - Adjusts the opacity of the layer (default is `100`).

- **References**:
  - `ref` - Points to another layer for linking attributes.
  - `refType` - Indicates the reference type (e.g., `moveTool`). Empty refType uses all properties - also the image data. With `moveTool` type only transformations  are used but not the image data. So for example mask layers have `refType` set to `moveTool` because the image data itself are just the mask image data.

## Layer Methods

### Common Methods

#### `getRefLayer()`
Returns the root layer for the current layer’s reference chain.

#### `getValueByRef(name)`
Fetches a value from the linked reference layer, allowing properties to be dynamically inherited.

#### `setValueByRef(name, value)`
Sets a value on the linked reference layer, ensuring consistent updates.

#### `toJSON()`
Serializes the layer into a JSON object, excluding internal properties and non-serializable types.

#### `unlink()`
Unlinks the current layer from its reference (`ref`), making it an independent layer.

#### `prepareDelete()`
Prepares a layer for deletion by properly unlinking all dependent references or clipping masks.

### Image Operations Methods

#### `previewCache()`
Generates and caches a preview of the layer. This is automatically triggered when properties change.

#### `generateAlphaMask()`
Generates an alpha mask and stores it for non-destructive use in masking. This function will be called automatically.

#### `render()`
Renders the current layer along with all masks, returning the rendered image's URL. 

#### `renderInDocumentSize(maxWidth = null)`
Renders the layer to fit the current document size, optionally scaling down via `maxWidth`.

#### `extractImageFromDocument(url)`
Extracts a segment of the document based on the current layer’s size, position, and rotation.


### `clippingMask()`

The `clippingMask()` function creates a clipping mask from a referenced layer and stores the image data in `this.url` from another layer (referred to as the clipping mask). This allows the clipping mask to behave like a standard layer mask, working specifically with the referenced image layer. This function will be called automatically.

## Masks

Gyre has go very powerful mask management and supports multiple masks, clipping masks , additional objects selected by masks and any kind of non-destructive filters provided by ComfyUI workflows. A mask layer automatically has got same transformation (positipon, rotation and size) of it's image layer.

Key elements:
- **ref**: The ID of the layer to which the mask is applied to.
- **Parent Layer**: The parent layer must be an image layer, as the clipping mask is applied to an image. SO the mask layer is entry of `children` array of such an image layer. Any image layer can have multiple masks/children.
- **refType**: The reference type property which has to be set to `"moveTool"` always.
- **type**: The layer type which is set to `mask` always.
- **refClippingMask**: The Clipping Mask references the image layer by ID where the mask data is coming from. Use this parameter only if you want to set a clipping mask.

### Masked Objects

Parts of an image can be extracted as objects. In this case additional linked layers are used on top of the original image layer. Each of such a linked layer has got it's own mask. In this case an additional layer with type `image` has to be generated but without an `url` parameter. With the `ref` parameter the image data is used from any the original image. The `refType` is not set in this case. As child in `children` array that new layer has got it's own mask. Transformation on orgiginal image like rotation, postion or resize and changing the image data will be automatically applied to the object layer.

### Filter on Mask

Use an Adjustment Layer to apply any filter (e.g. blur) to an mask.

## Layer Usage

### Creating and Manipulating Layers

To create a new layer:

```javascript
let layer = globalThis.gyre.createLayerInstance('Background Layer', 'image');
layer.width = 500;
layer.height = 300;
layer.x = 100;
layer.y = 150;
layer.rotation = 0.1; // Rotate by 0.1 radians
```

You can also initialize a layer with existing data using `layerFrom`:

```javascript
let predefinedData = { width: 400, height: 200, x: 50, y: 100, opacity: 80 };
let layer = globalThis.gyre.createLayerInstance('Predefined Layer', 'image', predefinedData);
```

### Non-Destructive Editing
Gyre layers are inherently **non-destructive**, allowing you to modify properties like `opacity`, `width`, `rotation`, and `position` without altering the original image data:

```javascript
layer.opacity = 75; // Adjusts the opacity while keeping the original data intact
layer.unlink(); // Unlink to make the layer independent of any references
```

### Rendering Layers

To render a layer including all applied masks and opacity settings:

```javascript
let renderedUrl = await layer.render(); // Get a URL for the rendered image
```

Render a scaled preview:

```javascript
let previewUrl = await layer.renderInDocumentSize(600); // Render with a max width of 600 pixels
```

## Best Practices

1. **Use Layer References for Reusability**: Leverage `ref` to link layers for easy reuse across different documents, ensuring design consistency.

2. **Custom Plugins with Extended Classes**: When creating new layer types, extend the base class using `globalThis.gyre.getLayerBaseClass()`. This allows you to build custom behavior while retaining the functionality of a standard Gyre layer.

   ```javascript
   class MySpecialLayer extends globalThis.gyre.getLayerBaseClass() {
       constructor(name, type) {
           super(name, type);
           // Custom initialization
       }
   }
   ```

3. **Caching Considerations**:
   - Use the `nocache` flag to disable automatic image generation caching during operations like resizing to improve performance.  
   - Once intensive operations are completed it has to be set to `false` which calls the `previewCache()` automatically  to update previews.

4. **Prepare for Deletions**: Always use `prepareDelete()` before removing layers to handle references and dependencies properly.



