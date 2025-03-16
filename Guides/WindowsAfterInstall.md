# This Sets Up Windows After Reinstalling Windows

## Setup Rclone using NSSM
```bash
# Setup Directories for Rclone mount
mkdir -p c:\rclone\mountcache
# Download Rclone
cp -r P:\WindowsFiles\Rclone\rclone.conf C:\Users\aaxyat\AppData\Roaming\rclone\rclone.conf
# NSSM Installation
sudo nssm install rclone C:\Users\aaxyat\AppData\Local\Microsoft\WinGet\Links\rclone.exe && C:\Users\aaxyat\AppData\Local\Microsoft\WinGet\Links\nssm.exe set rclone AppParameters "mount --config C:\Users\aaxyat\AppData\Roaming\rclone\rclone.conf --cache-dir c:\rclone\mountcache --attr-timeout 8700h --use-mmap --stats 10s --buffer-size 32M --vfs-cache-poll-interval 30s --poll-interval 30s --dir-cache-time 8700h --vfs-cache-max-age 4h --async-read=true --drive-server-side-across-configs --vfs-cache-mode full --vfs-read-chunk-size 32M --vfs-read-chunk-size-limit 1G --local-no-check-updated --drive-chunk-size=64M --multi-thread-streams=4 --multi-thread-cutoff 250M --transfers 8 --drive-disable-http2=true --fast-list --timeout 30m --vfs-case-insensitive --vfs-cache-max-size 20G --log-level ERROR --drive-pacer-min-sleep 10ms --drive-pacer-burst 200 --log-file=\"c:\rclone\rclonelogplex.txt\" --no-console --rc --rc-addr localhost:5573 \"crypt:\" X: --network-mode" && C:\Users\aaxyat\AppData\Local\Microsoft\WinGet\Links\nssm.exe set rclone AppDirectory C:\Users\aaxyat\AppData\Local\Microsoft\WinGet\Links && C:\Users\aaxyat\AppData\Local\Microsoft\WinGet\Links\nssm.exe set rclone DisplayName rclone && C:\Users\aaxyat\AppData\Local\Microsoft\WinGet\Links\nssm.exe set rclone ObjectName LocalSystem && C:\Users\aaxyat\AppData\Local\Microsoft\WinGet\Links\nssm.exe set rclone Start SERVICE_AUTO_START && C:\Users\aaxyat\AppData\Local\Microsoft\WinGet\Links\nssm.exe set rclone Type SERVICE_WIN32_OWN_PROCESS

```
