# This is currently the easiest solution to getting Space Engineers to run smoothly using DotNet:
### This guide assumes you have already have Steamplay/Proton installed with all of the prerequisites and such, as well as Space Engineers, and winetricks. If not, you can check around the internet for guides that can explain it much better than I can!

#### Here is a list of Known Issues, and potential solutions:



1. [Freeze at starup](#Issue-1)
2. [rundl32.exe error popup](#Issue-2)

3. [Looping Sound](#issue-3)

4. [Popping/Crackling Sound](#issue-4)

## The Setup Guide:

## Step 0:
  Backup any save data, blueprints, ETC...
  Then verify the integrity of your game files in Steam.

## Step 1:
  Ensure winetricks is up to date.

  `sudo winetricks --self-update`

## Step 2:
  Create your wine prefix in the compatdata folder in steamapps/common/

  `WINEPREFIX="INSERT/DIRECTORY/TO/SPACEENGINEERS/pfx" winetricks --force -q dotnet472 vcrun2015 faudio d3dcompiler_47`

## Step 3:
  Open your Space Engineers bin64 directory: usually at $HOME/.local/share/Steam/steamapps/common/SpaceEngineers/Bin64

  In this folder find the file **SpaceEngineers.exe.config** and open it in a text editor(Gedit, Kwrite, ETC...). 

  You should see something like this:
  ![before](Before.png)
    

## Step 4:
  Now add `<gcServer enabled = "true"/>` to a new line after the line that says `<runtime>`
  It should now look like this:
  ![before](After.png)

  Save the file and close it.

  Congratulations Space Engineers should now work properly on Linux. Have fun and enjoy!

  Note you may have to reapply these fixes if the game updates. It depends on if the file is changed durring the update.
  If you encounter issues try following these steps again.

  # Known issues:

  ### Issue 1:  
  Broken startup and missing videos, causing startup freeze that requires user input to get past the splash screen.

  ### Known Fix:
  Rename the file here: "LOCATION_OF_SPACE_ENGINEERS_INSTALL/SpaceEngineers/Content/Videos/KSH.wmv to KSH.wmv.old

  ### Issue 2:  
  Upon startup the game will show this error one or more times.

  ![rundllerror](rundll32.png)

  ### Known Fixes:
  1) Just hit no each time it appears. It should not impact the ability of the game to run.

  #### OR

  2) Run `WINEPREFIX="INSERT/DIRECTORY/TO/SPACEENGINEERS/pfx" winetricks`
  In the library tab of the configuration window add rundll32.exe to the New override for library box and click add. Then find it in the list below and click edit and ensure disabled is selected. (See image below:) Click apply, then close the window.

  ![rundlldisable](disablerundll32.png)

  ### Issue 3:  
  Some users report that faudio has looping sound issues.

  ### Known Fix:  
  None is known at this time. More testing is required.

  ### Issue 4:
  While using faudio some users report crackling or popping audio.

  ### Known Fix:
  Try adding the following to your steam launch options(right click Space Engineers in steam, click Properties, then in the General tab, click SET LAUNCH OPTIONS...) add the following in the box that appears.

  A) If something is already in the box add PULSE_LATENCY_MSEC=60 after those items but before %command% 

   e.g. `DXVK_HUD=full PULSE_LATENCY_MSEC=60 %command%` Make sure you have spaces between each item.

 B) If the box is empty add:

   `PULSE_LATENCY_MSEC=60 %command%`
    
   Then hit ok on the launch options window. If you are still experiencing issues, try modifying the value of PULSE_LATENCY_MSEC try 30, or 90 instead of 60.

  ## Special thanks to InflexCZE for taking the time to help. Without his help it could have taken many more months to figure this out.
  ## and Huge thanks to everyone else on https://github.com/ValveSoftware/Proton/issues/1792 for helping solve these issues.
