# Stalker 2 Replacing Virtual Textures in Unreal Engine 5.1

## Required Tools

- FModel: https://fmodel.app/
- Unreal Engine 5.1: https://www.unrealengine.com/en-US

# Install
- Install Unreal Engine 5.1 (not 5.3, not 5.2, not 4.8, not openxray :( )

# Set up our project
- Create a new project with the template `Games > Blank` and the following settings
  - Blueprint
  - Target Platform: Desktop 
  - Quality Preset: Maximum
  - Starter Content: [ ] (unchecked)
  - Raytracing: [ ] (unchecked)
  - Name the project `Stalker2`
    - If anyone knows a way around this lmk this is the only way my packages had the correct mount point

# Set up our options
- Click the `Platforms` button and go to `Packaging Settings`
    - Enable `Use Pak File` [X]
    - Enable `Use Io Store` [X]
    - Enable `Generate Chunks` [X]

- Search for "cook"
    - Enable `Cook everything in the project content directory` [x]

# Virtual Texturing
- Click on `Edit` and go to `Project Settings`
- Search for "virtual"
- Scroll down and enable the following 
    - Enable `Enable virtual texture support` [x]

- And then optionally if the textures you're replacing include these, enable them:
    - Enable `Enable virtual texture lightmaps` [x]
    - Enable `Enable virtual texture anistropic filtering` [x]
    - Enable `Enable virtual textures for Opacity Mask` [x]

- UE5 should prompt you to restart. Do that and wait 6 years


# Adding your cooked textures
- Add your textures matching the directory structure for your texture in FModel
- Now its time to edit the properties for each of your textures:
    - Check the properties for your texture in FModel for these fields:
        - [?] CompressionSetting
        - [?] PixelFormat
        - [?] sRGB
        - [?] VirtualTextureStreaming
    - Diffuse texture
      - `PixelFormat` is DXT1 and there is no `CompressionSetting` so this is a "Default" texture
    - Normal texture
      - `PixelFormat` is DXT5 and `CompressionSetting` is `TC_Normal` so this is a "Normal" texture
    - RHO (roughness, height, opacity) or other maps:
      - `PixelFormat` is DXT1 and `CompressionSetting` is `TC_Masks` so this is a "Masks" texture

# Convert to virtual textures
- Once you've set up all the textures with compression settings, it's time to make our virtual textures
    - Select every texture that had `VirtualTextureStreaming: true`
    - Right click > `Convert To Virtual Texture`
    - In the popup, lower the filter until all of your textures are selected
    - Hit save all

# Create asset label
- Back in the `Content` folder, right click and create a new data asset
    - Select `Primary Asset Label`
    - Name it `Label_YOURMODNAME`
    - Set the following:
        - `Priority`: 1 or higher
        - `Chunk ID`: 1000 or higher, this needs to be unique to your mod so don't pick 1420 cuz its already taken
        - `Cook Rule`: `Always cook`
        - `Label Assets in my directory`: [x] (CHECKED)
    - Hit save all

# HIT SAVE ALL

# Package it up
- Click `Platforms` > `Windows` > `Shipping`
- Click `Platforms` > `Windows` > `Package Project`
- Pick a directory for your generated package, I make a `Build` directory with subdirectories for unique builds
- *This will take a long time* 

# Yep still packaging

# Done yet?

# NOPE LOL NICE TRY still packaging

# FINALLY

# Grab your pak
- In the directory you picked, grab the 3 files with the chunk id you picked earlier from `
- Rename them all something like `z_YourMod_P`
- *make sure it ends in _P *
- Place all 3 files in your `~mods/` directory
- Open FModel and confirm your structure matches the game if not cry or check your project name

# Debugging
- Asset isn't replaced in FModel
    - Try recreating the asset label with a different Chunk ID
        - After doing this, go to Platforms > Windows > Cook Content and then Platforms > Windows > Package Project

# Test it out!!!
- Open the game and close it again for the 30th time tonight