This guide consists of two sections:
1. [Evaluating security settings of network devices](#networkdevices)
2. [Setting up network security monitoring in your network](#nsm)

<a name="networkdevices"></a>
## 1. Evaluating security settings of network devices
This section will guide you through the process of evaluating the security settings of network devices.

### Router
This guide will help you find your router's gateway (default IP address) using command-line tools and access it via your web browser. As an example I cover how to log in to an Xfinity router but others are fairly similar.

#### 1. Open Command Prompt or Terminal
- **Windows**: Press `Windows + R`, type `cmd`, and press Enter.
- **Mac/Linux**: Open the **Terminal** from your Applications or search for it.

#### 2. Find the Gateway Address

In the command prompt, type the following command and press Enter:
```ipconfig``` (Windows) or ```ifconfig``` (Mac/Linux)

Look for the **Default Gateway** under your network adapter. This is your router's IP address. In Linux, it's usually under `inet addr`.

An alternative approach is to look for your routing settings on your computer.

In Windows, type:
```route PRINT```
Look for IPv4 Route Table and find the Gateway address at the top usually.

In Mac/Linux, type:
```route -n``` or ```ip route show```
Look for the Gateway address under the `Destination` column.

Example (output may vary):
```
IPv4 Route Table
===========================================================================
Active Routes:
Network Destination        Netmask          Gateway       Interface  Metric
          0.0.0.0          0.0.0.0      192.168.0.1    192.168.0.209     45
        127.0.0.0        255.0.0.0         On-link         127.0.0.1    331
        127.0.0.1  255.255.255.255         On-link         127.0.0.1    331
     172.22.112.0    255.255.240.0         On-link      172.22.112.1   5256
     172.22.112.1  255.255.255.255         On-link      172.22.112.1   5256
```

In this example, the Gateway address is `192.168.0.1`.

#### 3. Access the Router
Using the router's IP address, open a web browser and type it in the address bar (e.g., 192.168.0.1). Then press Enter.

You will be prompted to enter a username and password. The default credentials are usually found on the router itself or in the manual. Common defaults are `admin` for both username and password, or `password` for password. You may have to google search for your router's default credentials (e.g., "Xfinity router default login").

After successfully logging in, you can navigate through the router's settings to evaluate its security settings.

#### 4. Evaluate WiFi Security
Find the WiFi settings (e.g., Connection -> WiFi) and check the following:
- **Security Type**: Use WPA2 or WPA3 for better security.
- **Password**: Ensure a strong password is set.
- **Guest Network**: Disable if not needed. But consider enabling it if you have many guest as it isolates them from your main network.

#### 5. Connected Devices
Look for a section that shows connected devices. Ensure you recognize all devices connected to your network. For example look at the following image:
[![Connected Devices](img/connected-devices.png)](img/connected-devices.png)

You can see the devices are either recognized by name, or shown with their MAC address. The associated IP provided by the router's DHCP is also shown. Devices that are permanent in your network should have a static IP assigned. You can do so in the device but it is better to do so on the router since this way the router's DHCP will not assign the IP to another device.

In the above image, if you click Edit, you can assign a static IP (also referred to as Reserved IP) to the device.

#### 6. Remote Management
Look for a setting that allows remote management of the router. This should be disabled unless you need it. Remote management allows you to access the router's settings from outside your network. If enabled, ensure it is secured with a strong password. Example in the following image:
[![Remote Management](img/remote-management.png)](img/remote-management.png)

#### 7. DMZ
The DMZ (Demilitarized Zone) setting allows you to expose a device to the internet. This should be used with caution and only if you know what you are doing.

#### 8. Device Discovery (UPnP)
Universal Plug and Play (UPnP) allows devices to discover each other on the network. This can be a security risk as it opens ports on the router. It is recommended to disable this unless you need it for specific applications.

<a name="nsm"></a>
## 2. Setting up network security monitoring in your network

There are several ways to setup network security monitoring in your network especially for following the subsequent chapters of the book. I have a few options but it will highly depend on the degree of time you want to put in and the available resources.

If you just want to practice along with the book, I recommend the second option. This will give you a flavor by allowing you to monitor your computer. You can always come back to the first option later if you want to make things more interesting and monitor your whole network.

- **Option 1**: [Full Hardware Solution (Difficult)](../chapter_02/full-hardware-solution.md)
- **Option 2**: [Running locally on prefabricated VM (Easy)](../chapter_02/running-locally-on-prefabricated-vm.md)

<a name="nsm-hardware"></a>
