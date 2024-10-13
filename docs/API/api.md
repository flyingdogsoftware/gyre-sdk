# Gyre API
![Gyre API](../api.png)

The `gyre` API is accessible via `globalThis.gyre` and provides various parameters, functions, and APIs to interact with the current open document, handle layers, manage dialogs, and more in a ComfyUI/Gyre installation.

## Parameters and APIs

### Layers

- **layers**: An array of all layers in the current open document.

- **createLayerInstance**: Create layer instance defined by layer type. Such an instance can be used to add a layer via layerManager API (see below). An optional data object can be provided to fill layer properties.
    ```javascript
  gyre.createLayerInstance(
  /** @type {string} */ name,
  /** @type {string} */ type,
  /** @type {object} */ layerFrom)
    ```

### Tools Layers

- **tools_layers**: An array of all possible tools which are available.


### Mask Manager

- **maskManager**: API for handling image mask data.
  - **loadMask**: loads mask pixel data - it needs image URL (usually it is base64 image URL) as the only parameter. 


### Open Dialog

- **openDialog**: Opens a dialog from any workflow in a ComfyUI/Gyre installation. These are usually dialogs added to a workflow used in a plugin. It has got the workflow name as the first parameter.
    ```javascript
    gyre.ComfyUI.openDialog(
      /** @type {string} */ workflowName,
      /** @type {object} */ formData, 
      /** @type {string} */ title = '', 
      /** @type {function} */newformdatacallback)
    ```

- **openDialogById**: Same as `openDialog` but it has got the workflow-id as the first parameter.

### Image API

- **imageAPI**: Functions handling image data. See [imageAPI](/API/imageapi) documentation.

### layerManager API

- **layerManager**: Functions handling layers like adding or deleting layers. See [layerManager](/API/layermanager) documentation.

### Palette Values

- **paletteValues**: Current environment parameters. Most important parameter:
  - **paletteValues.currentLayer**: Accesses the active layer image data.
  - **internalCopyInfo**: information about image data which has put into clipboard from own application (`x`,`y`).

### Drag & Drop API

- **dragDrop**: An API for handling drag & drop operations, such as a rectangle for a simple selection.

## Callback Functions

- **copy** (`callbackFunctions['copy']`):  
  Provide your own callback function to be executed when the user selects the menu entry or shortcut for the copy-to-clipboard operation. This allows an activated tool to put something from its own context into the clipboard. Make sure to delete this callback if the tool is destroyed.

- **paste** (`callbackFunctions['paste']`):  
  Provide your own callback function to be executed when the user selects the menu entry or shortcut for the paste-from-clipboard operation. This allows an activated tool to copy something from the clipboard into its own context. Make sure to delete this callback if the tool is destroyed.

- **onKeyDown**  (`callbackFunctions['onKeyDown']`): Please use this callback if your plugin wants to support own shortcuts. It has got the HTML event object as only parameter. Typing in INPUT, TEXTARE or SELECT elements somewhere in the UI will be ignored so we highly recommend to use this callback instead your own keyboard handler. Make sure to delete this callback if the tool is destroyed.

## Undo/Redo Functionality

The system provides an `undo_redo_add` function that allows you to register actions for undo and redo, with custom callbacks to handle state restoration.

### Example: Undo/Redo for Shape Selection

```javascript
undo_redo_add('selection', 'shape', {beforeShapesJSON: largePaper.beforeShapesJSON, afterShapesJSON: largePaper.afterShapesJSON }, (state, action) => {
    let restoreShapes;
    if (action === 'undo') restoreShapes = state.data.beforeShapesJSON;
    if (action === 'redo') restoreShapes = state.data.afterShapesJSON;
    largePaper.restoreJSON(restoreShapes);
});
```




### ComfyUI API

- **gyre.ComfyUI**: The ComfyUI API, which offers the following function:
  - **gyre.ComfyUI.executeWorkflow**: Executes a workflow.
    ```javascript
    gyre.ComfyUI.executeWorkflow(
      /** @type {string} */ workflowName,
      /** @type {function} */ callback_finished,
      /** @type {function} */ callBack_files,
      /** @type {object} */ data,
      /** @type {function} */ callback_error
    )
    ```
  - **gyre.ComfyUI.executeWorkflowById**: Executes a workflow by internal ID.
    ```javascript
    gyre.ComfyUI.executeWorkflowById(
      /** @type {string} */ workflowID
      /** @type {function} */ callback_finished,
      /** @type {function} */ callBack_files,
      /** @type {object} */ data,
      /** @type {function} */ callback_error
    )
    ```
    If you use the `executeWorkflow` function please make sure that a unique name is used like one which is connected to plugin tag name (e.g. `fds-image-editor-sam points`). So the end-user would be able to deactivate the default workflow and make a new one with same name (but with another ID) which will be used from the plugin. 
  - **workflowList**: An array of all available workflows.
  - **getWorkflowById**: You can use this function to read a workflow by ID instead of searching in the list by own code.
  - **convertFormData**: Converts complex data from `openDialog` or `openDialogById` function to simple key-value structure which is used by `executeWorkflow` and `executeWorkflowById`.
    ```javascript
    data=gyre.ComfyUI.convertFormData(
      /** @type {object} */ formData
    )
    ```



## Canvas Parameters and Helper Functions

### Canvas Parameters

- **canvas.width**: The width of the open document/canvas.
- **canvas.height**: The height of the open document/canvas.
- **canvas.zoom**: The zoom level of the canvas.
- **canvas.screen**: Information about current visible part of the canvas: `width`, `height`, `scrollX` and `scrollY`.

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
