# devcontainer

This project contains custom devcontainers for use in our repositories.

## addons

This devcontainer contains all you need to develop add-ons.

Usefull for:
  - Add-on development

**Image** | `ghcr.io/home-assistant/devcontainer:addons`
--|--
**Dockerfile**| [./addons/Dockerfile](./addons/Dockerfile)
**Example configuration (for `.devcontainer/devcontainer.json`)**| [./addons/devcontainer.json](./addons/devcontainer.json)
**Example tasks file (for `.vscode/tasks.json`)**| [./addons/tasks.json](./addons/tasks.json)

Notes:

- Use the command `supervisor_run` to start Home Assistant inide the decontainer, or run the task "Start Home Assistant" if you copied the tasks file.
- Use `ha` to use the custom Home Assistant CLI (Needs the supervisor to be running).

***