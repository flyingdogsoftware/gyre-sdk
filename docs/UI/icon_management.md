# Icon Management
![Icon Management](../icons.png)

Each plugin can provide different icons which can be used in templates by the `fds-image-editor-button` component. To make things easier, these icons have to be provided in the manifest of each plugin. This ensures that inside the templates, there is no unnecessary icon code which can bloat the entire project, especially if an icon is used multiple times.

## Icon Format

The format of an icon is simple:
- **body**: The SVG body of the icon.
- **width**: The original width (viewBox in SVG tag).
- **height**: The original height (viewBox in SVG tag).

Each icon gets the plugin tag automatically as a prefix so such icons can be used from anywhere without overwriting each other. For example, in the `fds-image-editor-sam` plugin, the `eye` icon can be used by the `fds-image-editor-sam-eye` name only.
