# Stalker 2 Replacing Virtual Textures in Unreal Engine 5.1

## Required Tools

- FModel: https://fmodel.app/
- Unreal Engine 5.1: https://www.unrealengine.com/en-US

# Install
- Install Unreal Engine 5.1 (not 5.3, not 5.2, not 4.8, not openxray :( )
- Install and configure Fmodel (not covered in this tutorial)


# Set up our project


- Create a new project with the template `Games > Blank` and the following settings
  - Blueprint
  - Target Platform: Desktop 
  - Quality Preset: Maximum
  - Starter Content: [ ] (unchecked)
  - Raytracing: [ ] (unchecked)
  - Name the project `Stalker2`
    - If anyone knows a way around this lmk this is the only way my packages had the correct mount point

![Alt text](img/newproject.png?raw=true "img")


# Set up our options

- Click the `Platforms` button and go to `Packaging Settings`

![Alt text](img/settings.png?raw=true "img")


  - Enable `Use Pak File` [X]
  - Enable `Use Io Store` [X]
  - Enable `Generate Chunks` [X]


![Alt text](img/settingspackaging1.png?raw=true "img")
    

- Search for "cook"
    - Enable `Cook everything in the project content directory` [x]

![Alt text](img/settingspackaging2.png?raw=true "img")

# Virtual Texturing
- Click on `Edit` and go to `Project Settings`
- Search for "virtual"
- Scroll down and enable the following 
    - Enable `Enable virtual texture support` [x]

- And then optionally if the textures you're replacing include these, enable them:
    - Enable `Enable virtual texture lightmaps` [x]
    - Enable `Enable virtual texture anistropic filtering` [x]
    - Enable `Enable virtual textures for Opacity Mask` [x]


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
    - `PixelFormat` is DXT1 and there is no `CompressionSetting` so this is a "Default" texture
        
![Alt text](img/diffuse-check.png?raw=true "img")


   - Normal texture
    - `PixelFormat` is DXT5 and `CompressionSetting` is `TC_Normal` so this is a "Normal" texture

        
![Alt text](img/normal-check.png?raw=true "img")
        
![Alt text](img/normal-comp-check.png?raw=true "img")



   - RHO (roughness, height, opacity) or other maps:
    - `PixelFormat` is DXT1 and `CompressionSetting` is `TC_Masks` so this is a "Masks" texture

              
![Alt text](img/rho-check.png?raw=true "img")
        
![Alt text](img/rho-comp-check.png?raw=true "img")


# Convert to virtual textures
- Once you've set up all the textures with compression settings, it's time to make our virtual textures
    - Select every texture that had `VirtualTextureStreaming: true`
    - Right click > `Convert To Virtual Texture`
        
![Alt text](img/converttovirtual.png?raw=true "img")


  - In the popup, lower the filter until all of your textures are selected
        
![Alt text](img/converttovirtualpopup.png?raw=true "img")

  - Hit save all
        
![Alt text](img/converttovirtualcheck.png?raw=true "img")


# Create asset label
- Back in the `Content` folder, right click and create a new data asset
  
![Alt text](img/assetlabelselect.png?raw=true "img")

   - Select `Primary Asset Label`

![Alt text](img/assetlabeltype.png?raw=true "img")

  - Name it `Label_YOURMODNAME`
    
![Alt text](img/assetlabelname.png?raw=true "img")

   - Set the following:
     - `Priority`: 1 or higher
     - `Chunk ID`: 1000 or higher, this needs to be unique to your mod so don't pick 1420 cuz its already taken
     - `Cook Rule`: `Always cook`
     - `Label Assets in my directory`: [x] (CHECKED)
    
![Alt text](img/assetlabelsettings.png?raw=true "img")

   - Hit save all

# HIT SAVE ALL

# Package it up
- Click `Platforms` > `Windows` > `Shipping`
- Click `Platforms` > `Windows` > `Package Project`
        
![Alt text](img/packageit.png?raw=true "img")



- Pick a directory for your generated package, I make a `Build` directory with subdirectories for unique builds
- *This will take a long time* 

# Yep still packaging

# Done yet?

# NOPE LOL NICE TRY still packaging

# FINALLY

# Grab your pak
- In the directory you picked, grab the 3 files with the chunk id you picked earlier
        
![Alt text](img/packagerename1.png?raw=true "img")

- Rename them all something like `z_YourMod_P`
- *make sure it ends in _P *
          
![Alt text](img/packagerename2.png?raw=true "img")


- Place all 3 files in your `~mods/` directory

- Open FModel and confirm your structure matches the game and your asset is replaced. if not cry or check your project name

![Alt text](img/packagetest.png?raw=true "img")


# Debugging
- Asset isn't replaced in FModel
    - Try recreating the asset label with a different Chunk ID
        - After doing this, go to Platforms > Windows > Cook Content and then Platforms > Windows > Package Project

# Test it out!!!
- Open the game and close it again for the 30th time tonight
