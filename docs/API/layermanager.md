# layerManager API

The `globalThis.gyre.layerManager` API provides functions to manage image layers on a canvas. Below are the detailed descriptions of each function along with example usage.

## Functions

### `addLayer(layer)`
Adds a new layer to the canvas.

#### Parameters
- **layer**: `Object` - The layer object to be added.

#### Example
```javascript
let gyre = globalThis.gyre;
let newLayer = {
    type: 'image',
    name: 'Test Layer',
    x: 0,
    y: 0,
    width: gyre.canvas.width,
    height: gyre.canvas.height,
    url: myBase64ImageURL
};

gyre.layerManager.addLayer(newLayer);
```
This example adds an image layer with the current document/canvas size.

### `deleteLayer(layerObject)`
Deletes a specified layer from the layer list.

#### Parameters
- **layerObject**: `Object` - The layer object to be deleted.


### `addLayerMask(url=null,extractFromDocumentSize=false)`
Easiest method to add a layer mask to current selected layer. For advanced mask managemet please check out [Smart Layers](/API/smart_layers).

#### Parameters
- **url**: `base64` - An image in base64 URL format which has to be in black/white and in same size as current selected layer image. If empty just an empty mask will be added.

- **extractFromDocumentSize**: `boolean` - If true the mask image data will be provided in document size instead of current selected layer image size. From that full image the part which fits to current selected layer image data (x,y,with,height and rotation) will be extracted.


### `getLayersBySubType(subType)`
Retrieves all layers of a specified sub-type.

#### Parameters
- **subType**: `string` - The sub-type of layers to retrieve.

#### Returns
- `Array` - An array of layers matching the specified sub-type.

### `getLayerById(id)`
Searches globally for a layer by its ID.

#### Parameters
- **id**: `string` - The ID of the layer to retrieve.

#### Returns
- `Object` - The layer instance.

### `findById(parent,id)`
Searches globally for a layer by its ID and returns result object with information about layer and parent.

#### Parameters
- **parent**: `layer` - The parent layer object, use  `null` if you want to search from root.
- **id**: `string` - The ID of the layer to retrieve.

#### Returns
- `Object` - An object with `entry` which is layer instance and `parent` which is parent layer.


### `getLayersSameGroup(id)`
Searches globally for a layer by its ID and returns a list of all layers in same group. If layer is not grouped it returns all top layers.

#### Parameters
- **id**: `string` - The ID of one layer in the group layer list to retrieve - not the parent layer ID.

#### Returns
- `Array` - An array of all layers within the same group.

### `observeChangesSameGroup(id,callback,interval)`
Searches globally for a layer by its ID and detects changes in same group or top level if layer is not grouped. Changes are changing of layer data, adding or removing layers.

#### Parameters
- **id**: `string` - The ID of one layer in the group layer list to observe - not the parent layer ID.

#### Parameters
- **callback**: `function` - A callback function which will be called after every change. The function gets a list of all changes as parameter after last callback call.

#### interval
- **interval**: `int` - The interval for checking changes in milliseconds (1000=1 sec). 

#### Returns
- `string` - The ID if the created observer

### `deleteObserver(id)`
Deletes observer created by observeChangesSameGroup.

- **id**: `string` - The ID of the observer to remove.


### `getLayerByName(name)`
Retrieves a layer by its name.

#### Parameters
- **name**: `string` - The name of the layer to retrieve.

#### Returns
- `Object` - The layer object with the specified name.

### `moveLayerBelow(layerBelowId, layerAboveId)`
Moves a layer below another specified layer.

#### Parameters
- **layerBelowId**: `string` - The ID of the layer to move below.
- **layerAboveId**: `string` - The ID of the layer above which the other layer should be moved.

### `nextImageLayer(layer)`
Finds the next image layer, skipping all other layers in between.

#### Parameters
- **layer**: `Object` - The current layer object.

#### Returns
- `Object` - The next image layer.

### `previousImageLayer(layer)`
Finds the previous image layer, skipping all other layers in between.

#### Parameters
- **layer**: `Object` - The current layer object.

#### Returns
- `Object` - The previous image layer.

### `selectLayers(arrayOfIDs)`
Selects layers specified by an array of IDs. Currently, only the layer ID is supported in the list. This function is useful to just refresh the layer panel after changes to the layer structure.

#### Parameters
- **arrayOfIDs**: `Array` - An array of layer IDs to be selected.

### `maxId()`
Get highest layer ID. So if you want to add a new layer just add 1 for it's ID.

#### Returns
- `integer` - The highest layer ID.

