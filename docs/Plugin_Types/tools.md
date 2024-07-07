# Tools
![Tools](../tools.png)

As research in the field of image and video editing progresses rapidly, new ideas and tools for manipulating and generating images are continually emerging. Frameworks like ComfyUI, while useful, often don't cover all aspects comprehensively. Our platform provides a powerful environment not only for end users, such as artists and designers, but also for researchers and developers. It facilitates the quick implementation of innovative UI ideas, benefiting artists and designers by enhancing their creative processes and productivity. These tools are not limited to advanced AI features; they also include traditional image editing tools.

## Possible Examples of AI Tools

- **Interactive Segmentation Tools**: Allow users to manually select and refine object boundaries in images.
- **Neural Style Transfer Tools**: Enable users to apply the style of one image to another, often requiring multiple iterations and adjustments.
- **Image-to-Image Translation Tools**: Require user input to guide the translation process between different domains, such as sketches to realistic images.
- **Face Morphing and Animation Tools**: Allow users to create and adjust facial animations and morphing effects interactively.
- **Real-Time Image Enhancement Tools**: Allow users to apply and tweak enhancements such as super-resolution, denoising, and color correction interactively.
- **Virtual Try-On Tools**: Require user input to adjust clothing and accessories on virtual avatars based on user preferences and body measurements.
- **Interactive Object Removal and Insertion Tools**: Let users specify and refine areas for object removal or insertion in images.

## Possible Examples of Non-AI Tools Usually Found in Traditional Image Editing Applications

- Selection Tools
- Crop and Slice Tools
- Clone Stamp Tools
- Gradient and Paint Tools
- Blur and Sharpen Tools
- Dodge and Burn Tools
- Drawing and Shape Tools
- Text Tools
- Eyedropper Tool

For supporting development, we offer a lightweight environment with some basic functions like zoom, which is the most challenging part for developing such a tool.

![Bild](#)

## Setting Up a New Tool

The steps are similar to custom UI components, so these are also based on Web Components. For parameter selection, we offer a floating toolbar that is located automatically depending on document size and scroll position screen size. Each tool needs an additional web component for dialog elements of such a toolbar with the suffix `.floating-toolbar`.

![Bild](#)

### Example

```html
<fds-image-editor-sam layer {tool_layer}></fds-image-editor-sam>
<fds-image-editor-sam-floating-toolbar layer {tool_layer}></fds-image-editor-sam-floating-toolbar>
```

The `{tool_layer}` parameter points to an internal data object storing all information of the current tool. Both components are used automatically by selecting a tool in the toolbar. Quite often, such a tool offers different variations, so the main application supports a menu with the sub-tools for switching between the states.

![Bild submenu](#)

For testing a tool, just use our test environment from our SDK:

```html
<script src="https://flyingdogsoftware.github.io/gyre-ui-dist/dist/node_modules/%40fds-components/fds-ai-image-editor/dist/fds-ai-image-editor.js?1"></script>
<script src="https://flyingdogsoftware.github.io/gyre-ui-dist/dist/fds-image-editor-canvas.js"></script>
```

The first script activates the connection to a ComfyUI server with workflow management, workflow pre-parser, and execution of any ComfyUI workflows by name.

With the test environment tag, you easily start the test environment with an example image:

```html
<fds-image-editor-test-env image_url="example.webp" id="stage"></fds-image-editor-test-env>
```

Now activate our plugin in the test environment:

```javascript
document.addEventListener("DOMContentLoaded", async function() {
    const stage = document.getElementById('stage');
    globalThis.gyre.registerPlugin("fds-image-editor-sam", {
        type: "tool",
        floatingToolBar: true,
        floatingToolBarWidth: 425,
        icons: {
            "eye": {
                body: '<path ...>'
            }
        }
    });
});
```

It uses a stripped-down version of the manifest here as well to keep things easy.

To activate the plugin on the test environment canvas, just use:

```javascript
stage.activateTool("tool-tag", "sub-tool"); // so in this example:
stage.activateTool("fds-image-editor-sam", "rect");
```

Both tool components need to provide a `refresh()` function which will be automatically called if something has been changed in the main application. This is usually activation of the tool, zoom function, or setting a new sub-tool.

## Gyre API

Inside the tool plugin, we provide a helper function for the calculation of mouse coordinates to image coordinates and vice versa. This calculation depends on three factors: the coordinate, the zoom factor, and the device screen ratio.

### Example (snippet from refresh function):

```javascript
if (tool_layer.points) {
    for (let point of tool_layer.points) {
        point._x = canvas.calcPosition(point.x);
        point._y = canvas.calcPosition(point.y);
    }
}
```

`canvas` is just the canvas object of the Gyre-API here.

In addition to the `refresh` function, the tool has to provide `prepareForSave()` and `prepareAfterLoad()` functions. In a complex gyre file, the current state of each tool is stored as well, so these usually provide a serialized version of `toolsLayer` without unneeded data.

## Manifest

This section provides detailed information about the JSON manifest structure for defining tools in your image editing plugin.

## JSON Structure

### Root Object

- **type** (string): The type of the plugin. Example: `"tool"`.
- **floatingToolBar** (boolean): Specifies if the toolbar should float. Example: `true`.
- **floatingToolBarWidth** (number): The width of the floating toolbar. Example: `490`.
- **title** (string): The title of the plugin. Example: `"Auto Matte"`.
- **layerTypes** (array): An array of layer types the tool is applicable to. Example: `["image"]`.
- **tools** (array): An array of tool objects.
- **icons** (object): An object containing icon definitions.

### Tool Object

Each tool object has the following structure:

- **name** (string): The name of the tool. Example: `"rect"`.
- **title** (string): The display title of the tool. Example: `"Rectangular Selection"`.
- **icon** (string): The identifier for the tool's icon. Example: `"fds-image-editor-sam-rect"`.
- **floatingToolBarWidth** (number): The width of the floating toolbar for this tool. Example: `440`.

### Icons Object

The icons object contains key-value pairs where each key is the name of an icon and the value is an object with the following structure:

- **body** (string): The SVG path data for the icon.
- **width** (number, optional): The width of the icon. Example: `1024`.
- **height** (number, optional): The height of the icon. Example: `1024`.

## Example

Here is an example of a JSON manifest for image editing tools:

```json
{
  "type": "tool",
  "floatingToolBar": true,
  "floatingToolBarWidth": 490,
  "title": "Auto Matte",
  "layerTypes": ["image"],
  "tools": [
    {
      "name": "rect",
      "title": "Rectangular Selection",
      "icon": "fds-image-editor-sam-rect",
      "floatingToolBarWidth": 440
    },
    {
      "name": "points",
      "title": "Selection by Points",
      "icon": "fds-image-editor-sam-points",
      "floatingToolBarWidth": 570
    }
  ],
  "icons": {
    "eye": {
      "body": "SVG body here"
    },
    "toolbar-icon": {
      "body": "SVG body here"
    },
    "points": {
      "body": "SVG body here"
    }
  }
}
```