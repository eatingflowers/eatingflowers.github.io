---
layout: post
title: Fly-Through with Unity and Oculus Rift
categories: Oculus, Unity, VR, C#
---

I'm working on a demo using Oculus Rift and Unity 3D.  I want to give the player the ability to move around in 3-dimensional space, as in a fly-through.  The default settings for OVRPlayerController (a prefab available from the Oculus Unity 4 Integration package available [here](https://developer.oculus.com/downloads/)), however, require the player to walk around on a plane, and gravity is in effect by default.  Moreover, the rotation of the OVRPlayerController is locked around the Y-axis (that is, the axis perpendicular to the plane OVRPlayerController is standing on), so one's movement is locked on the X-Z plane - not great for a fly-through.

I found a way to remove these restrictions and move around freely in three dimensions.  I couldn't find this anywhere online, so I hope this is helpful to someone.  (At the time of writing this I'm using Unity 5.1.1f1 with Oculus DK2 on OSX with version 0.5.0 of the Oculus Unity Integration package.)

[Here's a diff of the changes in case these aren't easy directions to follow.](https://github.com/syntactical/OVRPlayerController/commit/941d41669aecae201232f5014d82c6dfa32032ee?diff=split)

First, we have to make sure we're not affected by gravity.  Do this by setting the Gravity Modifier for OVRPlayerController to zero:

![](/assets/img/oculus-gravity.png)

OVRPlayerController won't move if it's not grounded on a plane, so we have to give it the ability to move if not grounded.  This can be done by commenting out the check for this condition:

	// No positional movement if we are in the air
	// if (!Controller.isGrounded)
	//	MoveScale = 0.0f;

There's something called centerEye which contains rotation data for the Oculus.  As far as I can tell, this object holds the rotation data for the Oculus.  But in the default OVRPlayerController settings, only the rotation about the Y-axis is used by OVRPlayerController. So let's change this line...

	if (HmdRotatesY)
	{
		Vector3 prevPos = root.position;
		Quaternion prevRot = root.rotation;

		transform.rotation = Quaternion.Euler(centerEye.rotation.eulerAngles.x, centerEye.rotation.eulerAngles.y, centerEye.rotation.eulerAngles.z);

There's also another place where the OVRPlayerController's transform's X- and Z-axis rotation is set to 0, so let's change that...

	Vector3 ortEuler = ort.eulerAngles;
	//ortEuler.z = ortEuler.x = 0f;
	ort = Quaternion.Euler(ortEuler);

And last, if you don't make the following change, the player will move in the direction gravity pulls it, but at a steady rate.  I don't quite understand why this needs to be changed, but it worked for me.

	float motorDamp = (1.0f + (Damping * SimulationRate * Time.deltaTime));

	MoveThrottle.x /= motorDamp;
	MoveThrottle.y /= motorDamp;
	MoveThrottle.z /= motorDamp;

Now you should be able to fly through your environment.
