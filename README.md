# apple-gpt-doc

{{Image|Game Porting Toolkit.png|Released 6th June, 2023 at WWDC23.}}

Game Porting Toolkit is Apple's new translation layer released on 6th June, 2023. Game Porting Toolkit ('''GPTK''') combines Wine with Apple's own D3DMetal which supports DirectX 11 and 12<ref>{{Refurl|url=https://mastodon.gamedev.place/@gavkar/110501451404624870|title=Gokhan Avkarogullari: "@yiningkarlli @aras @romainguy…" - Gamedev Mastodon}}</ref>. This is a less user-friendly method of installing Windows games on Apple Silicon Macs compared to [[CrossOver]] or [[Parallels]], however it unlocks the ability to play many DirectX 12 games. A lot more games work using GPTK, however, games that use anti-cheat or aggressive DRMs generally don't work, along with games that require AVX/AVX 2,<ref>This is a Rosetta limitation; Intel Macs should run these games just fine.</ref> e.g. The Last of Us Part I.

== Toolkit install instructions ==

=== Requirements ===
*macOS Sonoma should be used. You can download the pkg installer from [https://mrmacintosh.com/macos-sonoma-full-installer-database-download-directly-from-apple/ Mr Macintosh blog].
*macOS Ventura causes large numbers of issues with steamwebhelper.exe crashing so it isn't recommended, use the macOS Sonoma beta.
*Visit [https://developer.apple.com/downloads Apple Developer Downloads site], these files are now free to download use for any logged in Apple account.
**Search for Command Line Tools for Xcode 15 beta and download the dmg file, and run the contained pkg file. 
***If you have an old version Xcode installed, remove it.
**Search for Game Porting Toolkit and download it. Open/mount the dmg file (some commands will require that it is mounted).

=== Automated Installer ===
*A new open source installer which automates the steps found in the Homebrew approach below is now available from [https://github.com/installaware/AGPT InstallAware's GitHub Repo]. Scroll down where they have a screenshot of their graphical user interface, and download the DMG from the link right above that.
*The automated installer is notarized by Apple, so has no known malware and runs as soon as you've double-clicked it without any Gatekeeper concerns.
*Neither macOS Sonoma nor an Apple Silicon device are required by the automated installer.
*You also won't need to download the Game Porting Toolkit DMG from Apple, although if available, it will be utilized (you just point the installer to the DMG download, and it takes care of the rest). Of course, in this scenario, you would still need macOS Sonoma and an Apple Silicon device to enjoy 3D acceleration benefits.
*The automated installer also doubles up as a GUI to launch existing installed apps, all visually without ever having to drop down to the command line.

=== Macports ===
{{Fixbox|description=Macports option ADVANCED USERS ONLY| ref= | collapsed=yes| 
fix=(Optional) 

if you want install under Macports here is the guide
change directory into /opt
<pre> cd /opt</pre>
clone the repository for macports-wine
<pre> sudo git clone https://github.com/Gcenx/macports-wine</pre>
follow this guide to configure the local repository then sync 
https://guide.macports.org/#development.local-repositories
<pre> sudo port sync </pre>
you are ready to install 
first install game-porting-toolkit-d3dmetal , it will fail at first , follow the steps it gives you which tells you where to put the game porting toolkit dmg
<pre> sudo port install game-porting-toolkit-d3dmetal</pre>
next you install the porting toolkit itself and you should be good to move onto the Wine Prefix section in this guide, keep in mind it might fail the first time or two , but it might just need a retry; if it still doesn't work after a couple tries please report issues to https://github.com/Gcenx/macports-wine/issues
<pre> sudo port install game-porting-toolkit</pre>
Keep in mind instead of brew --prefix you will need to replace it with the wine from macports path,
/opt/local/libexec/game-porting-toolkit/bin/wine64 ;
and gameportingtoolkit itself should be in path and if not its under 
/opt/local/bin/gameportingtoolkit 
}}
=== Homebrew ===
Note: if you have ever installed Homebrew before, then it is advised to remove ARM64 Homebrew as this can interfere with this build process. Either use a Homebrew uninstall script or delete the folder <code>/opt/homebrew/bin</code>. If you prefer to keep both ARM64 and x86 versions of brew installed, you may add a "brew-switcher" to your <code>.zshrc</code> file to allow either version to be used depending on the active architecture. You may follow the steps at the end of this section after installing Brew to achieve this.

Open Terminal (search in Spotlight on macOS).

Install Rosetta:
<pre>softwareupdate --install-rosetta</pre>

Enter an x86_64 shell to continue the following steps in a Rosetta environment. All subsequent commands should be run within this shell.
<pre>arch -x86_64 zsh</pre>

Install the x86_64 version of Homebrew if you don't already have it.
<pre>/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"</pre>

Set the path:
<pre>
(echo; echo 'eval "$(/usr/local/bin/brew shellenv)"') >> ~/.zprofile
eval "$(/usr/local/bin/brew shellenv)"
</pre>

Make sure the brew command is on your path:
<pre>which brew</pre>

If this command does not print <code>/usr/local/bin/brew</code>, you should use this command:
<pre>export PATH=/usr/local/bin:${PATH}</pre>
{{Fixbox|description=Optionally retain both ARM64 and x86 versions of Brew|ref=|collapsed=yes|fix=
(Optional) If you want to have both ARM64 and x86 versions of Brew installed, begin by editing your <code>.zshrc</code> file:
<pre>nano ~/.zshrc</pre>

Scroll down (by using the arrow keys or Control + V) to the bottom of the file, and paste the following script:
<pre>
if [ "$(arch)" = "arm64" ]; then
    eval "$(/opt/homebrew/bin/brew shellenv)"
else
    eval "$(/usr/local/bin/brew shellenv)"
fi
</pre>

You may now restart your terminal and use the following command to return to an x86_64 shell:
<pre>arch -x86_64 zsh</pre>
Your shell will now select the right installation of Brew, depending on your architecture.
}}

(For bash holdovers: zsh instructions above also apply to bash. Just replace <code>.zshrc</code> with <code>.bashrc</code> and <code>.zprofile</code> with <code>.bash_profile</code>.)

=== Build ===
Run this command to download Apple tap:
<pre>brew tap apple/apple http://github.com/apple/homebrew-apple</pre>

Install the game-porting-toolkit formula. This formula downloads and compiles several large software projects. How long this takes will depend on the speed of your computer. It can take over 1 hour to complete depending on the speed of your Mac. This step depending on specs may take around 75 minutes on M1 to 36 minutes on M2 Max.
<pre>brew -v install apple/apple/game-porting-toolkit</pre>

If during installation you see an error such as '''“Error: game-porting-toolkit: unknown or unsupported macOS version: :dunno”, your version of Homebrew doesn’t have macOS Sonoma support. Update to the latest version of Homebrew and try again.'''
<pre>brew update ; brew -v install apple/apple/game-porting-toolkit</pre>

=== Prebuild ===
To save time you can use prebuild GPTK formula maintained by Gcenx.
<pre>brew tap gcenx/homebrew-apple</pre>

Install the game-porting-toolkit formula. This formula downloads and compiles several large software projects. How long this takes will depend on the speed of your computer. It can take over 1 hour to complete depending on the speed of your Mac.
<pre>brew install gcenx/wine/game-porting-toolkit</pre>

If during installation you see an error such as '''“Error: game-porting-toolkit: unknown or unsupported macOS version: :dunno”, your version of Homebrew doesn’t have macOS Sonoma support. Update to the latest version of Homebrew and try again.'''
<pre>brew update ; brew -v install gcenx/wine/game-porting-toolkit</pre>

=== Ensure the toolkit is already on latest version ===

Ensure you're in an x86_64 shell to continue the following steps in a Rosetta environment. All subsequent commands should be run within this shell, re-run if you're unsure if you're in correct shell or just doing an update.
<pre>arch -x86_64 zsh</pre>

Run Homebrew to gather potential updates and upgrade Apple's GPTK formula:

<pre>brew update && brew upgrade apple/apple/game-porting-toolkit</pre>

This step depending on specs may take around 48 minutes on M1 to 19 minutes on M2 Max.

If you're using Gcenx's premade formula to update prebuild GPTK fomula:

<pre>brew update && brew upgrade gcenx/wine/game-porting-toolkit</pre>

Remember using Gcenx's prebuild just saves you from building it from source, you need to follow next steps mentioned in this guide below or Apple's README.rtf included with Apple's GPTK DMG, which in case of drastic changes made by Apple will be always right, Wiki has some pros of all community tips and fixes bundled together but it takes a while to be up to date.

Doesn't matter if you built GPTK from Apple or using Gcenx's prebuild, if you're updating from previous release, make sure to repeat <code>[[#Preparing the toolkit]]</code> section below.

=== Preparing the toolkit ===
Make sure the Game Porting Toolkit dmg downloaded earlier is mounted at /Volumes/Game Porting Toolkit-1.0. Use this script to copy the Game Porting Toolkit library directory into Wine’s library directory.
<pre>ditto /Volumes/Game\ Porting\ Toolkit-1.0/redist/lib/ `brew --prefix game-porting-toolkit`/lib/</pre>

If for some reason you are installing a previous version of Game Porting Toolkit, refer to the Read Me.rtf file on that disk image for the correct command to copy the D3DMetal libraries.

Put the 3 scripts from the Game Porting Toolkit DMG into here <code>/usr/local/bin</code> using this command:
<pre>cp /Volumes/Game\ Porting\ Toolkit*/gameportingtoolkit* /usr/local/bin</pre>

===Wine prefix ===
A Wine prefix contains a virtual C: drive, similar to a Bottle in [[CrossOver]]. You will install the toolkit and your game into this virtual C: drive. Run the following command to create a new Wine prefix named my-game-prefix in your home directory. This will create a Wine prefix called <code>my-game-prefix</code> but could be renamed to anything.
<pre>WINEPREFIX=~/my-game-prefix $(brew --prefix game-porting-toolkit)/bin/wine64 winecfg</pre>
*A “Wine configuration” window should appear on your screen.
*Change the version of Windows to Windows 10.
*Choose Apply and then OK to exit winecfg.

If the “Wine configuration” window does not appear, and no new icon appears in the Dock, verify that you have correctly installed the x86_64 version of Homebrew as well as the game-porting-toolkit formula.

Now you are ready to install a launcher or individual Windows games into this Wine prefix, see below.

== Commands ==

The dmg ships with three scripts starting with <code>gameportingtoolkit</code> to simplify the use of GPTk.
;A. Standard launching
:<pre>gameportingtoolkit ~/my-game-prefix 'C:\Program Files (x86)\Steam\steam.exe'</pre>
:This launches the given Windows game binary with a visible extended Metal Performance HUD and filters logging to output from the Game Porting Toolkit.
:This is exactly the same as <code>MTL_HUD_ENABLED=1 WINEESYNC=1 WINEPREFIX=~/my-game-prefix $(brew --prefix game-porting-toolkit)/bin/wine64 'C:\Program Files (x86)\Steam\steam.exe'</code>.
;B. Launching without a HUD
:<pre>gameportingtoolkit-no-hud ~/my-game-prefix 'C:\Program Files (x86)\Steam\steam.exe'</pre>
:This is like the above, but without <code>MTL_HUD_ENABLED=1</code>.
;C. Launching with Wine ESYNC disabled
:<pre>gameportingtoolkit-no-esync ~/my-game-prefix 'C:\Program Files (x86)\Steam\steam.exe'</pre>
:This is again like the basic one, except without <code>WINEESYNC=1</code>. ESYNC is good for performance, but may occasionally suffer from compatibility issues.

To run Winecfg:
<pre>gameportingtoolkit ~/my-game-prefix winecfg</pre>

Installing individual exe games is as simple as running the installer using any of the commands above. If it does not require installation, just open your Wine prefix’s virtual C: drive in Finder (<code>open ~/my-game-prefix/drive_c</code>) and copy your game into an appropriate subdirectory.

=== Replacement ===
A defect of <code>gameportingtoolkit</code> is that it only takes two arguments: the prefix and the exe path. This disallows passing any extra argument (e.g. a <code>-dx11</code> or <code> -debug_mode</code> flag) to your exe; going back to the real ''wine64'' program removes this restriction. A custom shell function can be used to simplify the wine64 call:
<pre>
wine-gptk(){ WINEESYNC=1 WINEPREFIX=~/my-game-prefix $(brew --prefix game-porting-toolkit)/bin/wine64 "$@"; }
</pre>

The above function can be written to <code>~/.zshrc</code> so it stays loaded in the shell. <code>wine-gptk 'C:\Program Files (x86)\Steam\steam.exe'</code> would be functionally the same as example B. The difference is that you can pass arguments like <code>wine-gptk notepad 1.txt</code> or <code>wine-gptk imperator.exe -debug_mode</code>.

(The issue does not prevent gaming platforms like Steam from launching games with the required flags. It only prevents you from passing flags to the exe you are directly opening.)

== Installing gaming platforms==
=== Steam ===
Download the [https://cdn.cloudflare.steamstatic.com/client/installer/SteamSetup.exe Windows version of Steam] and place in your Downloads folder.

'''Install Steam'''
<pre>gameportingtoolkit ~/my-game-prefix ~/Downloads/SteamSetup.exe</pre>

'''Run Steam'''
<pre>gameportingtoolkit ~/my-game-prefix 'C:\Program Files (x86)\Steam\steam.exe'</pre>

'''Log into Steam'''

A common issue is that Steam will present with a blank black window. Try the [[#Steam login black screen]] section.

=== Battle.net ===
Download the [https://download.battle.net/en-gb/?platform=windows Windows version of Battle.net] and place in your Downloads folder.

Make a new Wineprefix for Battle.net, you can choose to use <code>my-game-prefix</code> or change this to anything else.
<pre>WINEPREFIX=~/my-game-prefix $(brew --prefix game-porting-toolkit)/bin/wine64 winecfg</pre>
*A “Wine configuration” window should appear on your screen.
*Change the version of Windows to <code>Windows 10</code>.
*Choose Apply and then OK to exit winecfg.

If you are running Diablo IV, then you need to run this script to update the Wineprefix to appear as a more recent version of Windows:
<pre>
WINEPREFIX=~/my-game-prefix $(brew --prefix game-porting-toolkit)/bin/wine64 reg add 'HKEY_LOCAL_MACHINE\Software\Microsoft\Windows NT\CurrentVersion' /v CurrentBuild /t REG_SZ /d 19042 /f
WINEPREFIX=~/my-game-prefix $(brew --prefix game-porting-toolkit)/bin/wine64 reg add 'HKEY_LOCAL_MACHINE\Software\Microsoft\Windows NT\CurrentVersion' /v CurrentBuildNumber /t REG_SZ /d 19042 /f
WINEPREFIX=~/my-game-prefix $(brew --prefix game-porting-toolkit)/bin/wineserver -k
</pre>

'''Run Battle.net launcher'''
<pre>gameportingtoolkit ~/my-game-prefix ~/Downloads/Battle.net-Setup.exe</pre>

Be aware that there is an issue launching Battle.net once installed, the only current way to re-login is to 'install' the launcher again. 

Start individual game without the launcher using this command:
<pre>arch -x86_64 gameportingtoolkit-no-hud ~/my-game-prefix 'C:\Program Files (x86)\Diablo IV\Diablo IV Launcher.exe'</pre>

=== Epic / GOG.com / Amazon Prime Gaming support with the Heroic Games Launcher ===
This is particularly useful because '''as it currently stands, the real Epic Games Launcher fails to install under the Game Porting Toolkit'''. Heroic supports Epic / GOG / Amazon Prime games.

# Install the [https://github.com/Heroic-Games-Launcher/HeroicGamesLauncher/releases native macOS Heroic Games Launcher] or from Homebrew [Requires 2.9.0 and/or newer]:
#* Ensure you're using ARM zsh:<pre>arch -arm64e zsh</pre>
#* Install Heroic's cask:<pre>brew install --cask heroic</pre>
#Go to <code>Settings</code> > <code>Game Defaults</code> > Make sure GPTK is selected as preferred method.
#Check <code>ESYNC</code> and <code>Show Stats Overlay</code> eventually you can use environment variables explained above.
#Make sure your Game Porting Toolkit Wine Prefix is selected. If you followed Apple's guide this is <code>/Users/$USER/my-game-prefix</code>.
#If you open <code>Settings</code> > <code>Other</code> section you can also enable <code>ESYNC</code> and <code>Metal HUD</code> by add any environment variables you want like <code>WINEESYNC=1</code> and <code>MTL_HUD_ENABLED=1</code>. Removing them or setting to <code>0</code> will disable them.
#Install and test desired game.

== Shortcut ==
{{Image|Game Porting Toolkit Steam icon.png|Icon made by user Omsier.|150px}}
You can make a shortcut for Steam for example by using macOS built-in Automator app.
*Open Automator.
*Select New Application.
*In the 2nd column select Run Shell Script.
*Copy and paste this code into the box:
<pre>
#!/bin/zsh

export PATH="/usr/local/bin:${PATH}"

arch -x86_64 gameportingtoolkit ~/my-game-prefix 'C:\Program Files (x86)\Steam\steam.exe' > /dev/null 2>&1 &
</pre>
*Save the shortcut somewhere e.g. Applications.
*Above shortcut can be customised to any Wineprefix or game.
*Add an icon by selecting the shortcut, pressing {{key|cmd|I}} to Get Info, then dragging and dropping a PNG file onto the top left of the window.

== Logging ==
The provided bin/gameportingtoolkit* scripts can be copied onto your path to facilitate different forms of logging and launching. You can run these scripts from any shell; you don’t need to switch to the Rosetta environment first.

Logging output will appear in the Terminal window in which you launch your game as well as the system log, which can be viewed with the Console app found in Applications ▸ Utilities. Log messages from the Game Porting Toolkit are prefixed with D3DM. By default the gameportingtoolkit* scripts will filter to just the D3DM-prefixed messages.

== Troubleshooting ==
=== Steam login black screen ===
{{Image|GPTK Steam black screen.png|Steam login black screen issue.}}

{{Fixbox|description=Alternate Steam launch command|ref=|fix=
Close the Terminal window and then reopen and retry the command, repeat several times.

<!-- Isn't this the same as the one above in "Steam"? -->
Alternate way of launching Steam (after installing):
<pre>
MTL_HUD_ENABLED=1 WINEESYNC=1 WINEPREFIX=~/my-game-prefix /usr/local/Cellar/game-porting-toolkit/1.0.4/bin/wine64 'C:\Program Files (x86)/Steam/steam.exe'
</pre>

If still not working then try using [[CrossOver]] and create a Steam bottle, then login, then redirect this WINEPREFIX to that bottle:
<pre>
WINEPREFIX="/Users/$USER/Library/Application Support/CrossOver/Bottles/Steam/" /usr/local/Cellar/game-porting-toolkit/1.0.4/bin/wine64 'C:\Program Files (x86)/Steam/steam.exe'
</pre>
}}

{{Fixbox|description=Use macOS Steam login|ref=<ref>https://github.com/IsaacMarovitz/Whisky/issues/41#issuecomment-1585640483</ref>|fix=
#Login to Steam macOS.
#From <code>/Users/$USER/Library/Application Support/Steam</code>, copy three files/folders: config, registry.vdf, userdata.
#Paste into <code>~/my-game-prefix/drive_c/Program Files (x86)/Steam/</code>

Alternatively, login to Steam macOS and paste this into terminal: 
<pre>
cp -R $HOME/Library/Application\ Support/Steam/{config,registry.vdf,userdata} "$HOME/my-game-prefix/drive_c/Program Files (x86)/Steam/"
</pre>
}}
=== Battle.net login black screen ===

try Whisky install.</br>
open the Whisky -> Battle.net bottle volume -> Bottle Configuration -> DXVK-DXVK enable</br>
After restarting Battle.net, you'll still have to wait no more than 10 minutes, during which time the screen will remain black until the icons load.

=== Steam crashes straight after opening ===
{{Fixbox|description=Disconnect external monitors|ref=<ref>Reference</ref>|fix=
Disconnect any external monitors, or alternatively stop mirroring the screen.
}}

=== Battle.net launcher won't re-launch ===
Re-install the launcher to reopen, no other fix at the moment.

===Windows version too old===
{{Fixbox|description=Windows version too old|ref=<ref>Reference</ref>|fix=
'''My game won’t run because it thinks the version of Windows is too old. Some games detect specific minimum versions of Windows and need to be updated.''' Use this script to update your wineprefix with build 19042 which should work for most games e.g. Spider-Man Remastered.
<pre>
WINEPREFIX=~/my-game-prefix $(brew --prefix game-porting-toolkit)/bin/wine64 reg add 'HKEY_LOCAL_MACHINE\Software\Microsoft\Windows NT\CurrentVersion' /v CurrentBuild /t REG_SZ /d 19042 /f
WINEPREFIX=~/my-game-prefix $(brew --prefix game-porting-toolkit)/bin/wine64 reg add 'HKEY_LOCAL_MACHINE\Software\Microsoft\Windows NT\CurrentVersion' /v CurrentBuildNumber /t REG_SZ /d 19042 /f
WINEPREFIX=~/my-game-prefix $(brew --prefix game-porting-toolkit)/bin/wineserver -k
</pre>
}}

=== steamwebhelper.exe crashes ===
This is caused by Steam being run through macOS Ventura or below, most users should upgrade to macOS Sonoma. You can use the CrossOver workaround to login to Steam but you'll experience constant crashing.

=== Nothing loads after a crash ===
Try closing all Wine threads:
<pre>killall -9 wineserver && killall -9 wine64-preloader</pre>

=== Anti-cheat and DRM ===
Anti-cheat (e.g. Easy Anti Cheat) and DRM (e.g. Denuvo) are long known to be incompatible with wine, because they hook into undocumented nooks and crannies in the Windows system.  Some games have workarounds e.g. Elden Ring. For other games, you need to wait until the DRM is removed by the game's developer.

=== My game won’t run because it requires Mono, .NET, or the MSVCRT runtime ===
For the example of VC Redistributable, download the x64 version here: https://aka.ms/vs/17/release/vc_redist.x64.exe

<pre>
WINEPREFIX=~/my-game-prefix $(brew --prefix game-porting-toolkit)/bin/wine64 vc_redist.x64.exe
</pre>

Generic commands:

<pre>
WINEPREFIX=~/my-game-prefix $(brew --prefix game-porting-toolkit)/bin/wine64 <some-installer.exe>
</pre>

And .MSI packages can be installed by launching the Windows uninstaller application and choosing to install a downloaded .msi package:
<pre>
WINEPREFIX=~/my-game-prefix $(brew --prefix game-porting-toolkit)/bin/wine64 uninstaller
</pre>

===My game won’t boot anymore even though I made no changes.===

If the game stopped booting without being updated, you can try clearing the D3DMetal shader cache. 
Run the following commands:

 cd $(getconf DARWIN_USER_CACHE_DIR)/d3dm
 cd ''GAME_NAME''
 rm -r shaders.cache

Do you have a different problem or other feedback?

Please let us know through https://feedbackassistant.apple.com.

=== Controller issues ===
{{Fixbox|description=Steam beta|ref=<ref>Reference</ref>|fix=
Issues may be fixed by enrolling into the Steam beta.
}}
{{Fixbox|description=Steam Big Picture Mode Controller Settings|ref=<ref>Fix EVERY controller issue on M1 Mac! video on YT by Andrew Tsai (bylk5VRlJPE) </ref>|fix=
Right click on the game -> Properties -> Controller -> Override for "Game Name" -> Disable Steam Input
}}
{{Fixbox|description=Preventing Launchpad from opening when pressing home|fix=
This is useful especially when using Steam's Big Picture mode.<br>
Paste this command in terminal :<br>
<pre>defaults write com.apple.GameController bluetoothPrefsMenuLongPressAction -integer 0</pre>
Disabling share button (Optional) :<br>
<pre>defaults write com.apple.GameController bluetoothPrefsShareLongPressSystemGestureMode -integer -1</pre>
}}

=== My game looks pixelated and the display resolution is limited ===
{{Fixbox|description=Enable Retina mode|ref=|fix=
Enable Retina (high resolution) mode:
<pre>
WINEPREFIX=~/my-game-prefix $(brew --prefix game-porting-toolkit)/bin/wine64 reg add 'HKEY_CURRENT_USER\Software\Wine\Mac Driver' /v RetinaMode /t REG_SZ /d 'Y' /f
</pre>
Some games will not run with Retina mode enabled. To disable it:
<pre>
WINEPREFIX=~/my-game-prefix $(brew --prefix game-porting-toolkit)/bin/wine64 reg add 'HKEY_CURRENT_USER\Software\Wine\Mac Driver' /v RetinaMode /t REG_SZ /d 'N' /f
</pre>
}}

===AVX===
AVX and AVX2 games are not supported by Rosetta on Apple Silicon (arm64). Such games include:

* [[Samurai Showdown]],
* [[Dead Stranding]],
* [[Assassin's Creed: Odyssey]],
* [[Yakuza Remastered Collection]],
* [[Yakuza: Like A Dragon]],
* [[Saints Row: The Third Remastered]],
* [[Left Alive]] (Requires FMA),
* [[Dying Light 2]],
* [[Madden NFL 22]],
* [[Lost Judgment]],
* [[WWE 2K22]],
* [[Ratchet & Clank: Rift Apart]],
* [[Sonic Frontiers]],
* [[Starfield]]

List may contain games with Mac port available or with already known AVX bypass fixes. Feel free to update this section just like in Dying Light 2's case.

AVX requirement can be bypassed in some games, e.g Dying Light 2.

{{Fixbox|description=AVX Fix for Dying Light 2|ref=<ref>Reference</ref>|fix=
There is a workaround in Dying Light 2 which is to simply remove/rename the "runtime_dx11.dll"

#Right click on DL2 on steam
#Manage
#Local files
#Navigate to "ph\work\bin\x64"
#Rename "runtime_dx11.dll" to "runtime_dx11.dll.bak"
#Start the game

If for some reason you need the "runtime_dx11.dll" back, just remove the ".bak" or verify game files integrity in steam
}}

== Game compatibility list ==
{{ii}} '''Total number of games: '''{{#cargo_query:tables=Compatibility_macOS|fields=COUNT(*)|group by=_pageNamespace|where=Compatibility_macOS.wine='perfect' OR Compatibility_macOS.wine='playable' OR Compatibility_macOS.wine='runs' OR Compatibility_macOS.wine='menu'}}
{{mm}} ''This list was last refreshed on '''{{CURRENTMONTHNAME}} {{CURRENTDAY}},  {{CURRENTYEAR}}'''. [{{fullurl:{{FULLPAGENAMEE}}|action=purge}} Purge] the page to refresh it.''

''
Note: this list is querying '''any''' Wine compatibility, not specifically GPTK. Compatibility status on this table does not necessarily represent GPTK compatibility.
''

{{#cargo_query:
tables=Compatibility_macOS
|fields=Compatibility_macOS._pageName, Compatibility_macOS.wine
|where=Compatibility_macOS.wine='perfect' OR Compatibility_macOS.wine='playable' OR Compatibility_macOS.wine='runs'
|limit=500
|offset=0
|format=template
|intro={{ListOfGames/intro}}
|template=ListOfGames/row
|outro=</table></div>
}}

{{#cargo_query:
tables=Compatibility_macOS
|fields=Compatibility_macOS._pageName, Compatibility_macOS.wine
|where= Compatibility_macOS.wine='unplayable' OR Compatibility_macOS.wine='menu'
|limit=500
|offset=0
|format=template
|intro={{ListOfGames/intro}}
|template=ListOfGames/row
|outro=</table></div>
}}

Old list - this should be made into individual wiki articles in order to populate the automated list above, which tracks all game compatibility methods using Wine.

Working games:
* [[AnimA: Reign of Darkness]]
* [[Astroneer]]
* [[Assassins Creed 3 Remastered]]
* [[Atomic Heart]]
* [[ARK: Survival Evolved]] - (~50 fps on low settings)
* [[Batman: Arkham Knight]] - launches with steam no-esync or crash
* [[BeamNG.drive]] - Disable ESync. Grass textures have blue artifacts on default maps. Download external maps to fix grass.
* [[Borderlands 3]]
* [[Blood: Fresh Supply]]
* [[Bloodstained: Ritual of the Night]]
* [[Brick Rigs]]
* [[Bright Lights of Svetlov]]
* [[Genshin Impact]] 3.7 (m2max with 4k 60fps stable) 
* [[Chasm: The Rift]] (It works but you can't change any video options as it will crash. You have to manually edit the config file.)
* [[Choo Choo Charles]] - (40-50fps on base M1)
* [[Control]] (DX12 mode, if downloaded from Heroic, needs to be ran by Terminal)
* [[Crysis Remastered]]
* [[Cultic]]
* [[Cuphead]] - (~90fps on M2 Pro 12CPU/19GPU)
* [[Cyberpunk 2077]]
* [[Days Gone]]
* [[Dead Space Remake]]
* [[Deep Rock Galactic]]
* [[Deceive Inc.]] - works well if launched without EAC
* [[Derail Valley]] (awesome performance, no missing manuals - in-game objects for train operation - like on CrossOver)
* [[Diablo IV]] [https://www.reddit.com/r/macgaming/comments/14307be/comment/jn7dxzo/?utm_source=reddit&utm_medium=web2x&context=3 ]
* [[DJMAX Respect V]] - Playable via Steam with Whiskey, which includes GPTK; BGAs do not appear during gameplay or in-menu; menu is currently a black screen so changing modes isn't feasible but Freestyle/default loads fine when hitting enter to get to song selection, which works as expected; no crashes on video setting changes (fullscreen <> windowed, different resolutions); recommend unlimiting framerate (vsync causes some visual stuttering but doesn't affect input or create lag; tested on M1 Pro, 16GB
* [[Dread Templar]]
* [[Duke Nukem 3D HRP (eduke32)]]
* [[Dying Light 2]] (needs AVX fix above)
* [[Dyson Sphere Program]] (some objects and main character weren't visible before)
* [[Elden Ring]]
* [[Final Fantasy VII Remake Intergrade]] (~50 FPS on High settings at 1080p with M1 Pro)
* [[Five Nights At Freddy's Security Breach]]
* [[Friends vs. Friends]]
* [[Gloomwood]]
* [[God of War]] (Works somewhat well on M1 Pro (16gb), wouldn't recommend lesser hardware.)
* [[Going Medieval]]
* [[Guilty Gear Strive]]
* [[Grand Theft Auto V]]
* [[Green Hell]]
* [[Halo 3]] (MCC - No Online due to Easy Anti-Cheat Compatibility) [https://www.reddit.com/r/macgaming/comments/143zoos/halo_3_mcc_no_online_eac_m1_pro_game_porting/]
* [[Halo Reach]] (MCC) 
* [[HI-Fi RUSH]]
* [[Hogwarts Legacy]] - launches fine first time but then won't relaunch - can be fixed by deleting ~/my-game-prefix/drive_c/ProgramData/Hogwarts Legacy - these are the files that are created at first launch that prevent relaunching
* [[Hatsune Miku: Project DIVA Mega Mix+]] - works with Retina mode; [https://github.com/blueskythlikesclouds/DivaModLoader/releases DivaModLoader] and [https://github.com/Still34/azura-diva/releases DivaNoSpy] recommended; run Steam with WINEDEBUG=-all WINEESYNC=0 WINEDLLOVERRIDES="dinput8.dll=n,b"; may randomly crash at the title screen: use [https://github.com/nastys/IntroPatch/releases IntroPatch] as a workaround
* [[Infinity Racer XD]]
* [[Jected - Rivals]]
* [[Kena: Bridge of Spirits]]
* [[Metal Gear Solid V: The Phantom Pain]]
* [[Only Up!]] - (Macbook Air 13,M1,16GB,7GPU) Playable through Steam at 1440x900, lowest settings, vsync off, resolution scale: 25-30%, fps lock: 30. Gives 30 fps with drops to 25 when loading map. Without lock, with other apps closed could give 30-60fps with lowest settings. The higher the hero climbs, the more memory used. At highest point I could climb to memory usage was about 11.5GB.
* [[Outer Wilds]] (through steam)
* [[Overwatch 2]]
* [[QUBE 2]]
* [[Party Animals]]
* [[Payday3]]
* [[People Playground]]
* [[Poppy Playtime Chapter 1]]
* [[Poppy Playtime Chapter 2]]
* [[PowerSlave Exhumed]]
* [[Prodeus]]
* [[Project Warlock]]
* [[Prey]] - Fixed the black textures with gptk beta version 1.03. Run with Heroic
* [[Ready or Not]]
* [[Return To Castle Wolfenstein (RealRTCW 4.0.12)]]
* [[Rise of The Triad: Ludicrous Edition]]
* [[Risk of Rain 2]] (does not require `-disable-gpu-skinning` like Crossover 22)
* [[Scarlet Nexus]]
* [[Sonic GT]]
* [[Sonic illusions]]
* [[Sonic Omens]]
* [[Sonic Project '06]]
* [[Spider-Man Miles Morales]] - ''requires Windows ver fix''
* [[SpongeBob SquarePants: The Cosmic Shake]]
* [[StarCraft: Remastered]]
* [[Tetris Effect: Connected]] - Game window likes to be uncooperative; really doesn't like retina mode, works otherwise
* [[The Mortuary Assistant]]
* [[Tiny Tina Wonderlands]]
* [[Turbo Overkill]]
* [[Turok 2: Seeds of Evil (Remastered)]]
* [[V Rising]]
* [[Viewfinder]] - Macbook air m1 8gb 60fps most of the time at max settings 1680x1050 resolution
* [[Warframe]] - ''To get installer/launcher working add dwrite (disabled) to library overrides in winecfg''
* [[WRATH: Aeon of Ruin]]
* [[Vampyr]] - 25-30fps on Ultra settings, tested on M1 MBA (DX11 game!)
* [[Tell Me Why]] - On maxed out settings, runs around 35-45fps on base M1 MBA (DX11 game!)
* [[It Takes Two]] - Semi-frequent stuttering, but runs well and isn't an issue for such a casual game.

Not working well:
* [[Horizon Zero Dawn]] - slowdown issues
* [[Genshin Impact]] - Launches, but stuck when calling menu using esc
* [[CarX Drift Racing Online]] - Game freezes after launch.
* [[Fallout 4]] - Game starts and runs well on m1 Pro at high settings, very smooth movement (wasd) but mouse is not picked up correctly. Looking around is not really a thing. Mouse works fine in menu
* [[FallGuys through Heroic]] - Game and anticheat launch, but game only shows black screen for a few seconds then crashes. Cursor gets changed to custom cursor

Not working yet:
*[[Street Fight 6 Demo]] (it will show black screen because of D3Dmetal error)
* [[Apex Legends]] (it will stuck because of anti-cheat)
* Any games that use the EA Launcher
** [[Need for Speed Heat]]
** [[Need for Speed Unbound]]
** [[Tom Clancy's The Division 2]]
* [[Among Us]]
* [[Assassins Creed Odyssey]]
* [[Assassins Creed Valhalla]]
* [[Assetto Corsa]] - Stuck on "Installing DirectX Runtime" and when it passes, it says running but then just stops
* [[Company of Heroes 3]]
* [[Death Stranding]]
* [[Doom (2016)]] - "FATAL ERROR: wglCreateContextAttribsARB failed" message on launch
* [[Kerbal Space Program 2]] - Loads into Main Menu but crashes on a step when loading into KSC and crashes with no reports
* [[F1 20]] - Crash on launch (DX11 Version Works)
* [[F1 21]] - Crash on launch
* [[F1 22]] - Crash on launch (  "Exception": "C0000005 EXCEPTION_ACCESS_VIOLATION at 0000000141bd8687")
* [[Grand Theft Auto IV]] - Rockstar games launcher crashes on launch, cracked version should work fine
* [[Halo Infinite]] - Crash on launch
* [[Hitman 3]] - launcher works, crash at 0x0000014136dddd hitman3+0x136dddd: int $3
* [[Microsoft Flight Simulator 2020]] (Steam) - Crash on launch
* [[Monster Hunter Rise]] - DX12 Game, it launches but window goes dark after compiling shaders.
* [[Monster Hunter World]] - Black Screen on Launch
* [[New World]] - Game loaded but couldn't connect to server due to Easy Anti Cheat
* [[Payday 2]]
* [[Red Dead Redemption 2]]
* [[Resident Evil]] (HD remaster, 2015) – crashes right after CAPCOM screen
* [[Resident Evil 5]] – crashes right after launching
* [[Rogue Company]] - anti-cheat conflict.
* [[s&box]] - crashes right on launch. (it should be DX11)
* [[Sea of Thieves]] - Launches, but the prompt to sign into an xbox live account appears blank and cannot
* [[Saints Row IV: Re-Elected]] - Launches, but crashes when trying to load into game
* [[Slime Rancher 2]] - Launches, but can't into game from main menu
* [[Sonic Frontiers]] - AVX Requirement
* [[SpongeBob Squarepants: Battle for Bikini Bottom Rehydrated]] - Crashes when loading 
* [[Teardown]] - When trying to launch, gives a error, "The program teardown.exe has encountered a serious problem and needs to close. We are sorry for the inconvenience."
* [[The Elder Scrolls V: Skyrim Special Edition]] - the launcher gives a "The program skyrimselauncher.exe has encountered a serious problem" message, not allowing us to get into the game
* [[theHunter: Call of the Wild]] - Crash on launch (Unhandled exception: page fault on read access to 0x0000000000000000 in 64-bit code (0x00000140b227f8))
* [[The Joy of Creation: Story Mode]] - Crash on launch
* [[Uncharted - Legacy of Thieves]] - Requires AVX
* All DX9 games (as this toolkit supports DX11 & DX12 only)
** [[30XX]] - "Failed to create D3D device" -> crash on launch; this is a rare 64-bit DX9 game
** [[Battleblock Theater]] - Black screen -> crash on launch
** [[Black Mesa]] - Starts to load to main menu at first but then crashes
** [[Castle Crashers]] - Black screen -> crash on launch
** [[Killing Floor 1]] - Crash on launch
** [[Left 4 Dead 2]] - Black screen -> crash on launch
** [[Lego Batman: The Videogame]] - Black screen -> crash on launch
** [[Lego Batman 2: DC Super Heros]] - Black screen -> crash on launch
** [[Lego Batman 3: Beyond Gotham]] - Black screen -> crash on launch (You can switch to DX11 but are currently unable to do so since the game will not launch)
** [[Mega Man Zero/ZX Legacy Collection]] - "Failed to create d3d9 device" -> crash on launch
** [[Sonic Mania]] - Crash on launch
** [[Space Engineers]] - Gets to first loading screen then crashes
** [[StarCraft II]] -> "Failed to initialize DirectX" error
** [[Star Wars: The Old Republic]]
** [[Battlefield 2]] - DirectX installation failed, won't launch, see the [https://gist.github.com/chrismwendt/1d01a306cdc8618a06bdfb555743e25c error log]



{{References}}
[[Category:Lists]]
