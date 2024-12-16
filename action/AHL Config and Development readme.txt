************************
*    AHL CVAR List     *
************************

clientside
----------

hud_fastswitch 1/0 - fast weapon switching
crosshair n - choose your crosshair
eyecandy n - how many effects happen on screen at once. Can cause "too many particles" if set too high
r_decals n - number of decal effects. Set it to 4096 if your system can handle it. AHL loves decals.
gore_level n - how much blood/gibs appear

cl_wallfx 1/0 - wall debris on/off
cl_wallfxscale n - size of the debris
cl_muzzlefx - First person muzzle flashes on/off
cl_externalmuzzlefx - 3rd person muzzle flashes on/off
cl_bloodfx - blood particles on/off

cl_forcefps 1/0 - force rush mode to stay in first person
cl_autotime 1/0 - automatic rush mode on/off
cl_slowsound 1/0 - slow sounds down when in rush mode
cl_norushmode 1/0 - never uses rush mode.  Use this if you have wierd things happening in multiplayer, or just dont like rush.

cl_bodystay  n - how long bodies stay on screen in seconds 0 = forever
cl_screentilt n - screentilt 0-8 0 = off 8 = max
cl_menubars 1/0 - removes the help bars from the top + bottom of the screen in spectator mode
hud_takeshots 1/0 - takes a screenshot of the scoreboard at the end of each level

serverside
----------

mp_tkdeal n - how to deal with TKers 2 = kick 1 or 0 = punish
mp_chasecam n - chase cam 0 = off 1 = on 2 = teammates only
mp_allowspectators 1/0 - allows free floating spectators
mp_allowpractice 1/0 - allows gun fights whilst waiting for the round to start
mp_tkbantime n - how long a TKer will be banned for
mp_balanceteams 1/0 - allows you to play with unbalanced teams
mp_forcebalance 1/0 - forces the player to join the team with fewest players
mp_fraglimit n - max kills before level change (DM + LMS Only)
mp_timelimit n - max time before a level change
mp_roundtimelimit n - max time a round can last
mp_forcerespawn 1/0 - whether a player is forced to respawn in DM/roundless TP
mp_gametype 1-5 - different game modes
mp_friendlyfire 1/0 - whether team mates can cause damage

sv_logdamage 1/0 - writes the damage caused to each player to the logs
sv_logchat 1/0 - writes conversations to the logs

sv_floodcount n - flood prevention, you can say x things in y seconds.  This is the x.
sv_floodtime n - flood prevention, you can say x things in y seconds.  This is the y.

