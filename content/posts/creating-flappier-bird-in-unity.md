---
title: "Creating 'Flappier Bird' in Unity"
date: 2023-07-11
description: ""
draft: false
author: 'Mohamed Barghush'
---

# **Creating 'Flappier Bird' in Unity**  
![1_l8irNv-cl63myFBRL2TRCw](https://user-images.githubusercontent.com/26281629/220197299-0984c09a-fd44-4b5f-90a1-235d82986df3.jpeg)  
## **Introduction**
**'Flappy Bird'** was and probably still is one of the most popular games in the world, creating a replica of it is one of the most popular things beginner game developers do as a challenge on their own. I didn't do it when I started game development, so why not do it now?

## **Creating the bird and the background**
Let's start by drawing a little bird, and it turned out like this:    
![1_G147y2sJe5KoeytRqaYp6g](https://user-images.githubusercontent.com/26281629/220197421-e36a54a8-b0a1-4929-b107-d7e6e9128d73.png)  
It surely looks dumb, but it's good enough to make the game, and adding up a simple background:    
![1_45QsCy3GuxSCU5PhERwf6Q](https://user-images.githubusercontent.com/26281629/220197468-66b6a050-0ebf-45df-b536-6fbcb8796d55.png)  
**what's missing ??? 🤔**
A ground:  
![1_BFY5Zx4oSvDFHeO_omdC6Q](https://user-images.githubusercontent.com/26281629/220197613-b83448b9-c40e-4a84-9180-4b263b67c0c8.png)  
Good enough! But, let's make it animated, because I have an idea for later:  
![1_vM5WSFKvZpOJQ02MwnATzw](https://user-images.githubusercontent.com/26281629/220197669-3143290b-5360-430a-8375-fc125c44d386.gif)  
**Cool !!!**

## **Now let's head to Unity**
I put everything in the scene, add a collider for the ground, make a script for the bird, **why isn't the script working? why isn't it working??? oh, that's why!!**

## **Implementing the bird's movement**
Let's add a Rigidbody, edit the script for the bird to advance forward (to the right), reset the velocity and jump with some force when the left mouse button is clicked:

```
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class BirdScript: MonoBehaviour
{
  Rigidbody2D rb;
  public float jumpForce = 5f;
  
  // Start is called before the first frame update
  void Start()
  {
    rb = GetComponent<Rigidbody2D>();
  }
  
  // Update is called once per frame
  void Update()
  }
    if(Input.GetButtonDown("Fire1")) {
      rb.velocity = Vector2.zero;
      rb.AddForce(Vector2.up* jumpForce, ForceMode2D. Impulse);
    }
    Vector2 velocity = new Vector2(3, rb.velocity.y); rb.velocity = velocity;
    rb.velocity = velocity;
  }
}
```

## **Adding the camera script**
The bird flies, but, into infinity, let me try to attach the camera to it and see it fly, **OMG!! bad idea! bad idea!** back outside, ok, let's make the camera move with the player, smoothly that is, with a script, nothing else can save me now:

```
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Camera: MonoBehaviour
{
  public Transform target;

  // Start is called before the first frame update
  void Start()
  {
  
  }
  
  // Update is called once per frame
  void Update()
  {
    Vector3 cameraPos = target.transform.position;
    transform.position = cameraPos;
  }
}
```

### **Moving the background and the ground**
Let's play the game, oh no, what is this?, everything disappears once I press **Play**, after checking the difference in the position of the camera: oh, that's why, it move on the Z-axis from -10 to 0 once I press Play, probably because of the script, let's add some offset to camera:

```
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Camera: MonoBehaviour
{
  public Transform target;
  public Vector3 offset;

  // Start is called before the first frame update
  void Start()
  {
    transform.position = target.transform.position + offset;
  }

  // Update is called once per frame
  void Update()
  {
    Vector3 cameraPos = target.transform.position + offset;
    cameraPos.y = offset.y;
    transform.position = cameraPos;
  }
}
```

### **Adding pipes**
Let's press Play! Nice! It works now! Good enough!
But man, the background and the ground, they aren't moving with the bird, they're not advancing with the bird, what can I do? Ok, I'll add the animated ground as a child to the camera, so it moves with the camera and animate because of the previously made animation, looks good enough! !now for the background! I can't just fix it to move with the camera, and I can't just instantiate it infinitely with the camera, instantiating and removing backgrounds doesn't seem like a very good option for Android games, it'll take so much resources, the game will probably run slower with this, and I need the game as optimized as possible for weaker devices, I know! I'll create 2 background game objects, and move them in front of the bird when it gets out of the camera's view, that way, I'll have only 2 game objects that are simple moving ahead of the bird to make it like they're infinitely spawning, so I'll create a parent for the 2 backgrounds and put the following script on it (which simply assigns the values for another script I'll put on each and every background tile):

```
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Spawner : MonoBehaviour
{
	public Tile[] tiles;
	public Vector3 position;
	public Vector3 offset;
	public Transform bird;
	public float distanceCheck;

	// Start is called before the first frame update
	void Start()
	{
		tiles[0].manager = this.GetComponent<Spawner>();
		tiles[0].bird = bird;
		tiles[0].distancecheck = distancecheck;
		tiles[0].transform.position = position;
		tiles[0].enabled = true;
		for (int i = 1; i < tiles.Length; i++)
		{
			tiles[i].manager = this.GetComponent<Spawner>();
			tiles[i].bird = bird;
			position += offset;
			tiles[i].distanceCheck = distanceCheck;
			tiles[i].transform.position = position;
			tiles[i].enabled = true;
		}
	}

	// Update is called once per frame
	void Update()
	{

	}
}
```

### **Implementing pipes as tiles**
Now I create a **Tile** script (which simply checks if the position of the current game object is less than the position of the bird by a certain value on the X-axis and moves ahead of the bird when greater than that distance):

```
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Tile: MonoBehaviour
{
  [HideInInspector] public Spawner manager; 
  [HideInInspector] public Transform bird; 
  [HideInInspector] public float distanceCheck; 
  [HideInInspector] public bool pipe = false;

  // Start is called before the first frame update
  void Start()
  {

  }
  // Update is called once per frame
  void Update() {
    if (bird.transform.position.x - this.transform.position.x >= distanceCheck) { 
      manager.position + manager.offset;
      this.transform.position = manager.position;
    }
  }
}
```
And put it on each single background tile, and add the 2 backgrounds in the script of the parent along with the values needed by trying for the best values. which for me are like this:

![1_Bc9kCwpuvU6FKlifklSDDg](https://user-images.githubusercontent.com/26281629/220200407-70518c8f-b641-4028-a7a8-e709e9503855.png)

And since the **Spawner** script which is the one on the parent assigns the values for the background tiles for me, I don't really need to assign anything on the **Tile** script in the **Unity Editor** myself.

**Now I have a game! Well, kinda.**

### **Adding collision detection for the bird and pipes**
There's nothing for the bird to do, it doesn't really die either, so no lose conditions, it'll just keep flying like this forever.
Let's go draw the pipes I saw in **Flappy Bird! Ok!** It turned out good enough:

![1_Kri6JbXeD9EM9lPaNgh2Uw](https://user-images.githubusercontent.com/26281629/220200522-69f2740c-0243-4ac8-9bc0-81d02a0b3a62.png)

A tall simple pipe drawing, now we import it into **Unity**, add them to the scene, and **BAM!!** We have pipes, oh it's a single pipe, that does nothing, it doesn't even affect the player, let's make it do so, we add 2 **2D Colliders**, one for each side of the script and adjust their values to match the borders of the pipes.

### **Implementing the bird's death and lose conditions**
Ok, now if we press **Play**, the bird does interact with it, but it doesn't die, how does the bird even die? Let's make it so the player can't control the bird anymore once it's dead by modifying the **BirdScript** script like this:

```
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class BirdScript : MonoBehaviour
{
  Rigidbody2D rb;
  public float jumpForce = 5f;
  public bool alive = true;

  // Start is called before the first frame update
  void Start()
  {
    rb = GetComponent<Rigidbody2D>();
  }

  // Update is called once per frame
  void Update()
  {
    if (!alive)
      return;

    if(Input.GetButtonDown ("Fire1")) {
      rb.velocity = Vector2.zero;
      rb.AddForce(Vector2.up * jumpForce, ForceMode2D.Impulse);
    }
    Vector2 velocity = new Vector2(3, rb.velocity.y);
    rb.velocity = velocity;
  }
}
```

I simply added a bool variable called **'alive'** which is a check for if the player is dead or not and if he's dead (so alive = false) it simply returns (which means exits the Update function without executing the rest of it).
Still, the bird doesn't die, it needs to trigger some kind of event to die, that event would be, **drum rolls !!**

**OnCollisionEnter2D** (for 2D colliders or collisions).

Now let's add a function to make the player die once it touches anything, so the **BirdScript** script should look like this:

```
using System.Collections;
using UnityEngine;
using System.Collections.Generic;
public class BirdScript : MonoBehaviour
{
	Rigidbody2D rb;
	public float jumpForce
	public bool alive = true;

	// Start is called before the first frame update
	void Start()
	{
		rb = GetComponent<Rigidbody2D>();
	}

	// Update is called once per frame
	void Update()
	{
		if (!alive) {
			return;
		}

		if(Input.GetButtonDown ("Fire1")) {
			rb.velocity = Vector2.zero;
			rb.AddForce (Vector2.up jumpForce, ForceMode2D. Impulse);
		}
		Vector2 velocity = new Vector2(3, rb.velocity.y);
		rb.velocity = velocity;
	}

	void OnCollisionEnter2D () {
		alive = false;
	}
}
```

## **Conclusion**
Now the player dies once it touches any of the pipes, now let's do the same thing we did on the background tiles to the **pipes**, so making 3 pipe game objects, putting them all under one parent that has the script **'Spawner'**, and each pipe having the script **'Tile'**, and modify the **Tile** and **Spawner** scripts a little bit by adding the **'pipes'** variable in the Spawner script and the **'pipe'** variable in the **Tile** script:

```
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Spawner: MonoBehaviour
{
	public Tile[] tiles;
	public Vector3 position;
	public Vector3 offset;
	public Transform bird;
	public float distanceCheck;
	public bool pipes = false;

	// Start is called before the first frame update
	void Start()
	{
		tiles[0].manager = this.GetComponent<Spawner>();
		tiles[0].bird = bird;
		tiles[0].distanceCheck = distanceCheck;
		if(pipes) {
			tiles[0].pipe = pipes;
			position.y = Random.Range(0, 4);
		}
		tiles[0].transform.position = position;
		tiles[0].enabled = true;

		for (int i = 1; i < tiles.Length; i++)
		{
			tiles[i].manager this.GetComponent<Spawner>();
			tiles[i].bird = bird;
			position += offset;
			tiles[i].distanceCheck distanceCheck;
			if (pipes) {
				tiles[i].pipe = pipes;
				position.y = Random.Range(0, 4);
			}
			tiles[i].transform.position = position;
			tiles[i].enabled = true;
		}
	}
}
```

```
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Tile: MonoBehaviour
{
	[HideInInspector] public Spawner manager;
	[HideInInspector] public Transform bird;
	[HideInInspector] public float distanceCheck;
	[HideInInspector] public bool pipe = false;

	// Start is called before the first frame update
	void Start()
	{
		
	}
  
  // Update is called once per frame
  void Update () {
    if (bird.transform.position.x - this.transform.position.x >= distanceCheeck) {
      manager.position += manager.offset;
      if (pipe) {
        manager.position.y = Random.Range(0, 4);
      }
      this.transform.position = manager.position;
    }
  }
}
```

And finally insert some good values for the **Spawner** script on the parent of the pipes:

![1_Nn7N46luSiZGvkoyydTmdA](https://user-images.githubusercontent.com/26281629/220205637-b1c1970f-ab76-464d-a2e4-08568b4c5242.png)

Ok! Now the game is **complete**!
**YAY!**
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
## **(Advanced)**
I don't feel satisfied with the bird just simply going up and down without any rotation, it looks so unlike the real Flappy Bird, Let's modify the **BirdScript** script to make it look where it's headed like this:

```
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
public class BirdScript: MonoBehaviour
{
  Rigidbody2D rb;
  public float jumpForce = 5f;
  public bool alive = true;
  
  // Start is called before the first frame update
  void Start () {
    rb = GetComponent<Rigidbody2D>();
  }
  
  // Update is called once per frame
  void Update() {
    if (!alive) {
      return;
    }
    
    if(Input.GetButtonDown("Fire1")) {
      rb.velocity = Vector2.zero;
      rb.AddForce(Vector2.up jumpForce, ForceMode2D. Impulse);
    }
    Vector2 velocity = new Vector2(3, rb.velocity.y);
    rb.velocity = velocity;
    Quaternion rotation = Quaternion.LookRotation(Vector3. forward, rb.velocity);
    Quaternion relativeRotation = Quaternion.RotateTowards(transform.rotation, rotation, 500000 * Time.deltaTime); 
    Vector3 eulerRot = relativeRotation.eulerAngles;
    eulerRot.z = Mathf.Clamp(eulerRot.z, 210, 315);
    transform.eulerAngles = eulerRot;
  }
  
  void OnCollisionEnter2D () {
    alive = false;
  }
}
```

I added the lines from **31–35**, which basically computes the rotation based on the **Rigidbody velocity** based on the **Vector3.forward** direction to produce a Vector3 with the right direction vector to look towards, then we calculate the Quaternion rotation of the object with the **Quaternion.RotateTowards** function, and we take the **euler angles** of that (Which is the rotation on the 3 axis rather than the 4 axis Quaternion), we limit the result between **210** and **315** degrees on the **Z-axis** (which are the right values for me personally from testing in the editor), and finally set the **euler angles** of the **bird game object** to the euler values we got on line **35**.
Now it's a game! you can download and play my version in  
[HERE](https://drive.google.com/file/d/11fmoKa8_t5ibV0cY7slBmw5eQ8Q7o0OL/view)

**Thanks for reading through!**
