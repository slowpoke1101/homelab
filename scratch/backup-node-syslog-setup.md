#setting up rpi4 backup node as syslog server as well
sudo apt update
sudo apt install rsyslog

##edit /etc/rsyslog.conf /etc/rsyslog.d/remote.conf
# Enable UDP syslog reception
module(load="imudp")
input(type="imudp" port="514")

# Enable TCP syslog reception (optional)
module(load="imtcp")
input(type="imtcp" port="514")

# Store incoming logs
*.* /var/log/remote.log

#restart
sudo systemctl restart rsyslog

OPNsense (Mimi)

    Go to System → Settings → Logging / Targets.

    Add TK’s IP, port 514, protocol UDP.

    Choose “Everything” or specific facilities.

TrueNAS (Joe)

    Go to System → Advanced → Syslog.

    Enter TK’s IP, port 514.

Proxmox (Sora)

    Edit /etc/rsyslog.conf:
    Code

    *.* @TK_IP:514

    Restart rsyslog.

GS308E (Tai)

    In the web UI, enable syslog and point to TK’s IP.
