# Music-Assistant-Lovelace-UI
A work in progress Lovelace UI for Music Assistant in Home Assistant. 


https://github.com/user-attachments/assets/0358ee10-6fdf-40ce-b838-19a5c3837f03


I created this github as a storage location for the discussion on this custom UI started over at https://github.com/orgs/music-assistant/discussions/2601
Please feel free to comment in that thread if you want to join the discussion. 

I wish I could have spend more time to fix a few things and make this an easier design to edit, but unfortunately I have not had the time to do this at the moment. With that said, I am going to release the code for it as is for you to pick through and see if you can find anything useful. Ill make updates to this when/if I have time. 
Remember this layout is for an oddball touchscreen I have on my stereo, with a resolution of 1920x720. This will obviously need to be addressed for different screens. 

This is pulled straight from my Raw Configuration Editor, so you should be able to create a new dashboard, and then just paste this into the Raw Config Editor on your end.  There are a few other bits you will need, and obviously some things you need to change to fit your setup. 

For starters, you need to edit the local address IP of your Music Assistant. In the Music-Assistant-Lovelace-UI.yaml file do a search for "192.168.86.6:8095" and edit it accordingly. Note that since this is a local IP this will not work if the page is viewed via Nuba Case, or other custom HTTPS domain available on your WAN. This is done for simplicity and security. If you want to go down the rabbit hole of opening this up securely to the outside world, I leave that up to you. 

Also make sure that Music Assistant webserver is enabled, or you will not be able to view any of its pages in the iFrames. 
To do this open up Music Assistant and go to settings. Choose the icon for Core in the top right and then choose Web Server(frontend and API) from the list. 
Make sure Expose the webserver (port 8095) is checked ON. 

Secondly you need to create a few Helpers in HA to act as triggers for this UI to work correctly. 

1. Create an input_boolean or Toggle, mine is called "input_boolean.rx_stereo_menu". You can use this name if you like, or name it whatever you want and then search for and edit the above code to replace "input_boolean.rx_stereo_menu" with whatever your boolean was named. 
This controls the side menu from being shown or hidden. 
2. Create a second input_boolean or Toggle, mine is called "input_boolean.rx_stereo_volume". You can use this name if you like, or name it whatever you want and then search for and edit the above code to replace "input_boolean.rx_stereo_volume" with whatever your boolean was named. This controls the touch volume slider from being shown or hidden. 
3. Create a input_select or Dropdown, mine is called "input_select.rx_stereo_menu_list". You can use this name if you like, or name it whatever you want and then search for and edit the above code to replace "input_select.rx_stereo_menu_list" with whatever your boolean was named. This controls which iframe window to show from the side menu. You need to add the following entries for the input_select, I cant remember if the order matters, so just in case I would add them in the order below. 
  -   Search
  -   Playlists
  -   Albums
  -   Artists
  -   Tracks
  -   Stations
  -   Coverart
4. This UI was built to work with a single player in my house. I wanted to get around to making this player a variable selection, but did not have the time to do so.
As it stands you would need to create a new dashboard or dashboard view for each player you want an interface for. 
Search through the yaml code for "entity: media_player.squeeze_rxstereo" and replace it with the name of the player you want to use. 

You also need a few images and the theme file. 
The images, attached here need to go in the root of your "www" folder in your config folder. If you put them into a sub-folder under www make sure you edit the code above accordingly. In the code above the path is listed as "/local/mass-power.png" and "/local/mass-background2.png", and there are two entries for each. You would need to edit these paths to "/local/sub-folder-name/mass-background2.png" and "/local/sub-folder-name/mass-power.png" accordingly for each if you put them in a subfolder. 
The theme is just an edit of a Mushroom theme. 
DO NOT PUT THIS THEME FILE IN THE MAIN MUSHROOM THEME FOLDER!!!! When Mushroom updates, it can delete custom themes in the /config/themes/mushroom folder. So create a secondary folder in /config/themes called "Custom Mushroom" and put the theme file in there. 

Lastly, there are some custom Lovelace cards you are going to need to make this work. 
All of these should be available in HACS, and I suggest using that method to install them. 
- _Mini-Media-Player_  https://github.com/kalkih/mini-media-player
- _Lovelace-hui-element_  https://github.com/thomasloven/lovelace-hui-element
- _Lovelace Mushroom Cards_  https://github.com/piitaya/lovelace-mushroom
- _My-Slider-v2_  https://github.com/AnthonMS/my-cards/blob/main/docs/cards/slider-v2.md
- _Lovelace-State-Switch_  https://github.com/thomasloven/lovelace-state-switch
- _Lovelace-Layout-Card_  https://github.com/thomasloven/lovelace-layout-card
- _Kiosk-Mode_  https://github.com/NemesisRE/kiosk-mode

**Note for Kiosk Mode - You can remove the section of the code before "views:" if you do not want to use Kiosk Mode. I know people use different methods for hiding the Side Menu and Top Bar. I prefer hiding the side menu via Home Assistant settings, and then using the Kiosk Mode addon to hide the top bar. 
The reason I like this is because I can create a helper input_boolean or toggle to show/hide the top bar. I then add a double tap function to one of the UI elements in the layout to toggle this helper. This way all I need to do when I want to edit a layout is double tap that element, and I can edit, then once I'm done with the edit, I double tape the element again and Im back to a nice clean interface with no top bar. If you want to give this method a try, just create a toggle helper called inbut_boolean.hide_header, and then add the following to one of the buttons on the screen. I did not add this to any of the elements in this UI yet, as I have this UI as a secondary view under my main layout. I control the header from one of the other views. If you want to add it, just choose which page element you want to act as your double tap and add the following: 
```
double_tap_action:
  action: call-service
  service: input_boolean.toggle
  target:
    entity_id: input_boolean.mbr_hide_header
```

I think that's it, but I might have missed something. I still have notifications set for the thread at Music Assistant this started under, so just let me know if you have questions. I will try to help, but I am still a bit overwhelmed with work. 
