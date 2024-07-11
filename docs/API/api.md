# Gyre API
![Gyre API](../api.png)

The `gyre` API is accessible via `globalThis.gyre` and provides various parameters, functions, and APIs to interact with the current open document, handle layers, manage dialogs, and more in a ComfyUI/Gyre installation.

## Parameters and APIs

### Layers

- **layers**: An array of all layers in the current open document.

### Tools Layers

- **tools_layers**: An array of all possible tools which are available.

### Layer Manager

- **layerManager**: API for handling layers.

### Mask Manager

- **maskManager**: API for handling image mask data.
  - **loadMask**: loads mask pixel data - it needs image URL (usually it is base64 image URL) as the only parameter. 


### Open Dialog

- **openDialog**: Opens a dialog from any workflow in a ComfyUI/Gyre installation. These are usually dialogs added to a workflow used in a plugin.

### Image API

- **imageAPI**: Functions handling image data. See [imageAPI](imageapi.html) documentation.

### layerManager API

- **layerManager**: Functions handling layers of canvas like adding or deleting layers. See [layerManager](layermanager.html) documentation.

### Palette Values

- **paletteValues**: Current environment parameters. Most important parameter:
  - **paletteValues.currentLayer**: Accesses the active layer image data.

### Drag & Drop API

- **dragDrop**: An API for handling drag & drop operations, such as a rectangle for a simple selection.

### ComfyUI API

- **gyre.ComfyUI**: The ComfyUI API, which offers the following function:
  - **gyre.ComfyUI.executeWorkflow**: Executes a workflow.
    ```javascript
    gyre.ComfyUI.executeWorkflow(
      /** @type {string} */ workflowName,
      /** @type {function} */ callback_finished,
      /** @type {function} */ callBack_files,
      /** @type {object} */ formData,
      /** @type {function} */ callback_error
    );
    ```

## Canvas Parameters and Helper Functions

### Canvas Parameters

- **canvas.width**: The width of the open document/canvas.
- **canvas.height**: The height of the open document/canvas.
- **canvas.zoom**: The zoom level of the canvas.

### Canvas Helper Functions

- **canvas.calcPosition()**: Calculates a position parameter according to the zoom and screen aspect ratio.
- **canvas.mouseCoords(eventObject)**: Delivers the real coordinates calculated from the mouse pointer position relative to the canvas, zoom, and screen aspect ratio.
- **canvasComponent**: Internal pointer to some main application functions. These are not available yet.

## Example Usage

```javascript
// Accessing the gyre API
const layers = globalThis.gyre.layers;
const tools = globalThis.gyre.tools_layers;

// Using the layerManager API
globalThis.gyre.layerManager.addLayer(newLayer);

// Opening a dialog
globalThis.gyre.openDialog(dialogName, dialogOptions);

// Executing a workflow in ComfyUI
globalThis.gyre.ComfyUI.executeWorkflow(
  "exampleWorkflow",
  function onFinished() {
    console.log("Workflow finished");
  },
  function onFiles(files) {
    console.log("Files received", files);
  },
  { key: "value" },
  function onError(error) {
    console.error("Workflow error", error);
  }
);

// Canvas helper functions
const position = globalThis.gyre.canvas.calcPosition();
const coords = globalThis.gyre.canvas.mouseCoords(event);
