# Install WSL2 on Windows (powershell)

```shell
wsl --list --online
wsl --install -d Ubuntu-20.04 #install ubuntu distro
wsl -l -v
```
# Delete wsl distro
```shell
wsl --terminate Ubuntu-20.04
wsl --unregister Ubuntu-20.04
```