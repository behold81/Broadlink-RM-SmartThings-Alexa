﻿# Read the main <a href='https://beckyricha.github.io/Broadlink-RM-SmartThings-Alexa/index.html'>readme</a> for this repository first!
This document contains the setup instructions for the SmartApp version that uses the SmartThings hub for local LAN control and the app "RM Bridge" as a bridge.

## Set-up your Broadlink RM:
I used the app designed for this device by its manufacturer (<a href='https://play.google.com/store/apps/details?id=com.broadlink.rmt&hl=en'>e-Control</a>, available in the play store) to set it up.  I only use this app for this setup but it can be installed and run on multiple devices if you also want to control things using the app directly.  It's not bad on its own.  My Broadlink arrived with a manual only in Chinese, but install the e-Control app and follow its instructions to add your broadlink RM device to your wifi network. Don’t worry - the app is in English.  I did not take detailed notes during this section and mine is already set up, but it was like many other apps.  You can also add devices in the Broadlink e-Control app as you wish (for instance if you also want to use that app for device control), but that does not affect anything in the RM Bridge app or in SmartThings if you are using this version of my code.  
 
## Set up your Android bridge:
Install the RM Bridge app, change any of the settings you wish and press the red circle where it says "stopped" to start the service. This code will only work while this service is running. Note the ip address and port where the bridge is operating, as well as your user name and password if you are using them.  It will be most reliable if you can set your android up with a static ip address, but that is beyond the scope of this tutorial (it can vary for different android devices but is usually accessible under wifi settings by long pressing the name of your wifi network and selecting the advanced options).  

## Add some devices to control:
For this to work, you need to record devices directly using a web portal that is designed for setting up devices with the "RM Bridge" app.  You might want to read through this whole section before trying one, as both the concept and the steps are important.  To make devices work with this app, you need to set them up in a very specific way.  

We are going to add devices here and assign them functions that happen when you turn them on and when you turn them off in smartthings. This doesn't need to actually be the power button on your remote.  Let's walk through the steps, and then I will explain some creative ways to use them afterward.  

1. From a device on the same network as your Broadlink and bridge, go to http://rm-bridge.fun2code.de/rm_manage/index.html
2. Click the "manage codes" button with the wrench icon
3. Click "create new codes"
4. Enter the local IP and port that the RM bridge app shows when you start it up, and click "load devices." (note - if you added security in the RM Bridge settings, you will need to enter your user name and password when prompted.)
5. Select the desired Broadlink device when it comes up, noting that you need to record codes to the same Broadlink that you will want to send the commands from (for instance if you have them in more than one room).  If your devices shows up in Chinese characters, you can change their names in the e-control app to make this easier.
6. Enter the device name in the box for that on the web page, with some very specific rules.  For my device handler to work, the device name is not optional as it says here.  This is what I will hopefully be able to skip with a smart app, but for this quick version you still have to learn and specifically name codes for on and off, as follows: Name of device exactly as it will appear in smartthings (can include spaces if you want), then a space (important), then either the word "on" or "off" (lower case matters for the words on and off).  Devices are not required to have both on and off commands recorded, but you do have to record separately whichever you want to work, even if both use the same button such as a toggle switch.  See the end of this section for some creative ways to use this feature.
7. Learning the code works like it did in the e-Control app, if you played with that durig setup.  There is a different approach for RF or IR device.  For an IR device bring your remote close to the Broadlink, click "learn code" and then preess the button you want to learn.  The web site will indicate success or not.  For an RF device, bring the remote close the the broadlink, click "frequency scan" and hold down the remote button for several seconds, until the indicator on the top right side stops spinning.  This is not compleltely reliable and may need to be repeated.   After this records, click "learn code" and press the button you want to learn.  If an RF device is being stubborn about accepting the code it sometimes helps to go back out to the previous web page and go back in to start over.  It can also help to wait a second or two after pressing the web site's button and before pressing a remote button.  If this gets frustating, the RM Tasker App version does record codes more reliably as you can use the e-Control app that came with the Broadlink.
8. Repeat steps 6 and 7 for all the devices you want to add.

Here's where some creativity can help, now that you've seen what the steps are.  Even though these are labeled as devices and their on and off switches, they don't need to physically be exactly that.  A "device" can be any action you want to put into a smartthings scene or action, or to trigger with the echo by saying "Alexa turn on _____" or "start _____".  It sometimes even responds to "switch to ________" which can be very natural.  For example, you could have something easy like a light switch or outlet controller that does have an on switch and an off switch.  You could also have a TV, where on and off are the same button, but you need to program it in twice, once with the name "on" and once with the name "off" if you want it to respond to both commands.  

The following examples are written the same way for all 3 versions, but recording devices that include multiple remote keys (e.g. etering sveral numbers on your TV remote to "turn on" a channel) requires additional work if using the rm bridge app, described at the bottom of this readme. RM Tasker does this easily but has other trade-offs. 

