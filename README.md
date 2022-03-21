# FNF-Porting-Stuff
The Things im using when i port a mod to android

## Instructions:

1. You Need to install AndroidTools and to replace the linc_luajit

To Install Them You Need To Open Command prompt/PowerShell And To Tipe
```cmd
haxelib git AndroidTools https://github.com/jigsaw-4277821/AndroidTools.git

haxelib remove linc_luajit

haxelib git linc_luajit https://github.com/jigsaw-4277821/linc_luajit-nebulazorua.git

```

2. Download the repository code and paste it in your source code folder

3. You Need to add in project.xml those things

On This Line
```xml
	<!--Mobile-specific-->
	<window if="mobile" orientation="landscape" fullscreen="true" width="0" height="0" resizable="false"/>

```

Replace It With
```xml
	<!--Mobile-specific-->
	<window if="mobile" orientation="landscape" fullscreen="true" width="1280" height="720" resizable="false"/>

```

On Those Lines
```xml
	<haxelib name="linc_luajit" if="desktop"/>
	<haxelib name="hxvm-luajit" if="desktop"/>

```

Replace It With
```xml
	<haxelib name="linc_luajit" if="desktop || android"/>
	<haxelib name="hxvm-luajit" if="desktop || android"/>

```

Than, After the Libraries, or where the packeges are located
```xml
	<haxelib name="faxe" if='switch'/>
	<!--<haxelib name="polymod"/> -->
	<haxelib name="discord_rpc" if="desktop"/>

```
add
```xml
        <haxelib name="extension-webview" if="android"/>
        <haxelib name="AndroidTools" if="android"/>

        <config:android permission="android.permission.READ_EXTERNAL_STORAGE" if="android"/>
        <config:android permission="android.permission.WRITE_EXTERNAL_STORAGE" if="android"/>

```

The last thing Before
```xml
	<!-- _________________________________ Custom _______________________________ -->

```
add
```xml
	<!-- Akways enable Null Object Reference check -->
	<haxedef name="HXCPP_CHECK_POINTER" if="release" />
	<haxedef name="HXCPP_STACK_LINE" if="release" />

```

4. Setup the Controls.hx

after those lines
```haxe
import flixel.input.actions.FlxActionSet;
import flixel.input.keyboard.FlxKey;

```
add

```haxe
#if android
import flixel.group.FlxGroup;
import android.FlxHitbox;
import android.FlxVirtualPad;
import flixel.ui.FlxButton;
#end

```

before those lines
```haxe
	override function update()
	{
		super.update();
	}

```

