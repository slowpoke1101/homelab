Configure nodes to send logs → scilab

Each node forwards syslog to TK’s IP on port 514.

    Mimi (OPNsense): Logging Targets → TK IP → UDP 514

    Sora (Proxmox): *.* @TK_IP:514 in /etc/rsyslog.conf

    Joe (TrueNAS): Syslog → TK IP

    Docker VM: *.* @TK_IP:514

    Tai (GS308E): Syslog → TK IP

    Flint APs: Syslog → TK IP (if supported)