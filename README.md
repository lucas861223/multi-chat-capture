# Multi Chat Capture
This is a chat overlay for twitch that can be captured by OBS as browser source. The focus of this project is to have a single chat that reads from multiple channels, so collaborations between streamers only need to capture a single chat, instead of everyone's chat. BTTV emotes are supported.


## Setup ##
<details>
  <summary>For normal users who just want to use the program and don't need to know the inner workings.</summary>
  
  1. Download the latest release from [the release page](https://github.com/lucas861223/multi-chat-capture/releases). 
  2. Open the html file with any editor, i.e. Notepad++.
  3. Replace both the lucas861223 to your own twitch login(your user name, not your display name). All lowercase. Only change the lucas861223 part, leave the hashtag(#) as is.
  4. Save and close.
</details>

<details>
  <summary>For developers who want to change more than the default settings, modify the behavoir, add functionality and such.</summary>
  
  There is a lot of reasons why the setup ended the way it does. Even though it's probably not the best way to do it, it is how I did it, and I'm not sure if there is better a alternative with the requirements I have. Here is the step to set it up: 
  
  1. Clone this repository
  2. Install dependencies as marked in package.json
  3. Modify node_module/index.js, specifically replacing both lucas861223 to your own twitch handle. Only change the lucas861223 part, leave the hashtag(#) as is. 
  4. Make other changes you wish to make, save and close.
  5. Bundle index.js using webpack, with the provided webpack.config.js

</details>

After successfully setting it up, you should be able to use OBS or other broadcasting software to capture this HTML file as a browser source. You can then modify the behavior by using Twitch chat commands or just modify the default setting.

## Commands ##
All of the commands will only work if they're sent to the main channel by the broadcaster of the channel. This means, if it's set up correctly, these commands would only work if you send the commands in your own channel.
<details>
  <summary>Here is a list of twitch chat commands for this chat overlay.</summary>
 
   
  - !join \[channelNames\]
    * \[channelNames\] A single channel or multiple channels separated by commas and/or space.
    * This command joins the channels and read the messages.
  - !leave \[channelnames\]
    * \[channelNames\] A single channel or multiple channels separated by commas and/or space.
    * This command leaves the channels and stops reading the messages.
  - !pfp
    * When joining multiple chats, the default behavior shows the profile picture of the streamer in front of the message to better distinguish which chat the message is from.
    * This command toggles showing the profile picture of the streamer to always be on or off regardless of how many channels there are.
  - !highlight
    * When being tagged in chat(@username), the message will be highlighted with red background. The default behavior is on.  
    * This command toggles whether to highlight message or not.
  - !size \[fontSize\]
    * \[fontSize\] An integer for the size of the font. Default is 30.
    * This command changes the size of the font, and will also adjust size of profile picture, badges and emotes.
  - !font \[fontName\]
    * \[fontSize\] Name of the font family. For example, Ariel or consolas. Capitalization does not matter. Default is Ariel.
    * This command changes the font of chat.
  - !color \[fontColor\]
    * \[fontColor\] Hex value of the color you want the chat to be in, for example, !color #ff0000 for red font. Default is black. [Here is a RGB color picker.](https://www.w3schools.com/colors/colors_picker.asp)
    * This command changes the color of chat message (except /me message, and user names).
  - !background \[backgroundColor\]
    * \[backgroundColor\] RGB hex value of the color you want the background of the chat to be, for example, !background #000000 for black background. The hash tag is needed. Default is black. [Here is a RGB color picker.](https://www.w3schools.com/colors/colors_picker.asp)
    * This command changes the background of the chat (except the highlighted messages if they are not turned off).
  - !opacity \[value\]
    * \[value\] integer ranging from 0-255, representing how opaque the background should be. The default is 0 (totally transparent). 
    * This command changes the opacity of the background of the chat (will not affect highlighted messages, text, badges and emote).
  - !clear
    * This command clears the whole chat.
    * Note that this command can also be achieved when doing /clear from the main channel.
 

</details>

## How to modify default behavior ##
Since it's annoying to have to issue a bunch of commands every time you start it up, you can modify these changes and save the file(s), so they become the default.
<details>
  <summary>Here is a list of default settings you can change, and how </summary>
   Each of the setting correlates to one of the command. I put them in order, so if the description of the setting is vague, you can cross-read it with the command section. The way to modify it would be the same instruction as the modification step in setup.
  
   - main channel- where the command is being listened to. This should also be your twitch channel.
     * Normal users: refer to Setup.
     * Developers: modify the const variable mainChannel, with the #.
   - channels to join- list of chatroom(s) to join on start up.
     * Normal users: Replace the "lucas861223" in bracket([]) with a list of channel or channels you want to join, each double quoted and separated by a comma. i.e. to join 3 channels, replace it with "lucas861223", "chewiemelodies", "moonmoon". Order does not matter, however your own channel MUST be in one of these.
     * Developers: modify the const variable channelList the same as the above standard.
   - Forcefully toggle PFP on/off.
     * Normal users: this is not possible to set. 
     * Developers: set the variable needsPFPOverride to true, and pfpOverride to true if you want it on, false if off.
   - Whether to highlight mentioned messages.
     * Normal users: this is not possible to set. 
     * Developers: set mentionHighlightOvverride to true to turn off highlighting.
  
  For the rest of the setting, they can all be achieved by modifying the css in index.html regardless of how you set it up.
   - font size
     * in :root, --font-size. Also change --emote-size to 1.5x of font size.
   - font color
     * in :root, --color. Replace it with RGB hex value. Include the hastag. 
   - font
     * in :root, --font-family
   - background color
     * in :root. --background. Replace it with RGBA hex value. The A is for transparency. Include the hashtag. [Here is a RGBA color picker.](https://hugabor.github.io/color-picker/)
</details>

## So what the hell is this setup? ##
<details>
  <summary> Here's why it ended up this way </summary>
  
  Originally I use [ComfyJS](https://github.com/instafluff/ComfyJS) for the IRC client because it is simple, and most importantly it is hosted on a CDN service. So I can just use <script src="link"\> in HTML for it to be included, without needing any other more files. I wanted to make this project as lightweight as possible, only needing 1 .html file would be the ideal, so no matter how not-tech-savvy one is, it can be easily used. 
  
  However ComfyJS do not handle timeout and bans, only message delete. This is not really acceptable because in slower chats, the same message may be on screen for a long time. If timeout and bans are not caught and messages are not removed, it may leave some crap on stream for extended period of time. Having to separatedly issue additional command after bans and timeout also seems too annoying. I thought about just extending the original class, but with the way the module was originally set up and the obfuscator used for it to be hosted on CDN, it does not seem possible. 
  
  In looking for alternatives, I thought of [tmi.js](https://github.com/tmijs/tmi.js), which I had seen in many twitch related JS projects. But there is more problems: it is not hosted on any CDN service that I can find. That means I need to include it using node becuase it also requires a bunch of stuff (and just "requries" itself). For it to be included, I need to use node js, which would make this project more than a .html file, and complicated it more from "oh just use it as browser source lol" to "oh just open cmd, download this download that, type this type that and use this URL as browser source lol".
  
  So to solve it, I try to find how to "combine" these dependencies and such into 1 file. Then I saw [webpack](https://webpack.js.org/). I used it, it worked great, but here's yet ANOTHER problem... In order to protect the source code, webpack uses obfuscator, which makes the whole script un-readable. So instead of having a .html with clean, readable code in the script section and telling normal users "oh just open the .html file, change this to that", I have to make hard-coded strings that which won't get obfuscated so they can easily control-F to find and replace them amongst the spaghetti... And because I need to use tmi.js and webpack, I need to include all these other stuff (webpack.config.js, node_module, index.js... etc.) in the repo. And this is how the whole project ended up in this mess.   
</details>