add
```haxe
	#if android
	public var trackedinputs:Array<FlxActionInput> = [];	

	public function addbutton(action:FlxActionDigital, button:FlxButton, state:FlxInputState) 
	{
		var input = new FlxActionInputDigitalIFlxInput(button, state);
		trackedinputs.push(input);
		action.add(input);
	}

	public function setHitBox(hitbox:FlxHitbox) 
	{
		inline forEachBound(Control.UP, (action, state) -> addbutton(action, hitbox.buttonUp, state));
		inline forEachBound(Control.DOWN, (action, state) -> addbutton(action, hitbox.buttonDown, state));
		inline forEachBound(Control.LEFT, (action, state) -> addbutton(action, hitbox.buttonLeft, state));
		inline forEachBound(Control.RIGHT, (action, state) -> addbutton(action, hitbox.buttonRight, state));	
	}
	
	public function setVirtualPad(virtualPad:FlxVirtualPad, ?DPad:FlxDPadMode, ?Action:FlxActionMode) 
	{
		switch (DPad)
		{
			case UP_DOWN:
				inline forEachBound(Control.UP, (action, state) -> addbutton(action, virtualPad.buttonUp, state));
				inline forEachBound(Control.DOWN, (action, state) -> addbutton(action, virtualPad.buttonDown, state));
			case LEFT_RIGHT
				inline forEachBound(Control.LEFT, (action, state) -> addbutton(action, virtualPad.buttonLeft, state));
				inline forEachBound(Control.RIGHT, (action, state) -> addbutton(action, virtualPad.buttonRight, state));
			case UP_LEFT_RIGHT:
				inline forEachBound(Control.UP, (action, state) -> addbutton(action, virtualPad.buttonUp, state));
				inline forEachBound(Control.LEFT, (action, state) -> addbutton(action, virtualPad.buttonLeft, state));
				inline forEachBound(Control.RIGHT, (action, state) -> addbutton(action, virtualPad.buttonRight, state));
			case FULL | RIGHT_FULL:
				inline forEachBound(Control.UP, (action, state) -> addbutton(action, virtualPad.buttonUp, state));
				inline forEachBound(Control.DOWN, (action, state) -> addbutton(action, virtualPad.buttonDown, state));
				inline forEachBound(Control.LEFT, (action, state) -> addbutton(action, virtualPad.buttonLeft, state));
				inline forEachBound(Control.RIGHT, (action, state) -> addbutton(action, virtualPad.buttonRight, state));	
			case DUO:
				inline forEachBound(Control.UP, (action, state) -> addbutton(action, virtualPad.buttonUp, state));
				inline forEachBound(Control.DOWN, (action, state) -> addbutton(action, virtualPad.buttonDown, state));
				inline forEachBound(Control.LEFT, (action, state) -> addbutton(action, virtualPad.buttonLeft, state));
				inline forEachBound(Control.RIGHT, (action, state) -> addbutton(action, virtualPad.buttonRight, state));	

				inline forEachBound(Control.UP, (action, state) -> addbutton(action, virtualPad.buttonUp2, state));
				inline forEachBound(Control.DOWN, (action, state) -> addbutton(action, virtualPad.buttonDown2, state));
				inline forEachBound(Control.LEFT, (action, state) -> addbutton(action, virtualPad.buttonLeft2, state));
				inline forEachBound(Control.RIGHT, (action, state) -> addbutton(action, virtualPad.buttonRight2, state));                        
			case NONE:
		}

		switch (Action)
		{
			case A:
				inline forEachBound(Control.ACCEPT, (action, state) -> addbutton(action, virtualPad.buttonA, state));
                        case B:
				inline forEachBound(Control.BACK, (action, state) -> addbutton(action, virtualPad.buttonB, state));
			case D:
                                //nothing				
			case A_B:
				inline forEachBound(Control.ACCEPT, (action, state) -> addbutton(action, virtualPad.buttonA, state));
				inline forEachBound(Control.BACK, (action, state) -> addbutton(action, virtualPad.buttonB, state));
			case A_B_C:
				inline forEachBound(Control.ACCEPT, (action, state) -> addbutton(action, virtualPad.buttonA, state));
				inline forEachBound(Control.BACK, (action, state) -> addbutton(action, virtualPad.buttonB, state));					
			case A_B_E:
				inline forEachBound(Control.ACCEPT, (action, state) -> addbutton(action, virtualPad.buttonA, state));
				inline forEachBound(Control.BACK, (action, state) -> addbutton(action, virtualPad.buttonB, state));	
			case A_B_X_Y:
				inline forEachBound(Control.ACCEPT, (action, state) -> addbutton(action, virtualPad.buttonA, state));
				inline forEachBound(Control.BACK, (action, state) -> addbutton(action, virtualPad.buttonB, state));		
			case A_B_C_X_Y:
				inline forEachBound(Control.ACCEPT, (action, state) -> addbutton(action, virtualPad.buttonA, state));
				inline forEachBound(Control.BACK, (action, state) -> addbutton(action, virtualPad.buttonB, state));	
                        case A_B_C_X_Y_Z:
                                //nothing
                        case FULL:
                                //nothing
			case NONE:
		}
	}	

	public function removeFlxInput(Tinputs) {
		for (action in this.digitalActions)
		{
			var i = action.inputs.length;
			
			while (i-- > 0)
			{
				var input = action.inputs[i];

				var x = Tinputs.length;
				while (x-- > 0)
					if (Tinputs[x] == input)
						action.remove(input);
			}
		}
	}	
	#end
```

5. Setup MusicBeatState.hx

in the lines you import things add
```haxe
#if android
import flixel.input.actions.FlxActionInput;
import android.AndroidControls.AndroidControls;
import android.FlxVirtualPad;
#end
```

after those lines
```haxe
	inline function get_controls():Controls
		return PlayerSettings.player1.controls;
```

add
```haxe
	#if android
	var _virtualpad:FlxVirtualPad;
	var androidc:AndroidControls;
	var trackedinputs:Array<FlxActionInput> = [];
	#end
	
	#if android
	public function addVirtualPad(?DPad:FlxDPadMode, ?Action:FlxActionMode) {
		_virtualpad = new FlxVirtualPad(DPad, Action);
		_virtualpad.alpha = 0.75;
		add(_virtualpad);
		controls.setVirtualPad(_virtualpad, DPad, Action);
		trackedinputs = controls.trackedinputs;
		controls.trackedinputs = [];
	}
	#end

	#if android
	public function addAndroidControls() {
                androidc = new AndroidControls();

		switch (androidc.mode)
		{
			case VIRTUALPAD_RIGHT | VIRTUALPAD_LEFT | VIRTUALPAD_CUSTOM:
				controls.setVirtualPad(androidc.vpad, FULL, NONE);
			case DUO:
				controls.setVirtualPad(androidc.vpad, DUO, NONE);
			case HITBOX:
				controls.setHitBox(androidc.hbox);
			default:
		}

		trackedinputs = controls.trackedinputs;
		controls.trackedinputs = [];

		var camcontrol = new flixel.FlxCamera();
		FlxG.cameras.add(camcontrol);
		camcontrol.bgColor.alpha = 0;
		androidc.cameras = [camcontrol];

		androidc.visible = false;

		add(androidc);
	}
	#end

	#if android
        public function addPadCamera() {
		var camcontrol = new flixel.FlxCamera();
		FlxG.cameras.add(camcontrol);
		camcontrol.bgColor.alpha = 0;
		_virtualpad.cameras = [camcontrol];
	}
	#end
	
	override function destroy() {
		#if android
		controls.removeFlxInput(trackedinputsUI);
		controls.removeFlxInput(trackedinputsNOTES);	
		#end	
		
		super.destroy();
	}
```

