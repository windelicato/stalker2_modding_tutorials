# Stalker 2 Replacing Virtual Textures in Unreal Engine 5.1

## Required Tools

- FModel: https://fmodel.app/
- Unreal Engine 5.1: https://www.unrealengine.com/en-US

# Install
- Install Unreal Engine 5.1 (not 5.3, not 5.2, not 4.8, not openxray :( )
- Install and configure Fmodel (not covered in this tutorial)


# Set up our project


- Create a new project with the template __Games > Blank__ and the following settings
  - Blueprint
  - Target Platform: Desktop 
  - Quality Preset: Maximum
  - Starter Content: [ ] (unchecked)
  - Raytracing: [ ] (unchecked)
  - Name the project __Stalker2__
    - If anyone knows a way around this lmk this is the only way my packages had the correct mount point

![Alt text](img/newproject.png?raw=true "img")


# Set up our options

- Click the __Platforms__ button and go to __Packaging Settings__

![Alt text](img/settings.png?raw=true "img")


  - Enable __Use Pak File__ [X]
  - Enable __Use Io Store__ [X]
  - Enable __Generate Chunks__ [X]


![Alt text](img/settingspackaging1.png?raw=true "img")
    

- Search for "cook"
    - Enable __Cook everything in the project content directory__ [x]

![Alt text](img/settingspackaging2.png?raw=true "img")

# Virtual Texturing
- Click on __Edit__ and go to __Project Settings__
- Search for "virtual"
- Scroll down and enable the following 
    - Enable __Enable virtual texture support__ [x]

- And then optionally if the textures you're replacing include these, enable them:
    - Enable __Enable virtual texture lightmaps__ [x]
    - Enable __Enable virtual texture anistropic filtering__ [x]
    - Enable __Enable virtual textures for Opacity Mask__ [x]


![Alt text](img/virtualenable.png?raw=true "img")


- UE5 should prompt you to restart. Do that and wait 6 years


# Adding your cooked textures
- Add your textures matching the directory structure for your texture in FModel

![Alt text](img/checkpath.png?raw=true "img")

![Alt text](img/importtextures.png?raw=true "img")



- Now its time to edit the properties for each of your textures:
    - Check the properties for your texture in FModel for these fields:
        - [?] CompressionSetting
        - [?] PixelFormat
        - [?] sRGB
        - [?] VirtualTextureStreaming
     
          
   - Diffuse texture
    - __PixelFormat__ is DXT1 and there is no __CompressionSetting__ so this is a "Default" texture
        
![Alt text](img/diffuse-check.png?raw=true "img")


   - Normal texture
    - __PixelFormat__ is DXT5 and __CompressionSetting__ is __TC_Normal__ so this is a "Normal" texture

        
![Alt text](img/normal-check.png?raw=true "img")
        
![Alt text](img/normal-comp-check.png?raw=true "img")



   - RHO (roughness, height, opacity) or other maps:
    - __PixelFormat__ is DXT1 and __CompressionSetting__ is __TC_Masks__ so this is a "Masks" texture

              
![Alt text](img/rho-check.png?raw=true "img")
        
![Alt text](img/rho-comp-check.png?raw=true "img")


# Convert to virtual textures
- Once you've set up all the textures with compression settings, it's time to make our virtual textures
    - Select every texture that had __VirtualTextureStreaming: true__
    - Right click > __Convert To Virtual Texture__
        
![Alt text](img/converttovirtual.png?raw=true "img")


  - In the popup, lower the filter until all of your textures are selected
        
![Alt text](img/converttovirtualpopup.png?raw=true "img")

  - Hit save all
        
![Alt text](img/converttovirtualcheck.png?raw=true "img")


# Create asset label
- Back in the __Content__ folder, right click and create a new data asset
  
![Alt text](img/assetlabelselect.png?raw=true "img")

   - Select __Primary Asset Label__

![Alt text](img/assetlabeltype.png?raw=true "img")

  - Name it __Label_YOURMODNAME__
    
![Alt text](img/assetlabelname.png?raw=true "img")

   - Set the following:
     - __Priority__: 1 or higher
     - __Chunk ID__: 1000 or higher, this needs to be unique to your mod so don't pick 1420 cuz its already taken
     - __Cook Rule__: __Always cook__
     - __Label Assets in my directory__: [x] (CHECKED)
    
![Alt text](img/assetlabelsettings.png?raw=true "img")

   - Hit save all

# HIT SAVE ALL

# Package it up
- Click __Platforms__ > __Windows__ > __Shipping__
- Click __Platforms__ > __Windows__ > __Package Project__
        
![Alt text](img/packageit.png?raw=true "img")



- Pick a directory for your generated package, I make a __Build__ directory with subdirectories for unique builds
- *This will take a long time* 

# Yep still packaging

# Done yet?

# NOPE LOL NICE TRY still packaging

# FINALLY

# Grab your pak
- In the directory you picked, grab the 3 files with the chunk id you picked earlier
        
![Alt text](img/packagerename1.png?raw=true "img")

- Rename them all something like __z_YourMod_P__
- *make sure it ends in _P *
          
![Alt text](img/packagerename2.png?raw=true "img")


- Place all 3 files in your __~mods/__ directory

- Open FModel and confirm your structure matches the game and your asset is replaced. if not cry or check your project name

![Alt text](img/packagetest.png?raw=true "img")


# Debugging
- Asset isn't replaced in FModel
    - Try recreating the asset label with a different Chunk ID
        - After doing this, go to Platforms > Windows > Cook Content and then Platforms > Windows > Package Project

# Test it out!!!
- Open the game and close it again for the 30th time tonight
