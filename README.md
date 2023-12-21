```javascript
cat <<EOF | sudo tee /lib/systemd/system/logkit.service
[Unit]
Description=Logkit Daemon
After=network.target

[Service]
Type=simple
#NotifyAccess=all
PrivateTmp=true
WorkingDirectory=/data/logkit/package
User=root
Group=root

ExecStart=/data/logkit/package/logkit -f /data/logkit/package/logkit.conf
StandardOutput=journal
StandardError=inherit
LimitNOFILE=65535
LimitNPROC=4096
LimitAS=infinity
LimitFSIZE=infinity
TimeoutStopSec=0

KillSignal=SIGTERM
KillMode=process

SendSIGKILL=no

SuccessExitStatus=143
TimeoutStartSec=900

[Install]
WantedBy=multi-user.target
EOF
```
