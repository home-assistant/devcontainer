# devcontainer

This project contains custom devcontainers for use in our repositories.


## Images

Image | Description | Dockerfile
-- | -- | -- 
`ghcr.io/home-assistant/devcontainer:apps` | For Home Assistant App development | [./apps/Dockerfile](./apps/Dockerfile)
`ghcr.io/home-assistant/devcontainer:supervisor` | For Supervisor development | [./supervisor/Dockerfile](./supervisor/Dockerfile)

Versioned images are available with the custom devcontainer version prepended (e.g. `1-supervisor`). This loosly resembles what
upstream devcontainers are providing as well. The version is meant to be incremented when non-backwards compatible changes are
made. That allows existing devcontainer configuration to work while updating the devcontainers (e.g. when the Supervisor devcontainer
is updated to a new Python version).

## Example files

Example files to use with Visual Studio Code 

### Apps

Example files for the `apps` devcontainer

- [Example configuration (for `.devcontainer/devcontainer.json`)](./apps/devcontainer.json)
- [Example tasks file (for `.vscode/tasks.json`)](./apps/tasks.json)



## Notes

### `apps` and `supervisor`

- Use the command `supervisor_run` to start Home Assistant inside the devcontainer, or run the task "Start Home Assistant" if you copied the tasks file.
- Use `ha` to use the custom Home Assistant CLI (Needs the supervisor to be running).

### AppArmor

If the host kernel supports AppArmor, it is automatically active inside
the devcontainer for the Supervisor and apps. The `hassio-supervisor`
profile is downloaded and loaded on first boot. This allows apps
developers to develop and test AppArmor profiles within the devcontainer
environment.

AppArmor denials are logged to the kernel ring buffer and can be viewed
with `dmesg` or `journalctl -k`. Note that `auditd` cannot run inside
the container due to missing permissions on the host kernel's audit
subsystem. For full audit logging, run `auditd` on the host OS directly.

**Host kernel considerations:** The `apparmor` package inside the
container ships default policies which may prohibit D-Bus communication,
potentially interfering with the Supervisor and apps. Additionally, the
host kernel's AppArmor feature set can lead to different behavior of
profile enforcement. For example, Ubuntu kernels may enable AppArmor
features that are not present on other distributions, which can affect
how profiles are applied.

To disable AppArmor for the Supervisor, set `SUPERVISOR_UNCONFINED` in
your `containerEnv`:

```json
"containerEnv": {
    "SUPERVISOR_UNCONFINED": "1"
}
```

This causes the Supervisor container to run with `apparmor=unconfined`
instead of the `hassio-supervisor` profile.

