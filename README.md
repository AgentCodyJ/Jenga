# Overview
Jenga game for Crossover "Unity Game Developer" Assessment

## Author
Cody Jolin
Creation Date: 2023-11-03
LinkedIn Profile: https://www.linkedin.com/in/cody-jolin-87882457/
Game Design Portfolio: https://codyjolin.wordpress.com/

## Controls
*Left-click hold and move mouse = Orbit camera
*Right-click on block = Check block info
*Space = Remove glass blocks and toggle gravity
*Space (2nd time) = Restart scene
*E = Move camera focus to next Jenga stack
*Q = Move camera focus to previous Jenga stack

## Simple Dev Log
-Updated Unity version + created new Unity 3D Project.
-Renamed scene to something more meaningful -> “CodyJolinMain.unity”.
-Created 3D Object -> Plane named “Floor”.
-Imported wood, rock, sand, and stone textures + skybox from Unity Asset Store.
-Used Rendering -> Lighting -> Environment menu to set desert landscape skybox.
-In same menu, created subtle red fog effect (went with linear, as exponential and exponential squared were more than I wanted).
-Applied sand material/texture to Floor and changed its tiling to 100 so that it wouldn’t be obvious that the texture was repeating. 
	Moved smoothness down for low reflection and added some red to the albedo value for a red tinge to match skybox. 

-Created new 3D Object -> Cube and changed its scale to (3,1,1) to make a rectangle.
-Used materials from wooden + stone textures taken from the Asset store to create a wooden block (rectangle + wood material). 
-Adjusted the Box Collider to have values of (1.05, 1.05, 1.05), allowing for a visible gap between the blocks. 
	Otherwise, the blocks visibly blend together, especially if they are of the same type.
-Added Rigidbody to the block and unchecked "Use Gravity" so that the default start of the scene does not apply gravity to the Jenga stacks.
-Created tag "Block" and applied it to the block. This will be used later to target the blocks and turn gravity back on.
-Created a prefab out of this wooden block instance.
-Copied the prefab two times and modified each one to create a stone block and glass block prefab. 
	The glass block uses a standard material with Transparent Rendering Mode + Metallic around .75 and Smoothness around .45.

-Created Readme.md to log Overview, Author information, Controls, and Simple Dev Log (this file).

-Created C# script named “GameController.cs”.
-Opened project in Visual Studio and began to code "GameController.cs".
-This script handles:
	*API call to fetch student data
	*Sorting and grouping of student data
	*Constructing Jenga stacks at runtime based on API call data, including duplicate data check and error handling
		Note: Code has been written with the flexibility of handling Grades 1-9 (i.e. more than just 3 grade stacks)
	*Creating Jenga stack label based upon grade (i.e. "6th Grade", "7th Grade", etc)
	*Logging Jenga stack debug info to Unity console
	*Creating camera focal point for each Jenga stack at half the stack's height
-Attached "GameController.cs" to Main Camera and set fields in inspector.
-Upon needing JSON conversion methods, Unity’s Package Manager “Add package from git URL” option was used to import “com.unity.nuget.newtonsoft-json”. 
	This then allowed the package to be used in scripts by including “using Newtonsoft.Json”.

-Created C# script "StudentStat.cs".
-This script serves as an API object class for storing student stat data.
-Implemented Equals method for custom equality comparison between StudentStat instances (used to duplicate stat detection).
-Attached "StudentStat.cs" script to each of the wooden, stone, and glass block prefabs.

-Created C# script named "CameraKeyboardController.cs".
-This script handles:
	*Setting initial camera position/rotation with respect to the first Jenga stack
	*Moving camera with left-click + move mouse
	*Displaying block info on screen when block is right-clicked
	*Pressing space bar to enable gravity (1st press) or reload the scene (2nd press)
	*Pressing "Q" to switch camera to previous Jenga stack or "E" to switch to next stack
-Attached "CameraKeyboardController.cs" to Main Camera and set fields in inspector.

-Created new 3D Object -> "Text - TextMeshPro" Object named "StackLbl" to serve as the Grade Label displayed in front of each Jenga stack. 
-Adjusted the size, color, alignment, and starting position for the label and then created a prefab out of it.
	This prefab is used by the “GameController.cs” script to instantiate a label for each newly created stack.

-Created new UI -> Canvas object named "UI Canvas" to hold the UI text for player controls and block student stat information.
	Note: Unity automatically adds the EventSystem upon creating a UI object.
-Set Render Mode of "UI Canvas" to "Screen Space - Overlay" so that UI text children would be displayed at all points of the game (when given text values).
-Created new UI -> "Text - TextMeshPro" Object named "BlockStats" as a child of "UI Canvas" with a blank text value (set later by right-click a block).
-Created new UI -> "Text - TextMeshPro" Object named "Controls" as a child of "UI Canvas" with text value set to the game controls.
	Controls are shown on screen at all times to the player.
-For both UI text objects, set text position, color, size, alignment, anchor, and visible range (how much of the screen the UI text is allowed to take up).

-During testing, the Jenga stacks spawned in with a bit of physics wobble, which led to them slowly toppling over.
	A quick fix for this was to enable "Adaptive Force" in the project Physics settings, which helps with stacked Rigidbodies.
