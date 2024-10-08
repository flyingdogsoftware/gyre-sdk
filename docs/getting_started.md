# Getting Started with the Gyre SDK
![Getting Startet](start.png)

Before implementing a plugin, please identify your focus. Here are the options:

- **Extending via ComfyUI Workflow**: No programming needed. Use our form builder to directly design dialogs for the main application. These can encompass AI-based or non-AI image operations. There are hundreds of nodes and thousands of workflows for various AI applications available, which can be used directly with just a few steps. Many are already integrated as so-called default workflows. For a quick start, please follow our official YouTube channel.
- **Developing a Plugin for Gyre**: This can include Tools, UI Components, new Layer Types, or new Brushes. This SDK covers all aspects of this development only.
- **Extending ComfyUI with Python-based Nodes**: These nodes can be utilized in any workflow.
- **For Adobe UXP developers**: Check out our comparison page to understand the key differences.

With the exception of brushes, all plugins are made using Web Components. This means you will use custom tags in the main application. Choose a good prefix for these tags, such as your organization’s name (e.g., myproject-my-tool). The prefixes “fds-” and “gyre-” are reserved for our own extensions.

Depending on the plugin type, each requires a different manifest and support for various callback functions within your Web Component. These Web Components can be created in any framework, though our examples use Svelte.

## How It Works

The Gyre middleware scans all other extension folders for a subfolder named `gyre_entry`. Installing a Gyre extension follows the same process as any other ComfyUI extension, whether through the Manager, `git clone`, or unzipping. These can be added to existing ComfyUI extensions. Each ComfyUI extension can handle ComfyUI custom nodes, default workflows, and one or more Gyre plugins.

## Supported Plugin Types

- **UI Elements**: These will appear in the "Add Element" menu of the form editor in the middleware.
- **Custom Layer Types**: Capable of hosting complex applications. Our internal 3D or Pose layers use this type of extension.
- **Image Canvas Tools**
- **Custom Brushes**

The main Gyre application loads all manifest definitions for these plugins from the ComfyUI extensions. Using the `js_path` parameter, it also loads the JavaScript files for each plugin. Consequently, the manifest files and the plugin executables are stored in separate locations.

## Setting Up Your Own Extension/Plugins

1. Develop custom elements with their own tags using the Shadow DOM, ensuring they are isolated from the main application. Use a unique prefix to avoid conflicts (e.g., "yourname-yourcompany-gradient-slider" instead of "gradient-slider").
2. Create your folder under `custom_nodes` like any other ComfyUI extension.
3. Add a subfolder `gyre_entry` and add manifest there.
4. Bundle the JS executables with ComfyUI extension (usually as node modules in sub path below `gyre_entry`).

<img src="extension.png" width="500">
