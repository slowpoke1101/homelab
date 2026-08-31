
1. Connect Flint2 and Access Its UI

    Plug Flint2 into a switch port configured with:

        PVID 1

        Tagged VLANs 88 and 99

    Locate Flint2’s IP address in the OPNsense UI.

    Navigate to Flint’s web UI.

    Go to Network > Network Mode and choose Access Point.
    Flint will now receive DHCP from OPNsense and its IP will change.

<img src="images/WAPmode.png" width="50%">

    Navigate to Flint’s new IP.

    Go to System > Advanced Settings and install LuCI.

    Open the LuCI interface and log in.

2. Create VLAN Devices

    In LuCI, go to Network > Interfaces.

    Select the Devices tab.

<img src="images/navigationinterfaces.png" width="50%">
<img src="images/navigationdevices.png" width="50%">

    Click Add device configuration.

<img src="images/adddevice.png" width="50%">

    Set:

        Device Type: VLAN (802.1q)

<img src="images/adddevice2.png" width="50%">

    VLAN ID: 88 (or whatever tag you need)

<img src="images/adddevice3.png" width="50%">

    Click Save, then Save & Apply.

3. Create Interfaces for Each VLAN

    Go back to the Interfaces tab.

    Click Add new interface.

<img src="images/addinterface.png" width="50%">

    Set:

        Name: IOT99 (example)

        Protocol: Unmanaged

<img src="images/addinterface2.png" width="50%">

    Device: Software VLAN br-lan.99

<img src="images/addinterface3.png" width="50%">

    Click Create interface.

4. Configure Bridge VLAN Filtering

    Return to the Devices tab.

    Find br‑lan and click Configure.

    Open the Bridge VLAN Filtering tab.

    Enable Bridge VLAN Filtering.

    Add VLANs as needed.
    Example for LAN1 input:

        VLAN 1: untagged

        VLAN 88: tagged

        VLAN 99: tagged

<img src="images/bridgedVLAN.png" width="50%">
5. Create Wi‑Fi SSIDs for Each VLAN

    Go to Network > Wireless.

<img src="images/navigationwireless.png" width="50%">

    Find the 5 GHz radio and click Add.

<img src="images/addwireless.png" width="50%">

    Set:

        ESSID: IOT99

        Network: IOT99

<img src="images/addwireless2.png" width="50%">

    Go to the Wireless Security tab.

    Choose encryption (example: WPA2‑PSK).

    Enter your Wi‑Fi password.

<img src="images/addwireless3.png" width="50%">

    Click Save.