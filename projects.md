---
layout: page
title: Projects
permalink: /projects/
---

Below is a listing of personal projects I have hosted here on github.  
Many of these are recovered from old archives so may no longer run if you attempt to do so.

**[Eve Pricing]**  
**Tools**: Ruby, Sinatra, MongoDB  
Eve Pricing is a project I put together to help a friendcome up with potential costs for crafting simple items in Eve Online. 
It pulls item pricing for specific regions in the game from [EVE-Central](http://eve-central.com) periodically. Current cost and history for each item is available including a comparative price between the current market price of the item and the total price to purchase the components of the item.

**Notes**:  
After several months, the site's responsiveness slowed down drastically.
It was never fully examined as the site was not really be used at that point.
There were several potential points to be examined including the periodic script to update all the item pricing, the database structure itself, and the VM it was running on.
This project is my first projct using MonogoDB and an ORM to talk to the database. I felt it could have been the database growth combined with the structure causing slow look up times whenever an item's history data was involved.

**[Gilded Rose Kata]**
**Tools**: Ruby, RSpec
This is a refactoring project converted to Ruby and refactored.
The original source, ruby conversion source, my refactored code, and my rationale on the changes I made are discussed in the Readme.

**[Keyboard Macro Simulator]**  
**Tools**: Ruby  
This project roughly simulates the macro keys found on gaming keyboards.
I wrote it after becoming frustrated with how often the software to generate a macro with my keyboard would crash when attempting to make a macro.
The project is designed to keep the same requirements the macros on a keyboard would have, namely only sending keystrokes to the active window.  

**Notes**:
The program has long keypress delays (a full second) and must be told what macro to run via the console, then the program to be controlled needs to be set to the active window.  
To feel more like keyboard macros, it could be made to intercept keystrokes and watch for specific combinations to execute macros.  
Breaking the macros away from specific methods and into configuration files would make it more expandable and less specific to a particular game.

**[Rcraft]**  
**Tools**: Ruby  
Rcraft is a ruby wrapper around a MineCraft server.  
It runs the minecraft server and intercepts I/O from it, primarily receiving notices and player chat.  
Player chat is parsed and used to tell if any commands are issued.
Server commands can be issued without giving players access to full admin rights.
Primarily, this allows for giving players resources and items while in survival mode.
Item requests are more friendly than the normal server command as items can be requested by name rather than the item id, though a fallback to the item id is in place.

I wrote this when running a minecraft server for some friends.  
As a wrapper, instead of a mod, this gave item access to them no matter what mods we placed on the server itself. It worked just as well for a vanilla server as it did for a modded server.

**Notes**:  
Minecraft has changed considerably since this was last updated so this will no longer work without updating item names and ids at the very least.

**[URL Shortener With Sinatra]**  
**Tools**: Ruby, Sinatra, ActiveRecord
This project is a basic url shortener written with Ruby and Sinatra. 
It follows a screencast discussing how to do so with some modifications to overcome some errors in the code.  The screencast's page still exists but the video itself appears broken.
I heavily commented it for some friends interested in it to get an idea of what was going on.

The program uses base62 via the alphadecimal gem to convert the row's id to a short string of characters comprised of upper and lower case characters and numbers.  It avoids multiple entries with the same url by returning the existing entry if the url is added again.

[Eve Pricing]: https://github.com/makkura/eve_pricing
[Gilded Rose Kata]: https://github.com/makkura/gilded_rose_kata
[Keyboard Macro Simulator]: https://github.com/makkura/keyboard_macros
[Rcraft]: https://github.com/makkura/rcraft
[URL Shortener With Sinatra]: https://github.com/makkura/short_url_sinatra
[WoW Horn]: https://github.com/makkura/wow_horn