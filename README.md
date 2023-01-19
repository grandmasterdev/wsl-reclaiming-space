# WSL reclaiming disk space

WSL does not release storage that has been claimed by an app/file/service when they are deleted from the sub-system. Over time, the disk space will be filled up. User will need to do the following to re-claim the lost/used storage manually.

## Windows 10 Home

```powershell
wsl --shutdown
diskpart

# open window Diskpart
select vdisk file="C:\WSL-Distros\...\ext4.vhdx" # usually at $env:LOCALAPPDATA\Packages\Canonical...
attach vdisk readonly
compact vdisk
detach vdisk
exit

# run wsl again
wsl
```

## Windows 10 Pro

```powershell
# optimize (shrink) WSL 2 .vhdx
# must run in `PowerShell as Administrator` user
# distro folder can be found at $env:LOCALAPPDATA\Packages\
# examples:
# 	CanonicalGroupLimited.Ubuntu20.04Windows_79rhp1fndgsc

cd $env:LOCALAPPDATA\Packages\REPLACE_ME_WITH_TARGET_DISTRO_FOLDERNAME\LocalState

wsl --shutdown

optimize-vhd -Path .\ext4.vhdx -Mode full

# run wsl again
wsl
```