You could get creative with your device definitions and add any number of things in a way where Alexa can control them.  If you set up a device called ESPN, and its "on button" is programmed to the keystrokes for that channel, your Alexa can respond to "turn on ESPN."  "Turn on the television" for me turns on the TV,  remote HD sender I have and switches the HDMI to the proper input. "Turn to the other input" activates my HDMI switch, or you could program the keystrokes needed on your TV remote.  I programmed in a single fan remote as 3 devices: "low fan," "medium fan" and "high fan."  Each one has its corresponding button set to "on" and the overall remote's "off" switch assigned to all 3.  I also made one just called "fan" and set its off switch as well.  Now I can tell Alexa to turn on the "low fan" or just to turn off the fan.  The possibilities are endless.

## Install this app into SmartThings (finally):
1. Go to [Smart Things IDE] (https://graph.api.smartthings.com/) and log in. You may need to do this on a computer.  I have difficulty getting it to work right browsing on a phone.
2. Click "my device handlers" and on the page that leads to,  click "create new device handler."
3. Ignore all the default information that comes up, and select the tab that says "from code."
4. Paste in the text from the file in my github called <a href='https://github.com/beckyricha/Broadlink-RM-SmartThings-Alexa/blob/master/Broadlink%20RM%20Bridge%20Switch%20LAN'>"Broadlink RM Bridge Switch LAN."</a>
5. On line 46, where it reads def url1="xxx.xxx.x.x:xxxx" you need to manually overwrite the x's with your local ip address and port from the RM Bridge app.  This will probably look like 192.168.1.2:7474 or something similar with your specific numbers.  Do not include the "http://"  If you entered a password in the RM bridge app, you need to add those in place of username:password on line 47 (leave the quotes in place).  If you did not set up authentication, just leave this line alone, wihtout deleting this dummy text.  
5. Click the "create" button. 
6. At the top of the code, select "publish" and "for me."

## Add your devices to SmartThings
For each device you want to add, perform the fllowing steps while still in the smartthings IDE (the web site where you added the code):
1. Click "my devices" near the top of the screen.
2. Click "New Device"
3. Make the Device ID the exact same name as you entered in the web site when you were setting up the code learning, but do not follow it with a space or the word "off" or "on" as you did there (note that this must be unique - if this doesn't work for you, you will need to change the deviceID referencec in the code to whichever other device field you want to use). You can either make the device's  name and label match that, or call it something else.  I believe label is optional.  Either the name, or label if present, is what SmartThings and Alexa will use to control your device (i.e. when you say Alexa, turn on [device name/label]).  Having this differ from the device ID may be useful, for example if you have a remote controlled plug that you attach different things to (like I do at Christmas).  Your RM Bridge name and device ID might be something boring like outlet1, but you could easily change the label to "Christmas Tree", "Living room lamp" etc in the SmartThings app withut having to repeat this setup.  (hat tip to user <a href='https://github.com/itsamti'>itsamti</a> for making this suggestion and coding the change, along with upgrades to the switch controls in the ST app).
5. In the dropdown for device type, select "RM Bridge Switch LAN"
6. Make sure to select your location and hub. this uses your hub so needs to be connected.
7. Click "create"

Your device should now appear in the SmartThings app and work properly, and you can import it into Alexa for use there.

## Adding devices that need multiple keys
RM Bridge was not set up to directly use multiple keys.  To make this work, I set up a separate device handler for these, called <a href='https://github.com/beckyricha/Broadlink-RM-SmartThings-Alexa/blob/master/RM%20Bridge%20Switch%20LAN%20Multikey'>"RM Bridge Switch LAN Multikey."</a>  I would only use this one for devices that actually need multiple keystrokes as it is slower and less reliable than the main code.  I set it up to take up to 4 keystrokes.  It is installed just as the other device handler above was, including manually changing the ip, port and authorization information.  The device codes are also recorded the same way, but now their names are "device name on" "device name on2" "device name on3" and "device name on4", with the same pattern for any "off" sequence.  If any are not recorded it just sends a command that does nothing, but does not cause any errors.  If you need more or fewer keys, change the lines of code at 36 and 40.  Note that the number 1000 is milliseconds between commands.  I found that my hub needs this long to clear and accept another command, but you may need to tweak this depending on how yours is responding.  When you create the smartthings device, this is also as decribed above, but you need to select this device handler for your device type.  If this does not work well, I have also developed a cloud-based version that may work more reliably if you want to open a router port. I don;t routinely usse this one, so please start an issue if you have trouble and I'll try some things that may improve it.  I don't plan to do this until someone needs it.

<script src="//z-na.amazon-adsystem.com/widgets/onejs?MarketPlace=US&adInstanceId=316d030e-54f2-4085-bbc4-5ba45c996661&storeId=seniorhacks-20"></script>
<p><small>I am a participant in the Amazon Services LLC Associates Program, an affiliate advertising program designed to provide a means for me to earn fees by linking to Amazon.com and affiliated sites.</small></p>
<script>
  (function(i,s,o,g,r,a,m){i['GoogleAnalyticsObject']=r;i[r]=i[r]||function(){
  (i[r].q=i[r].q||[]).push(arguments)},i[r].l=1*new Date();a=s.createElement(o),
  m=s.getElementsByTagName(o)[0];a.async=1;a.src=g;m.parentNode.insertBefore(a,m)
  })(window,document,'script','https://www.google-analytics.com/analytics.js','ga');

  ga('create', 'UA-89762317-3', 'auto');
  ga('send', 'pageview');

</script>