bot_snipe 1/0 - when running bots
bot_wp_edit 1/0 - turns on bot editing mode
sv_wpedit 1/0 - allows the server/computer to edit bot waypoints
mp_bots n - adds bots.  Enter a number.
kickbot "name" - kicks the bot you name.  Using kick rather than kickbot will make ahl crash (dont know why :( )

************************
*  Creating Overviews  *
************************

The way to make overview files doesnt work completely like it says anymore.  Valve changed things slightly.
The new way is more fiddly, in order to prevent script kiddies hacking the code I think...

Heres how it works, (basically)

Start a half-life DEDICATED server (dedicated is important!!).  Make sure bots are turned off
Start a hltv proxy, and point it at your server.
On the server, change level to the level you want to make an overview for (changelevel ahl_xx)
Then start a half-life game, and connect to the proxy, using the hltv slot (its the one that has no text on it at the moment).

type developer 1
change views a couple of times, this should get rid of the "help text borders".  Keep going until you are come to a big green grid.

type dev_overview 1
This should put you in the top down mode, if all you get is a green screen, you probably didnt switch to overview mode before typing dev_overview, type dev_overview 0, and cycle through the different views again and it should work.

Cut and paste from the doc here (coz its still the same)

You can move the map around with keys FORWARD,BACKWARD, RIGHT & LEFT. Maps will be automatically rotated to fit optimally onto the screen. If they are rotated, FORWARD and RIGHT are swapped, etc. Use the keys ATTACK and ATTACK2 to change the zoom. With zmin and zmax you can choose which parts of the map should be drawn. Only polygons between these two values are shown. This can be used to cut off ceilings or other unwanted objects. Origin is your viewpoint projected into the map. The map will be rotated around this point if you move the mouse in overview mode. Zmin can be changed with UP & DOWN and zmax with JUMP & DUCK. If you think you've found the correct values for the new map, simply make a screenshot and remember all of the values, which are needed for the overview description file (to avoid console text on the screenshot set developer 0).

/end cut&paste

Align your map to fit the screen.  Note down all of the numbers to do with the position of the camera

Grab screenshots of the map.  If necessary, move up and down to grab screen shots of the different levels of the map, then use photoshop to merge them together (use layers and the eraser tool) or add neat effects (eg, no-credit could be re-done to look like a sketch of the map, as seen in the petrol station, or other maps could be changed to look like security blue-prints or something).

Save it as the mapname.tga, and place it into the overviews directory.  Take one of the map.txt files from one of the other mods (DMC, TFC, HL), rename it to your maps name.txt, open it, and fill out the values with the ones you noted down earlier...

cut and paste again...

Next, you have to create an overview description file that will be parsed by the client DLL to get the map image filename and the correct view parameters. This file must be in the overviews\ directory and must have the same name as the current BSP file, but with TXT as its extension. Here is an example (overviews\de_vegas.txt):

// overview description file for de_vegas.bsp

global 
{
	ZOOM		1.63
	ORIGIN	1903 	-1283	-1088
	ROTATED	0
}

layer 
{
	IMAGE	"overviews/de_vegas.tga"
	HEIGHT	-1088
}

The global section describes zoom, the origin point the map is rotated around, and a boolean to indicate if the map image is rotated or not. The layer section describes the map image filename and at which height (z-axis) the map should be shown. The overview mode uses the some coordinate space as the game world and all icons are drawn at the same position as their corresponding entities

/end cut&paste

Then when you join a hltv server, you should find when you switch to a map view there is a picture of the map, rather than a green grid.

I have to say, the biggest problem I've found is that the entities on the map sometimes didn't relate to the position of the entities in the world.  The worst one for this is hondos maps (I tried doing a no-credit one.  It seems ok now..), because the secrets make the maps much larger than the overviews are.  I think the best bet is to make sure the HEIGHT field has the correct value for whatever you used as zmax.  Then watch the game with some bots or something to see if you have the origin set up correctly, if not, adjust the origin, and reload the level.  (this is where running the game in a window comes in useful, add -window 640 480 to your commandline)

******************************
*    AHL map entity help     *
******************************

Rough write up on changes to normal HL map entities and info on any new ones.  
Pretty much the same as beta 4 as map stuff is still in development and testing...


 ITEM/WEAPON/AMMO CLASSNAMES :
        weapon_anaconda
        weapon_beretta
        weapon_akimbob    - Akimbo Berettas
        weapon_colt
        weapon_akimcolt   - Akimbo Colt 1911
        weapon_de50
        weapon_saa
        weapon_akimsaa    - Akimbo SAAs
        ammo_pistol       - Generic ammo, includes ammo for all pistols

        weapon_hkmp5
        ammo_mp5clip

        weapon_50cal
        ammo_50cal
        ammo_sniper

        weapon_m4
        ammo_m4

        weapon_msg90
        ammo_msg90
        ammo_766mm

        weapon_handcannon
        weapon_ithaca
        ammo_shells
        ammo_buckshot

        weapon_knife
        weapon_frag

        item_vest
        item_laser
        item_stealth
        item_silencer
        item_bandolier
        item_flashlight
        item_nightvision


 On Targetnames :
  HL automatically checks for certain targets when a client dies, connects etc.
        game_playerleave - fired by a player when they disconnect from the server

        game_playerjoin - fired when a player when they first JOINS a server

        game_playerspawn - fired each time the player spawns

	game_playerdie - fired by the player that died

        game_playerkill - fired by the player that killed another player
		(fire for just killing... even if you killed a team mate)

	*NEW*	
	game_playerscore - fired when you GET a frag
		(In teamplay, this means killing a non-team member)

	game_roundtimeover - fire when the time runs out 
		(saves usings some multimanager)
	*NEW*	


================
all map ent's

 ***IMPORTANT***
	If you wish your map to work well for both Teamplay, goals AND deathmatch,
	make sure you add the flag 'noreset' with the bitflag '2' to every ent
	that should be removed when the game is not suppose to use the goals
	(Like in deathmatch). This goes for any added special spawn points,
	trigger_hurt's, items etc.

================
 Additions :
	noreset - bit flag
		1 - entity does not reset when the round does	
		2 - entity should be removed in DM or when 'mp_goalsoff' is set
		4 - entity should be removed in TEAMPLAY
		8 - entity should be removed if Rounds of off (DM or old style teamplay)

================
game_team_master
================
 Changes :
  teamindex <1-??>
	There is no team at index 0 anymore.

  teamname
	Use this instead of teamindex, because its easier to keep track of.
	(Only use if you define the teams first using info_team ent's)

================
game_team_set
================
 Additions :
  teamindex <1-??>
	Set master to this team instead

  teamname
	Same as teamindex, but by name
	(Only use if you define the teams first using info_team ent's)


================
game_score
================
 Info : As soon as you add one of these, normal teamplay takes a back seat
 to what ever you have planned. Normally, the round will end if only one
 team remains alive. Once this entity is added, that is no longer the case.
 If you add one of these, you must also add some game_text to say who won
 etc. The DLL will no longer do that for you.

 Changes : 
	Will only awards points if the game is actually in progress.
	If you intend to end the round with a 'game_roundend', 
	award points at the same time.

 Additions :
  teamindex <1-??>
	Normally, a game_score with the 'TEAM SCORE' spawn flag will award points
	to the team of the player that activates it. By setting 'teamindex', you can
	override this. This is useful if you want to target it with a non-client,
	or perhaps have a goal which can be 'accidently' done by another team
	(such as killing themselves etc).

  teamname
	Same as teamindex, but by name
	(Only use if you define the teams first using info_team ent's)


// NEW ONES
================
game_roundend
================
 Ends the round in a gametype that supports round (Action Teamplay only really)
 There is a slight delay before firing, allowing you to award points etc first.
 Won't fire if the game isn't actually in progress.

 Variables :
  wait <0.1+>
	Delay before the round will try to restart.
	Must be greater then 0 obviously
	Defaults to 3.5 seconds

================
game_player_die
================
 About the only real use of this is to check for the death of a team.
 Everything else it does can be done by anything else really.
	REMEMBER : set the targetname to 'game_playerdie'

 Spawnflags :
  SF_WHOLE_TEAM 1
	Will only fire if the activators entire team is dead

*NEW*
 Variables :
   teamname - if teamname is set and spawnflags SF_WHOLE_TEAM is set,
	then this will only fire when THIS team is all dead. Important
	to set this when dealing with sub teams with max members of '1' etc
	(eg. "The Hunted" style of scenario)
*NEW*

================
info_team
================
 These allow you to set the number of teams, a teams name, model, weapon choices etc.
 It is important to remember one thing : the order you put them in.
 	eg. The first info_team you add will become the team at index 1
 	    The second info_team you add will become the team at index 2
	    The third to team index3 etc

 It's really important for setting a teams ally.

 Action Half-Life only supports 4 teams at the moment.
 So remember to only add four, eh?
	

 Variables :
  name <max 16 chars>
	duh?

  model <max 16 chars>
	hmm, I wonder...

  ally <teamname>
	Makes this team consider the team by this name to be an 'ally'
	That means it will show them as 'friendly' and consider them
	as a team kill etc. This is how you can make 'the hunted' type 
	of scenarios and such.

  max_members <0-?>
	0-1 : A percentage of the biggest other team
		eg. If at 0.6, and the biggest team had 10 ppl, 
		    only 6 ppl could join this team.
	1 : Normals acts just like 1+, except with part of a sub team
		If part of a sub team, this forces the team to be counted
		in the round start check. Useful for making sure there
		is actually a person playing 'the hunted' in that kind
		of scenario
	1+ : Max this amount of ppl may join this team
		Useful for 'the hunted' type of scenarios.
	
  respawn <0-1, 1+>
	0-1 : Wait until this percantage of the team is dead, then respawn
	1+ : wait this many seconds before being able to respawn

  lives <-1+> - number of times someone on this team can die
	-1 : Unlimited lives. Team members can respawn as much as they want
	1+ : Each team member can respawn this many times before remaining dead

  weaponlist <bitvalue> - The weapon choices this team has
	-1 : Anyone joining this team is considered a civie. 
	     They start with no weapons and can't pick em' up either.
        -2 : Anyone joining this team will simply have no weapons at the start.
	     Might be useful for maps with added items or assigning items with
	     game_player_equip's

	Weapon Choice BitValues :
		Beretta 		1
		Anaconda		2
		MP5k			4
		50cal Sniper		8
		HandCannon		16
		Shotgun			32
		Knives			64
		2nd Pistol		128
		M4 Assault Rifle	256
		Semi-Auto Sniper	512

  itemlist <bitvalue> - The item choices this team has
	Item Choice BitValues :
		Vest			1
		Laser Sight		2
		StealthSlippers		4
		Silencer		8
		Bandolier		16
		FlashLight		32
		Night Vision 		64
		No Grenade		128	- no grenade with Bandolier


// Other map entities

================
func_breakable
================
 If it doesn't have a target or targetname, it will respawn in DM.
 In teamplay, it will respawn no matter what when the round resets.

================
tigger_once
================
 Will reset when the round does.
 Definate on this one :)

================================================================
 func_door, trains, plats, buttons etc
================================================================
 Most of these should reset when a round does.
 I have tested lots of doors, plats, buttons etc, some trains and momentary buttons/doors.
 They all seem to work fine. Had a problem with one door on Asylum2 for some reason (freezer)


================
trigger_camera
================
 Additions :
  SF_CAMERA_ALL_PLAYERS - spawnflag 8
        Makes every single player suddenly view through this camera.
        Useful for end credits on a map (RawHyde's Wikked Korp)_


================
all items (ammo/weapons/special items)
================
 Additions :
	wait - > 0.0+ : respawn after this many seconds
			eg. wait 0.1 : near instant respawn
		< 0 : repsawn after -1 * this many seconds, and remain in Action Teamplay
			eg. wait -30 : item still exists in the game
				and will respawn after 30 seconds
	
	master - just like triggers etc. Player must match the neccessary master fields
		to be able to pick it up. Allows team only items etc


================
game_player_message
================
 If a player fires this ent, it will make them send a say_team and/or
 radio message to everyone on their team. It's mainly here to help
 out newbies/streamline games a bit more. Use it/don't use it.

 Variables :
        message - say_team message to send
        radio - radio message to send
                NOTE : if no (message) is given, no one will
                        know who sent the message

================
target_key
================
 Similar to the Quake2 ent 'trigger_key'.
 When fires, searches the activator for a specified game_item.
 If they have at least one of the items, it will fire

 Variables :
        itemname - name of the item to search for. Must be the
                same as what ever game_item you want it to refer too.

 Spawnflags :
  SF_KEEP_ITEM
        Normally, firing a target_key removes the item.
        If KEEP_ITEM is set, it won't.
        This is useful for making security doors and such, where
        you may want players to be able to re-use the item.

================
game_item       **** Untested - Let me know ****
===============
 Used for adding retrievable goals into a map.
 When a player touches a game_item, it checks to see if that
 player is already carrying an item by the same name. If not,
 it gives them one.

 Variables :
  model - set the items model (defaults to "models/flag.mdl")

  *NEW*	
  pmodel - set to the model that will attach to the player for (use with SF_WEAPON)

  target - fired when normal and touched (as apposed to dropped and touched)
  *NEW*	

  skin - set the skin

  targetname - Set so that the item can be USED.
        Using the item toggles it state, depending on how it was used.
        Most things simply reset it (make it respawn).

  initialstate - 0+ - Item starts ON, and touchable
                -1 - Item starts OFF, hidden and untouchable

  itemname - name of the item. Two game_item's CAN have the same
        itemname (for the purpose of restricting items). The way that
        game_item's are kept track of doesn't matter if one or more
        have the same name.

  teamindex <1-??> - HACK : add a teamindex to say who 'owns' the item.
        This is for adding a 'reset' capability if the item is dropped
        and touched by a player on this team. It is also used to
        fire a different target if a player on this team touches it
        while it while it is still available.

  teamname
	Same as teamindex, but by name
	(Only use if you define the teams first using info_team ent's)

  ally_target - HACK : goes with 'teamname/index'. If a player on teamindex team
        touches the item, but does not pick it up, this target will be fired.

  return_target - HACK : goes with 'teamname/index'. If a player on teamindex team
        touches a DROPPED version of the item, but does not pick it up,
        this target will be fired & the original item reset.

  drop_target - If someone carrying the item drops it (through death etc),
        this target will be fired

  respawn -     0 - When dropped, respawn immediately
                1+ - When dropped, wait this many seconds and respawn
                -1 - When picked up, remain normal
                   (don't hide & become untouchable)
        

  respawn_target - If the item respawns on its own (after being dropped),
        this target will be fired


 Spawnflags :
  SF_NULL 1
        Exists only for reference
        If targetted, will 'give' itself to the target
        Use it to give ppl security cards etc

  SF_FOLLOW 2
        Copies itself to the person who touched.
        That copy then follows that person around
        (Like the flags in TFC CTF etc)

  SF_WEAPON 4
	Becomse the players active weapon and looks like they are carrying it.
	Attacking with the item drops it.
		NOTE : Player cannot use another weapon while holding such an item.


================
game_text
================
 Additions :
  teamindex <1-??>
	Send a message to everyone on this team (ONLY!)

  teamname
	Same as teamindex, but by name
	(Only use if you define the teams first using info_team ent's)

  %p - if in the 'message' part, will be substituted with the activators
        name instead. eg. "%p has taken the flag!" would come out as
                        "Mr_Grim has taken the flag!"

  %t - if in the 'message' part, will be substituted with the activators
        teams name instead. eg. "The %t's flag has been taken!" would
        come out as "The SAS's flag has been taken!"



================
monster_victim  **** BROKEN :( ****
================
 Spawns a dude who just sits there and can be shot/used.
 Even if killed/gibbed, it will reset when the round does.
 
 Variables :
	deadtarget - when killed by a player, will fire this target

	target	- if 'used' by a player, fires this target

 	model	- use this model instead of the scientist 
		(must have ACT_IDLE animation)

	health	- set health (default : 100)

*NEW*
        master - set to a game_team_master to make them team specific


================
monster_hostage  **** BROKEN :( ****
================
 Exactly the same as a monster_victim when dealing with variables etc,
 except when a monster_hostage is used, it will follow the user around.
 They will also move out of the way when you walk into them.
 If a player uses them again, they will stop following them.
 For the hostages to get around better, a .wps files should be made
 for the level.


================
trigger_hostage
================
 Just like any other basic trigger (needs a brush model), except this
 one can only be touched by monster_hostages. When they are touched,
 it fires its target based of the person leading the hostage, and then
 it removes the hostage from the game (makes non-solid & invisible etc)


================
game_player_equip
================
 This will override weapon/item selection menu for the player using it.
 (It also disables random item spawn in DeathMatch)
 In a teamgame, it won't (use info_team's item/weaponlist for that).
   For instance : if you wanted everyone to start with just a knife,
	 you could add a game_player_equip with 'weapon_knife 1' set. 
	 When a player spawned, it would simply spawn them in with one 
	 knife, and not bring up the weapon selection menu.
*NEW*

