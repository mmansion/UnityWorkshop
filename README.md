##  Workshop 1 : Intro to Unity


### Agenda

* [10 MIN] Introduction

* [10 MIN] On Story 

* [10 MIN] On Materials & Tools

* [~30 MIN] Overview & Project Setup

  + Gather Assets
  + Create New 2D Project
  + Configure / Tour Workspace
  + Create Assets Folders
  + Create a Scene -> “Scene1”
  + Add a Sprite to the stage - discuss Game Objects
  + Run the game/app

* [5] Break

* [~35 MIN] Scene Nav

  + Configure the scene’s Camera
  + Attach Pixel-Perfect Script
  + Create a Canvas, and Scene Title
  + (Illustrator) Review UI Design
  + Add UI Assets to Project, Organize
  + Create Scene Background (Sprite)
  + Add the Buttons
  + Add fonts, set text for buttons
  + Create a media placeholder
  + Create a Script
  + Add Button Interactivity, Log Results
  + Paste the rest of “gotoScene”
  + Change scene name to SlideShow
  + Change title text to “Slide Show”
  + Duplicate Scene (change name, title, background)
  + Navigate between scenes

[5] Break

[~35 MIN] Part 3, Media & Interactivity

  Slideshow Scene
  + Slideshow Images (Resources Folder) 
  + add slideshow buttons
  + add slideshow script
  
  VideoPlayer Scene
  + assets/create/render texture
  + attach component (video player)
  + big buck bunny

  Animation Scene
  + (Photoshop) Frame-by-Frame, export
  + Create assets folder, add images
  + Drag multiple to stage, create animation
  + Animation window

  + Build the Project

### Resources

#### [Download Assets](https://drive.google.com/file/d/0B1Zs29ohFpIgY0Z2aEoyTE1vTkk/view?usp=sharing)

#### PixelPerfectCamera.cs

```
using UnityEngine;
using System.Collections;

public class PixelPerfectCamera : MonoBehaviour {

	public static float pixelsToUnits = 1f;
	public static float scale = 1f;

	public Vector2 nativeResolution = new Vector2 (1920, 1080);

	void Awake () {
		
		var camera = GetComponent<Camera> ();

		if (camera.orthographic) {
			scale = Screen.height/nativeResolution.y;
			pixelsToUnits *= scale;
			camera.orthographicSize = (Screen.height / 2.0f) / pixelsToUnits;
		}
	}

}
```

#### UEventReceiver.cs
```
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.EventSystems;
using UnityEngine.SceneManagement;

public class UiEventReceiver : MonoBehaviour {

	public GameObject slideShow;
	public int totalSlides = 8;
	public int currSlideNo = 0;

	public void gotoScene() {
		
		var go = EventSystem.current.currentSelectedGameObject;

		if (go != null) {
			
			Debug.Log ("Clicked on : " + go.name);

			switch (go.name) {

			case "btnSlideshow": 

				SceneManager.LoadScene ("SlideShow", LoadSceneMode.Single);
					break;

			case "btnVideoPlayer": 

					SceneManager.LoadScene ("VideoPlayer", LoadSceneMode.Single);
					break;

				case "btnAnimation": 
				
					SceneManager.LoadScene ("Animation", LoadSceneMode.Single);
					break;

				default:
						break;
			}

		} else {
			
			Debug.Log ("currentSelectedGameObject is null");
		}
	}

	public void nextSlide () {
		

		currSlideNo++;

		if (currSlideNo > totalSlides) {

			currSlideNo = 0;
		}

		changeSlide ();
	}

	public void prevSlide () {
		
		currSlideNo--;

		if (currSlideNo < 0) {

			currSlideNo = totalSlides;
		}

		changeSlide ();
	}

	private void changeSlide() {
		
		slideShow.GetComponent<SpriteRenderer> ().sprite = Resources.Load("img_" + currSlideNo, typeof(Sprite)) as Sprite;
	}
		
		
}

```
