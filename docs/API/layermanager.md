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
- `Object` - The layer object with the specified ID.

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
Selects layers specified by an array of IDs. Currently, only the layer ID is supported in the list.

#### Parameters
- **arrayOfIDs**: `Array` - An array of layer IDs to be selected.
