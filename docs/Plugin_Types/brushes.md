
# Brush Plugins
![Brushes](../brushes.png)

Brush Plugins in our AI image editing application are designed to extend the functionality of the Brush tool. Unlike layer plugins, these are not Web Components but are instead implemented as classes, each representing a different brush type. These classes provide specific drawing functions that allow for a variety of brush effects and behaviors.

## Overview

Brush plugins are classes that implement essential drawing functions. These brushes will be available in the Brush selection of the Brush tool, enabling users to choose from a variety of brush types for their drawing needs.

<img src="../brush1.png"  >

Selecting a brush 

## Required Functions

Each custom brush class must implement the following functions:

### 1. start(context, point)
- **Description**: Initializes the brush stroke.
- **Parameters**: 
  - `context`: The drawing context.
  - `point`: The starting point of the brush stroke.
- **Functionality**: Set up the initial state of the brush stroke, such as initial position and any necessary setup for the drawing context.


### 2. continue(context, newPoint)
- **Description**: Continues the brush stroke from the last point to the new point.
- **Parameters**: 
  - `context`: The drawing context.
  - `newPoint`: The new point to continue the brush stroke.  
- **Functionality**: Draw from the previous point to the new point, updating the drawing context as needed.



### 3. end()
- **Description**: Finalizes the brush stroke.
- **Functionality**: Perform any cleanup or finalization necessary after the brush stroke is complete.

### 4. refresh(globalValues)
- **Description**: This function will be called if brush size had been changed.
- **Parameters**: 
  - `globalValues`: A pointer to globalThis.gyre.paletteValues object having `brushSize` value. 


## Example Custom Brush Class

Here is an example of a simple custom brush class implementing the required functions:

```javascript
class SimpleBrush {
  start(context, point) {
    context.beginPath();
    context.moveTo(point.x, point.y);
  }

  continue(context, newPoint) {
    context.lineTo(newPoint.x, newPoint.y);
    context.stroke();
  }

  end() {
    // Finalize the stroke if needed
  }

   refresh(globalValues) {
    this.globalValues=globalValues // same as globalThis.paletteValues with brushSize parameter
   }

}
```


# Manifest Documentation

The manifest is a JSON object that describes the properties and settings of a brush tool used in an application. Below is a detailed description of each field in the manifest, along with an example for clarity.

## Manifest Structure


## Fields

### `type`
- **Description**: Specifies the type of tool.
- **Type**: `string`
- **Example**: `"brush"`

### `title`
- **Description**: The display name of the brush.
- **Type**: `string`
- **Example**: `"Caligraphy Brush"`

### `images`
- **Description**: Contains paths to images associated with the brush.
- **Type**: `object`
  - **largeIcon**
    - **Description**: Path to the large icon image shown in the brush tool menu and when selecting a brush. This field is required.
    - **Type**: `string`
    - **Example**: `"appdata/brushes/img/brush_Caligraphy2.jpg"`

### `form`
- **Description**: Defines the configurable parameters of the brush. Currently, only the slider type is supported.
- **Type**: `object`
  - **Parameter Name (e.g., spring, friction, splitNum, diff)**
    - **Description**: Name of the parameter.
    - **Type**: `object`
      - **label**
        - **Description**: Display name of the parameter.
        - **Type**: `string`
        - **Example**: `"Spring"`
      - **type**
        - **Description**: Type of the input control. Currently, only `slider` is supported.
        - **Type**: `string`
        - **Example**: `"slider"`
      - **from**
        - **Description**: Minimum value of the slider.
        - **Type**: `number`
        - **Example**: `1`
      - **to**
        - **Description**: Maximum value of the slider.
        - **Type**: `number`
        - **Example**: `1000`
      - **stepCount**
        - **Description**: Number of steps in the slider range.
        - **Type**: `number`
        - **Example**: `1`
      - **default**
        - **Description**: Default value of the slider.
        - **Type**: `number`
        - **Example**: `500`

## Example Manifest

```json
{
    "type": "brush",
    "title": "Caligraphy Brush",
    "images": {
        "largeIcon": "appdata/brushes/img/brush_Caligraphy2.jpg"
    },
    "form": {
        "spring": {
            "label": "Spring",
            "type": "slider",
            "from": 1,
            "to": 1000,
            "stepCount": 1,
            "default": 500
        },
        "friction": {
            "label": "Friction",
            "type": "slider",
            "from": 1,
            "to": 1000,
            "stepCount": 1,
            "default": 500
        },
        "splitNum": {
            "label": "Split number",
            "type": "slider",
            "from": 1,
            "to": 20,
            "stepCount": 1,
            "default": 10
        },
        "diff": {
            "label": "Diff",
            "type": "slider",
            "from": 1,
            "to": 20,
            "stepCount": 1,
            "default": 3
        }
    }
}
```

This manifest provides a structured way to define the properties and settings of a brush tool, including its type, title, associated images, and configurable parameters. The form parameters depend on the specific brush, with the slider type currently supported. The `largeIcon` image is required and used in the brush tool menu and selection process.

<img src="../brush2.png" >

Caligraphy Brush from our plugin examples.



