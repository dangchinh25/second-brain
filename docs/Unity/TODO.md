- Sprite Editor
	- What is Pixel per Unit and wh arts need to beat is Pixel per Unit and why everything in pixel arts need to be 16
- Understand tiles pallete
- Animation Editor
- Tag
- Canvas
- Event System Object
- Prafab
- SceneManager
- Packages
	- Cinemachine => Kinda important
	- Input System => Most important?
	- URP
	- ProBuilder
	- 2D Sprite
	- TextMeshPro
- PresetManager
- Capsule Collider
	- ContactFilter
	- RaycastHit

NOTES:
- Use [SerializedField] to expose a variable in a script to be access by Unity UI
	- The exposed variable can be anything, including number, string, or even another Unity Object that we can get access to in code 
- Every logic that the user creates will stays in various users script, and if it interacts with other object we can just use [SerializedField] to get access to it
	- Or if the logic involves some components (i.e: Animator, RigidBody, etc) inside the user object, we can use **GetComponent()** to get access to that component 
- **Sprite Renderer** component is for rendering images/attaching images to an object instead of using the default white box
- **Rigid Body** is for adding physics to the component
- To bound the object and create custom interaction between different object => Use collider (for example, without collider the player may move through wall)