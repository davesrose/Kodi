# Notes on accesing KODI on Amazon FireTV

First it's best to find FireTV's IP in its settings. arp -a will list all your network device IPs in terminal, but that could still be confusing if you have multiple Amazon devices. Run: adb connect [IP Address]. For navigation on the Fire, you run adb shell. You can copy local files with adb push. Below is an example of me using adb to autostart the plugin plex:

## Creating local autostart files

Examples are in this folder. There are general guides for how to create an autostart of addons here:
[a link](https://kodi.wiki/view/Autoexec_Service) . You create an addon.xml file to point to your .py. For my need of running an addon, the .py is simple

addon.xml:

```xml
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<addon id="service.autoexec" name="Autoexec Service" version="1.0.0" provider-name="your username">
	<requires>
		<import addon="xbmc.python" version="3.0.0"/>
	</requires>
	<extension point="xbmc.service" library="autoexec.py">
	</extension>
	<extension point="xbmc.addon.metadata">
		<summary lang="en_GB">Automatically run python code when Kodi starts.</summary>
		<description lang="en_GB">The Autoexec Service will automatically be run on Kodi startup.</description>
		<platform>all</platform>
		<license>GNU GENERAL PUBLIC LICENSE Version 2</license>
	</extension>
</addon>
```

autoexec.py:

```py
import xbmc

xbmc.executebuiltin('RunScript(script.plexmod)')
```

## Transferring to FireTV

Per Kodi Wiki, you first create a folder in your addons directory. For my version of FireTV, run:

```bash
adb shell
cd /sdcard/Android/data/org.xbmc.sled/files/.kodi/addons/
mkdir service.autoexec

exit shell (ctrl + c)
```

Then push your local files to the service.autoexec folder:

```bash
adb push '/Users/[user]/Dev/Kodi/service.autoexec/addon.xml' '/sdcard/Android/data/org.xbmc.sled/files/.kodi/addons/service.autoexec'
adb push '/Users/[user]/Dev/Kodi/service.autoexec/autoexec.py' '/sdcard/Android/data/org.xbmc.sled/files/.kodi/addons/service.autoexec'
```
