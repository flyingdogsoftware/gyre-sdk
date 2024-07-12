# FDS Image Editor Components
![Gyre API](../fds_components.png)

The FDS Image Editor provides various custom components to facilitate image editing workflows. Below are the descriptions and usage examples of these components.

## Components

### `<fds-image-editor-progress-bar>`

This component displays an overall progress bar for the workflow execution and a sub-progress bar for the current running ComfyUI node.

#### Parameters
- None

#### Usage
This component is very useful and easy to use. It shows progress automatically.

```html
<fds-image-editor-progress-bar></fds-image-editor-progress-bar>
```

### `<fds-image-editor-toggle>`

This component represents a toggle switch.

#### Parameters
- **on:change**: Event handler for the change event. The new value can be accessed via `e.detail`.
- **value**: The boolean value of the toggle switch.

#### Note
- Do not provide the boolean value as a string.

#### Usage
```html
<fds-image-editor-toggle on:change={(e) => {
    let newValue = e.detail;
}} value={booleanValue}></fds-image-editor-toggle>
```

### `<fds-image-editor-button>`

This component represents a button. It can be customized with different properties like icon, title, state, device, type, and size.

#### Parameters
- **icon**: `string` - The name of the icon.
- **title**: `string` - The title of the button.
- **state**: `""|"active"|"disabled"` - The state of the button.
- **device**: `""|"desktop"|"iPad"` - The device type.
- **type**: `""|"button"|"icon"` - The type of the button.
- **size**: `string` - The size of the button.
- **on:click**: Event handler for the click event.

#### Usage Examples

##### Using icon only
```html
<fds-image-editor-button
    icon="background"
    title="Define Background"
    on:click={() => {
        // Define background action
    }}
></fds-image-editor-button>
```

##### Text Button
```html
<fds-image-editor-button
    type="button"
    on:click={() => {
        // Clear action
    }}
>
    Click me
</fds-image-editor-button>
```

#### Parameters
-  **icon**: `string` - The name of the icon.
-  **title**: `string` - The title of the button.
-  **state**: `""|"active"|"disabled"` -The state of the button.
-  **device**: ``""|"desktop"|"iPad"` - The device type.
-  **type**: `""|"button"|"icon"` - The type of the button which can be text or icon only.
-  **size**: `string` - Optional The size of the button.


### `<fds-image-editor-dialog>`

This component represents a dialog box.

#### Parameters
- **class**: `string` - Additional class for styling.
- **width**: `string` - The width of the dialog.
- **open**: `boolean` - Whether the dialog is open or closed.
- **onclosed**: `function` - Event handler for when the dialog is closed. Usually the variable used to open the dialog is set to false here.

#### Usage
```html
<fds-image-editor-dialog
    class="colorScheme"
    width="1000px"
    open={true}
    onclosed={() => {
        // Handle close event
    }}
>
    <span slot="modalcontent">
        <!-- Modal content goes here -->
    </span>
</fds-image-editor-dialog>
```

### `<fds-local-files>`

This component is used to load local files. In this example, it is configured to load `.glb` 3D files. The component is used in headless mode, making it invisible, which is the recommended mode. File loading can be triggered by calling the `load()` function on the component instance.

#### Parameters
- **description**: `string` - Description of the files to be loaded.
- **design**: `string` - Design mode of the component. Set to `headless` to make it invisible.
- **bind:this**: Binds the component to a variable.
- **on:load**: Event handler for the load event.
- **accept**: `object` - Specifies the file types that can be loaded.

#### Usage
```html
<fds-local-files 
    description="glb 3D files" 
    design="headless" 
    bind:this={fileLoader3D} 
    on:load={load3D} 
    accept={{ 'model/gltf_binary': ['.glb'] }} 
/>
```
#### Example

Below is a complete example of using the `<fds-local-files>` component in headless mode along with a button to load a `.glb` 3D model file.

```html
<script>
    let fileLoader3D;

    function load3D() {
        let binary = fileLoader3D.array_buffer;
        // Do something with binary file data here...
    }
</script>

<fds-local-files 
    description="glb 3D files" 
    design="headless" 
    bind:this={fileLoader3D} 
    on:load={load3D} 
    accept={{ 'model/gltf_binary': ['.glb'] }} 
/>

<fds-image-editor-button
    type="button"
    on:click={() => {
        fileLoader3D.load();
    }}
>
    Load Model...
</fds-image-editor-button>
```

#### Explanation

- The `<fds-local-files>` component is configured to load `.glb` 3D files and is used in headless mode, making it invisible.
- The `fileLoader3D` variable is bound to the `<fds-local-files>` component instance.
- When the button is clicked, the `load()` function is called on the `fileLoader3D` instance, triggering the file loading process.
- The `load3D` function is executed when the file is loaded, allowing you to handle the binary file data.

This setup allows for a clean and user-friendly interface where file loading is initiated through a button, while the actual file input element remains hidden.




## `<fds-image-editor-slider>`

This component represents a slider input that can be adjusted within a specified range. The slider can be configured to be vertical or horizontal.

### Parameters

- **min**: `string` - The minimum value of the slider.
- **max**: `string` - The maximum value of the slider.
- **vertical**: `boolean` - If `true`, the slider is displayed vertically.
- **on:input**: `function` - Event handler for the input event, triggered when the slider value changes.
- **value**: `number` - The current value of the slider.

With `window.fds_device = 'iPad'` globally iPad slider design is activated.

### Usage

```html
<fds-image-editor-slider
    min="2"
    max="200"
    vertical="true"
    on:input={(e) => {
        let newValue = e.target.value;
        // Handle the new value
    }}
    value={currentValue}
/>
```

### Example Usage

Below is a complete example of using the `<fds-image-editor-slider>` component with a vertical orientation and handling the input event.

```html
<script>
    let currentValue = 50;

    function handleInput(event) {
        currentValue = event.target.value;
        // Additional logic to handle the slider value change
    }
</script>

<fds-image-editor-slider
    min="2"
    max="200"
    vertical="true"
    on:input={handleInput}
    value={currentValue}
/>
```

### Explanation

- The `min` parameter sets the minimum value of the slider.
- The `max` parameter sets the maximum value of the slider.
- The `vertical` parameter, when set to `true`, displays the slider vertically.
- The `on:input` parameter binds an event handler to the input event, which is triggered whenever the slider value changes.
- The `value` parameter sets the current value of the slider.

This component allows for intuitive adjustment of values within a specified range, with support for both vertical and horizontal orientations.



<img src="../slider.png" width="400">

Same slider on iPad and desktop

