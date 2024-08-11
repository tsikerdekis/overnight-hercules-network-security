Let's start with some of the basics, checking your Antivirus, Firewall, and open ports on your system.

# AntiVirus
There are many antiviruses out there, some are free and some are paid. In the guide I cover two:
- Windows Defender (Free) (comes pre-installed with Windows)
- Avast (Free and Paid) (https://www.avast.com/)

## Configuring your Antivirus (Windows Defender)
Give me a set of instruction on checking the settings of windows defender so that it is enabled and they are the best security up to date etc.

1. Open the Windows Security app on your computer.
2. Click on the "Virus & threat protection" tab on the left-hand side of the window.
3. Under the "Virus & threat protection settings" section, make sure that the "Real-time protection" toggle switch is turned on.
4. Click on the "Manage settings" link under the "Virus & threat protection settings" section.
5. In the "Real-time protection" settings, make sure that the "Cloud-delivered protection" and "Automatic sample submission" options are turned on.
6. Go back to the main Windows Security window and click on the "Firewall & network protection" tab on the left-hand side.

## Configuring your Antivirus (Avast)

1. Open Avast Antivirus on your computer.
2. Click on the "Menu" button in the top-right corner of the Avast interface.
3. From the dropdown menu, select "Settings".
4. In the Settings window, you will find various categories on the left-hand side.
5. Click on the category that corresponds to the settings you want to check (e.g., General, Protection, Privacy, etc.).
6. Within each category, you will see different options and settings that you can customize.
7. Review and modify the settings according to your preferences.
8. Once you have made the desired changes, click on the "OK" or "Apply" button to save the settings.
9. You can now close the Avast Antivirus settings window.

Note: The exact steps and options may vary depending on the version of Avast Antivirus you are using. It's always a good idea to refer to the Avast documentation or support resources for more specific instructions.

# Firewall
I cover two firewalls in this guide:
- Windows Firewall (comes pre-installed with Windows)
- Malwarebytes Windows Firewall Control (Free) (https://forums.malwarebytes.com/topic/296798-malwarebytes-windows-firewall-control-wfc/)

## Windows Firewall
The Windows Firewall is a built-in security feature that helps protect your computer from unauthorized access over the network. Although often not appreciated as much, it is a decent peace of software to use.

Here's how you can check and configure your Windows Firewall settings:

1. Open the Control Panel on your computer.
2. Click on "System and Security".
3. Click on "Windows Defender Firewall".
4. In the left-hand pane, click on "Allow an app or feature through Windows Defender Firewall".
5. Click on the "Change settings" button (you may need to provide administrator permission).
6. Scroll through the list of apps and features and make sure that the checkboxes next to the apps you want to allow through the firewall are checked.
7. If you want to add a new app or feature to the list, click on the "Allow another app..." button and follow the on-screen instructions.
8. Click on the "OK" button to save your changes.

## Malwarebytes Windows Firewall Control
Malwarebytes is a popular security software that offers a range of features, including a firewall. Under the hood, it still uses the core components of Windows Firewall, but it adds an extra layer of protection and customization. The most important feature is the ability to individually authorize applications that you want to allow accessing the internet.

Here's how you can check and configure the firewall settings in Malwarebytes:

1. Open the Malwarebytes Windows Firewall Control application on your computer.
2. Set the profile to "Medium Filtering" for a good balance between security and usability. This will block most outgoing communications with the internet unless you authorize them.
3. Go to "Rules" in the left-hand pane and set direction to Outbound.
4. Under "Notifications", you can choose to be notified when an application tries to access the internet. This can be useful to detect suspicious behavior. Set to "Display Notifications" for this.
5. Now, when a new application wants to access the internet you should see a notification in the lower right corner of your computer. You have a choice to allow or block the application. Beyond that you can click on the additional information about the application (e.g., is it a signed Windows program, etc.).
6. Each time you allow an application a Windows rule is created. If you want to revert this, you can go to the "Manage Windows Firewall Rules" (it's the bottom left icon in the menu) to delete an rules that you don't like.

# Open Ports
Open ports are like open doors to your computer. They allow communication between your computer and the internet. It's important to know which ports are open on your system and to close any unnecessary ones to prevent unauthorized access.

You can use netstat to identify what are the open ports on your computer. Here's how you can do it:

## Windows
1. Open a command prompt on your computer.
2. Type the following command and press Enter:
```netstat -an```
3. This will display a list of all open ports on your computer along with the corresponding IP addresses and protocols.
4. Look for the "LISTENING" ports, as these are the ones that are actively accepting connections.
5. These are your open ports. It can be scary to see many of them but rest assured, most likely these are not accessible from the internet. We'll talk more about this in a later chapter but is a useful tool that you can use to frequentently review or investigate if you suspect something fishy is going on.