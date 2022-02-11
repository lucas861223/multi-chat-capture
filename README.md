# Multi Chat Capture
This is a chat overlay for twitch that can be captured by OBS as browser source. The focus of this project is to have a single chat that reads from multiple channels, so collaborations between streamers only need to capture a single chat, instead of everyone's chat. BTTV and FFZ emotes are supported.

**Discontinued**
Twitch now asks for OAuth tokens or App Access Token to get user information, which is needed for profile pictures, channel id for channel emotes. It's pretty understandable but stupid. The user id part is pretty easily resolved, but as far as I can find, no one has done a profile picture fetcher, and having profile picture is what this overlay is originally for. Getting and refreshing the tokens are a pain, especially since this is just a chat overlay, so I likely won't do anything about it. 


## Setup ##
  
  1. Download the latest release from [the release page](https://github.com/lucas861223/multi-chat-capture/releases). 
  2. Open the html file with any editor, i.e. Notepad++.
  3. Replace lucas861223 to your own twitch login(your user name, not your display name). All lowercase. Only change the lucas861223 part, leave the hashtag(#) as is.
  4. Save and close.

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
  - !shadow
    * This command toggles text shadow on/off. For now the settings of the shadow cannot be modified via command, however you can modify it by changing the css of the html file regardless of which setup you chose. The default behavior is off.
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
     * Modify the const variable mainChannel, with the #.
   - channels to join- list of chatroom(s) to join on start up.
     * Add a list of channel or channels you want to join to variable channelList, each double quoted and separated by a comma. i.e. to join 3 channels, modify channelList to be [mainChannel, "moonmoon", "chewiemelodies"]. Order does not matter; do not remove mainChannel.
   - Forcefully toggle PFP on/off.
     * Set the variable needsPFPOverride to true, and pfpOverride to true if you want it on, false if off.
   - Whether to highlight mentioned messages.
     * Set mentionHighlightOvverride to true to turn off highlighting.
  
  For the rest of the setting, they can all be achieved by modifying the css in index.html.
   - font size
     * in :root, --font-size. Also change --emote-size to 1.5x of font size.
   - font color
     * in :root, --color. Replace it with RGB hex value. Include the hastag. 
   - font
     * in :root, --font-family
   - background color
     * in :root, --background. Replace it with RGBA hex value. The A is for transparency. Include the hashtag. [Here is a RGBA color picker.](https://hugabor.github.io/color-picker/)
   - text shadow
     * in :root, --text-shadow. Replace it with the settings you want. Google for format.
</details>