6. Setup MusicBeatSubstate.hx

in the lines you import things add
```haxe
#if android
import flixel.input.actions.FlxActionInput;
import android.FlxVirtualPad;
#end
```

after those lines
```haxe
	inline function get_controls():Controls
		return PlayerSettings.player1.controls;
```

add
```haxe
	#if android
	var _virtualpad:FlxVirtualPad;
	var trackedinputs:Array<FlxActionInput> = [];
	#end
	
	#if android
	public function addVirtualPad(?DPad:FlxDPadMode, ?Action:FlxActionMode) {
		_virtualpad = new FlxVirtualPad(DPad, Action);
		_virtualpad.alpha = 0.75;
		add(_virtualpad);
		controls.setVirtualPad(_virtualpad, DPad, Action);
		trackedinputs = controls.trackedinputs;
		controls.trackedinputs = [];
	}
	#end

	#if android
        public function addPadCamera() {
		var camcontrol = new flixel.FlxCamera();
		FlxG.cameras.add(camcontrol);
		camcontrol.bgColor.alpha = 0;
		_virtualpad.cameras = [camcontrol];
	}
	#end
	
	override function destroy() {
		#if android
		controls.removeFlxInput(trackedinputsUI);
		controls.removeFlxInput(trackedinputsNOTES);	
		#end	
		
		super.destroy();
	}
```

And Somehow you finised to add the android controls to your psych engine copy

now on every state/substate add
```haxe
        #if android
	addVirtualPad(FULL, A_B);
        #end

	//if you want it to have a camera
        #if android
	addPadCamera();
        #end

	//in states, those needs to be added before super.create();
	//in substates, in fuction new at the last line add those

	//on Playstate.hx after all
	//obj.camera = ...
	//add
        #if android
	addAndroidControls();
        #end

	//to make the controls visible the code is
	#if android
	androidc.visible = true;
	#end

	//to make the controls invisible the code is
	#if android
	androidc.visible = false;
	#end
```

7. Prevent the Android BACK Button

in TitleState.hx

after
```haxe
	override public function create():Void
	{
```

add
```haxe
		#if android
		FlxG.android.preventDefaultKeys = [BACK];
		#end
```

8. Set An action to the BACK Button

you can set one with
```haxe
		#if android FlxG.android.justReleased.BACK #end
```

9. sys.FileSystem and sys.io.File

this is not working in your game storage but on phone storage will work with this

```haxe
SUtil.getPath() + 
```
this will make the game to use the phone storage
but you will have to add one thing in Your source

in Main.hx before 
```haxe
		addChild(new FlxGame(gameWidth, gameHeight, initialState, zoom, framerate, framerate, skipSplash, startFullscreen));
```

add 
```haxe
		SUtil.doTheCheck();
```
this will chack for android storage permisions and for the assets/mods directory

An example

in TitleState.hx instead of
```haxe
	        #if sys
		if (!sys.FileSystem.exists("assets/replays"))
			sys.FileSystem.createDirectory("assets/replays");
		#end
```

Replace with
```haxe
		#if sys
		if (!sys.FileSystem.exists(SUtil.getPath() + "assets/replays"))
			sys.FileSystem.createDirectory(SUtil.getPath() + "assets/replays");
		#end
```

In Replay.hx you will have to do the same thing with SUtil

10. On Crash Application Alert

on Main.hx after
```haxe
 	public function new()
	{
		super();
```	
add
```haxe
 	        SUtil.gameCrashCheck();
```	

11. File Saver

This is a future to save files with sys.io.File
This is the code
```haxe
SUtil.saveContent("your file name", ".txt", "lololol");

//The file location where is saved, will be where the assets and mods are located in phone storage in system-saves folder
```

13. Do an action when you press on the screen
```haxe
		#if android
                var justTouched:Bool = false;

		for (touch in FlxG.touches.list)
		{
			if (touch.justPressed)
			{
				justTouched = true;
			}
		}
		#end

                if (controls.ACCEPT #if android || justTouched #end)
                {
                        //Will do something
                }
```

## Credits:
* Saw (M.A. JIGSAW) me - doing the rest code, utils, pad buttons and anoder things
* luckydog7 - original code for android controls
* HayatoKawajiri - Hitbox and Pad designer
