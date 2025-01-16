# devcontainer

This project contains custom devcontainers for use in our repositories.


## Images

Image | Description | Dockerfile
-- | -- | -- 
`ghcr.io/home-assistant/devcontainer:addons` | For Add-on development | [./addons/Dockerfile](./addons/Dockerfile)
`ghcr.io/home-assistant/devcontainer:supervisor` | For Supervisor development | [./supervisor/Dockerfile](./supervisor/Dockerfile)

Versioned images are available with the custom devcontainer version prepended (e.g. `1-supervisor`). This loosly resembles what
upstream devcontainers are providing as well. The version is meant to be incremented when non-backwards compatible changes are
made. That allows existing devcontainer configuration to work while updating the devcontainers (e.g. when the Supervisor devcontainer
is updated to a new Python version).

## Example files

Example files to use with Visual Studio Code 

### addon

Example files for the `addons` devcontainer

- [Example configuration (for `.devcontainer/devcontainer.json`)](./addons/devcontainer.json)
- [Example tasks file (for `.vscode/tasks.json`)](./addons/tasks.json)



## Notes

### `addons` and `supervisor`

- Use the command `supervisor_run` to start Home Assistant inside the devcontainer, or run the task "Start Home Assistant" if you copied the tasks file.
- Use `ha` to use the custom Home Assistant CLI (Needs the supervisor to be running).

