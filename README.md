# unity-pan-and-zoom

A light Unity MonoBehaviour for handling tap, swipe and pinch motions on mobile.

Features:
- Can be used to control a 2D orthographic camera out of the box.
- Supports touch emulation with a mouse if a mouse is present.
- Simple API for listening to input events.
- Detects UI and ignores touch input when over raycastable UI elements.

## Install

You can:
- Download the [PanAndZoom.cs](https://raw.githubusercontent.com/GibsS/unity-pan-and-zoom/master/PanAndZoom.cs) file and place it in your Unity project.
- Add this repository as a submodule if you are using git.

## How to

### Use it out of the box

Simply place the script on one of your GameObjects. Look at the inspector to customize it's behaviour.

You can either set the PanAndZoom camera to the one you want or leave it to "None" and let the script fetch the main Camera. 

### Listen to events

There are 5 events you can listen to (Every position is in screen coordinates):

- ```onStartTouch(Vector2 position)```: Called when the player starts to touch the screen.
- ```onEndTouch(Vector2 position)```: Called when the player finishes touching the screen.
- ```onTap(Vector2 position)```: Called when a tap occured (a short tap motion by the player).
- ```onSwipe(Vector2 delta)```: Called when the player swiped the screen (moved a single finger on the screen). 
The argument is the amount of movement on screen.
- ```onPinch(float olddistance, float newdistance)```: Called when the player pinches the screen with two fingers.
The first argument if the old distance between the two fingers. The second argument is the new distance.

You just need to get the reference to the component and add listeners to the different events:

```cs
PanAndZoom panAndZoom = go.GetComponent<PanAndZoom>();

panAndZoom.onTap += position => {
  Debug.Log("I've been tapped at " + position + "!");
};
```

### Overwrite camera control

If a player clicks a specific element in your game, you might want to disable the camera for the duration of the action (dragging an object, ..). To do that, you need to call:

```cs
panAndZoom.CancelCamera();
```

This will lead to the camera control to be disabled until the player stops touching the screen.

### Get the position in world space

```cs
panAndZoom.onTap += screenPosition => {
  Vector2 worldPosition = panAndZoom.camera.ScreenToWorldPoint(screenPosition);
  // or worldPosition = Camera.main.ScreenToWorldPoint(screenPosition) If you are certain it's the right one. 
  
  Debug.Log("The world was tapped at " + worldPosition + "!");
};
```

### Bound camera movement

You can constrain a camera to a given area by defining bounds either through the inspector (make sure to enable camera bounds) or in code:

```cs
// The player will not be able to see anything outside of these bounds
panAndZoom.boundMinX = 0;
panAndZoom.boundMaxX = 100;
panAndZoom.boundMinY = 0;
panAndZoom.boundMaxY = 100;
```
