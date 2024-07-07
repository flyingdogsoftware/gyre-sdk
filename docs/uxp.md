# Gyre Plugins Development for Adobe UXP Developers

## Comparison Table

| Feature                        | Gyre SDK                    | Adobe UXP                    |
| ------------------------------ | --------------------------- | ---------------------------- |
| iPad support                   | X                           | -                            |
| Web Support                    | X                           | -                            |
| Standalone application support | X                           | X                            |
| UI Components                  | FDS Image Editor Components | Adobe Spektrum Components    |
| Actions                        | -                           | X                            |
| Workflows (ComfyUI)            | X                           | -                            |
| UI Plugins                     | X                           | -                            |
| Tool Plugins                   | X                           | -                            |
| Layer Plugins                  | X                           | -                            |
| Brush Plugins                  | X                           | -                            |
| Panel Plugins                  | -                           | X                            |
| Image data access              | X                           | X                            |
| Hybrid Plugins                 | Python                      | C++                          |
| Form Designer                  | X                           | -                            |
| Frameworks                     | all                         | all                          |
| API                            | Large API                   | Some basic functions         |
| AI Models management           | X                           | -                            |
| Test environment               | Test Env, Main Application  | Main Application             |
| Developer Support              | Discord, Developer Sessions | Adobe Forum, Developer Days, Preview program |

## Platform Overview

Our platform offers significantly more options for developing UI extensions with deeper integration, resulting in a wide variety of plugins. For instance, unlike Photoshop, our system allows for the development of new tools specifically for the image canvas. With the tight integration of ComfyUI, plugins can leverage powerful AI and non-AI features seamlessly.

## Plugin Development Process

Our plugin development process occurs in two stages:
1. **Creating a standalone project** with all plugin features.
2. **Integrating it within a ComfyUI extension** for distribution in the main application.

Additionally, we have improved aspects like icon handling or a larger API for a better user experience.

## Platform Choices

We chose not to support Panels due to their poor performance on smaller screens, such as the iPad. While Adobe relies on Creative Cloud for distribution, we utilize ComfyUI Manager as our primary distribution channel.
