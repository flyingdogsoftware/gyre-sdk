## UI Components
![UI Components](../components.png)


In any image editing application, the form input is usually more complex than just standard form elements like text input, switches, or sliders. ComfyUI nodes also struggle with this problem, resulting in text input for configuration of node parameters (e.g., gradient values as string instead of colorful gradient sliders). With our powerful form editor, the end-user can adapt new workflows directly inside the main application as dialogs. Providing these users powerful UI components is key for having an overall professional and easy-to-use interface.

We also offer direct configuration for splitting one string value into several values, which can be assigned with our Gyre mappings tool to each ComfyUI node parameter.

<img src="../component2.png" >

Gradient slider component used two times in a dialog.

<img src="../component3.png"  width="400">

Same component offered in form editor so non-developer can use it for any workflows.

### Example

```html
<fds-gradient-slider name="gradient" value="0;1.0"></fds-gradient-slider>
```

Using `split_value_num=2` and `split_value_type=number`, the above example will provide:

- A field "gradient" with the value string "0;1.0".
- Field "gradient_0" with the float value 0.
- Field "gradient_1" with the float value 1.0.

These fields are utilized in Gyre mappings.



## Manifest

After you have developed and tested your UI element in a separate project, add it to Gyre with a component manifest in `gyre_ui_components.json` inside the `gyre_entry` folder of your ComfyUI extension. Follow the syntax of this example:

```json
{
  "components": [
    {
      "tag": "fds-gradient-slider",
      "attributes": {
        "name": "gradient",
        "value": "0;1.0",
        "split_value_num": 2,
        "split_value_type": "number"
      }
    }
  ]
}
```

## JSON Structure

### Root Object

- **copyright** (string): The copyright information.
- **components** (array): An array of component objects.

### Component Object

Each component object has the following structure:

- **name** (string): The name of the component.
- **tag** (string): The HTML tag for the component.
- **icon** (string): An SVG icon representing the component.
- **parameters** (object): An object containing the parameters for the component.

### Parameters Object

The parameters object contains key-value pairs where each key is the name of a parameter and the value is an object with the following structure:

- **type** (string): The type of the parameter (e.g., "text", "textarea").
- **default** (string): The default value of the parameter.
- **label** (string): The label for the parameter.

## Example

Here is an example of a JSON manifest for two components:

```json
{
  "copyright": "2024 flying dog software",
  "components": [
    {
      "name": "Gradient Editor",
      "tag": "fds-gradient-editor",
      "icon": "<svg > ... Icon here...</svg>",
      "parameters": {
        "name": { "type": "text", "default": "gradient", "label": "Name" },
        "label": { "type": "text", "default": "Gradients", "label": "Label" },
        "default": {
          "type": "textarea",
          "default": "0:0,0,0\n100:255,255,255",
          "label": "Default value"
        }
      }
    },
    {
      "name": "Gradient Slider",
      "tag": "fds-gradient-slider",
      "split_value_num": 2,
      "split_value_type": "number",
      "icon": "<svg > ... Icon here...</svg>",
      "parameters": {
        "name": { "type": "text", "default": "gradient", "label": "Name" },
        "label": { "type": "text", "default": "Gradient", "label": "Label" },
        "default": {
          "type": "text",
          "default": "0;1.0",
          "label": "Default value"
        },
        "from_color": {
          "type": "text",
          "default": "black",
          "label": "From color"
        },
        "to_color": { "type": "text", "default": "grey", "label": "To color" },
        "min": { "type": "text", "default": "0", "label": "Min" },
        "max": { "type": "text", "default": "1.0", "label": "Max" },
        "value_type": { "type": "text", "default": "float", "label": "Type" },
        "num_handles": {
          "type": "text",
          "default": "two",
          "label": "Num Handles"
        }
      }
    }
  ]
}
```

<img src="../component1.png" width="400">


## Gyre API

Have a look at the Gyre API for documentation of the global API which is available [here](#). For that special component type, there are two individual parameters:

- `globalThis.gyre.workflowId` is set to the workflow-ID of the current open form.
- `formElements`: form elements of the open form including current values.
