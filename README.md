# devcontainer

This project contains custom devcontainers for use in our repositories.


## Images

Image | Description
-- | --
`ghcr.io/home-assistant/devcontainer:addons` | For Add-on development
`ghcr.io/home-assistant/devcontainer:supervisor` | For Supervisor development

 

## Example files

Example files to use with Visual Studio Code 

### addon

Example files for the `addons` devcontainer

- [Example configuration (for `.devcontainer/devcontainer.json`)](./addons/devcontainer.json)
- [Example tasks file (for `.vscode/tasks.json`)](./addons/tasks.json)



## Notes

### `addons` and `supervisor`

- Use the command `supervisor_run` to start Home Assistant inide the decontainer, or run the task "Start Home Assistant" if you copied the tasks file.
- Use `ha` to use the custom Home Assistant CLI (Needs the supervisor to be running).

