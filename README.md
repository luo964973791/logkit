```javascript
mkdir -p /data/logkit && cd /data/logkit
wget https://pandora-dl.qiniu.com/logkit_centos_v1.5.5.tar.gz
tar zxvf logkit_centos_v1.5.5.tar.gz && mv _package package

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


systemctl start logkit
```
