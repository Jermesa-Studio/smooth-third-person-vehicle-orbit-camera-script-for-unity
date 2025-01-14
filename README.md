# Jrs Orbit Camera: Free & Open-Source Third-Person Vehicle Camera for Unity

Welcome to the ultimate guide for using the **JrsOrbitCamera**, a free and open-source third-person vehicle camera script for Unity. Whether you're a beginner or an experienced developer, this guide will walk you through everything you need to know about this powerful tool.

## Table of Contents

- [What is JrsOrbitCamera?](#what-is-jrsorbitcamera)
- [Why Use JrsOrbitCamera?](#why-use-jrsorbitcamera)
- [Features](#features)
- [Step-by-Step Guide](#step-by-step-guide)
- [Tips for Best Results](#tips)
- [Other Uses of JrsOrbitCamera](#other-uses)
- [Adjust the Middle Area](#adjust-middle-area)
- [Full Script Code](#code)

## What is JrsOrbitCamera?

The **JrsOrbitCamera** is a camera script for Unity, to orbit or rotate around an object. It was designed to provide a smooth and customizable third-person camera experience, particularly for vehicle-based games. It allows the camera to orbit around a target (like a car) while providing intuitive controls for rotation and zooming.

## Why Use JrsOrbitCamera?

If you're developing a third-person vehicle game, having a camera that smoothly follows and orbits around the vehicle is crucial. The JrsOrbitCamera offers:

- **Smooth Camera Movements:** The camera smoothly follows the target, providing a professional look and feel.
- **Customizable Controls:** You can adjust the rotation speed, zoom levels, and other parameters to fit your game's requirements.
- **Touch & Mouse Support:** The script supports both touch and mouse inputs, making it versatile for different platforms.
- **Free & Open-Source:** The script is free to use and modify, making it accessible for all developers.

## Features

Here are some of the key features of the JrsOrbitCamera script:

- **Orbit Control:** The camera can orbit around the target using mouse or touch input.
- **Zoom Control:** Users can zoom in and out using the mouse scroll wheel or pinch gestures on touch devices.
- **Rotation Smoothing:** The camera's rotation is smoothed to avoid jerky movements.
- **Middle Area Control:** The script allows you to define a middle area on the screen where camera controls are active.
- **Mobile-Friendly:** The script is optimized for touch inputs, making it ideal for mobile games.

## Step-by-Step Guide

Follow these steps to integrate the JrsOrbitCamera into your Unity project:

1. **Download the Script:** Download the script from [GitHub](https://github.com/Jermesa-Studio/smooth-third-person-vehicle-orbit-camera-script-for-unity).
2. **Add the Script to Your Project:** Drag and drop the script into your Unity project's Assets folder.
3. **Attach the Script to a Camera:** Select your camera in the Unity Editor, and add the JrsOrbitCamera script as a component.
4. **Set the Target:** Assign the target (e.g., your vehicle) to the `target` field in the script.
5. **Adjust Settings:** Customize the camera's rotation speed, zoom levels, and other parameters in the Inspector.
6. **Test the Camera:** Play your game and test the camera controls to ensure everything works as expected.

**Tip:** If you want to see the camera in action, you can download the complete project from [GitHub](https://github.com/Jermesa-Studio/JRS_Vehicle_Physics_Controller).

## Tips for Best Results

Here are some tips to get the most out of the JrsOrbitCamera script:

- **Adjust the Middle Area:** The middle area is where the camera controls are active. You can adjust its size in the Inspector to fit your game's UI.
- **Use Smoothing:** Enable rotation smoothing to avoid jerky camera movements, especially in fast-paced games.
- **Test on Different Devices:** If you're developing for mobile, test the camera controls on different devices to ensure a consistent experience.
- **Customize Zoom Levels:** Adjust the minimum and maximum zoom levels to fit your game's needs.

## Other Uses of JrsOrbitCamera

While the JrsOrbitCamera is designed for vehicle games, it can also be used in other scenarios:

- **Third-Person RPGs:** Use the camera to follow a character in a third-person RPG.
- **Exploration Games:** The camera can be used to explore environments, especially in open-world games.
- **Cinematic Scenes:** The smooth camera movements make it ideal for cinematic scenes in your game.

## Adjust the Middle Area

The **Middle Area** is a crucial feature of the JrsOrbitCamera script. It defines the area on the screen where camera controls (like rotation and zoom) are active. This is particularly useful for mobile games, where you want to avoid accidental touches outside the gameplay area.

### How to Adjust the Middle Area?

The middle area is controlled by the **middleAreaPercentage** variable in the JrsOrbitCamera script. Here’s how you can adjust and use it:

- **Access the Middle Area Setting:**
  1. In the Unity Editor, select the camera object that has the JrsOrbitCamera script attached.
  2. In the Inspector, you’ll see a field called Middle Area Percentage. This value determines the size of the middle area as a percentage of the screen.
- **Adjust the Middle Area Percentage:**
  1. The default value is 0.3, which means the middle area takes up 30% of the screen’s width and height.
  2. You can increase this value (e.g., 0.5) to make the middle area larger, or decrease it (e.g., 0.2) to make it smaller.
- **Test the Middle Area:**
  1. Enter Play mode and move your mouse or touch the screen within the middle area to see how the camera responds.
  2. Observe how the camera rotation behaves when the input is inside or outside the middle area.

**Note:** The middle area is only active for touch inputs. Mouse inputs will work regardless of the middle area setting.

## Full Script Code

Below is the complete code for the JrsOrbitCamera script. You can copy and paste it into your Unity project:

```csharp
// MIT License
//
// Copyright (c) 2023 Samborlang Pyrtuh
//
// Permission is hereby granted, free of charge, to any person obtaining a copy
// of this software and associated documentation files (the "Software"), to deal
// in the Software without restriction, including without limitation the rights
// to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
// copies of the Software, and to permit persons to whom the Software is
// furnished to do so, subject to the following conditions:
//
// The above copyright notice and this permission notice shall be included in all
// copies or substantial portions of the Software.
//
// THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
// IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
// FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
// AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
// LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
// OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
// SOFTWARE.

using System.Collections;
using UnityEngine;

public class JrsOrbitCamera : MonoBehaviour
{
    public Transform target;
    public float rotationSpeed = 5.0f;
    public float rotationSmoothTime = 0.1f;
    public float zoomSpeed = 10.0f;
    public float minZoom = 5.0f;
    public float maxZoom = 15.0f;
    public float zoomTime = 0.5f;
    public float distance;
    [SerializeField] private float middleAreaPercentage = 0.3f; // Adjust this value in the Inspector

    private float yaw = 0.0f;
    private float pitch = 45.0f; // Start from above the vehicle
    private float rotationVelocityX;
    private float rotationVelocityY;
    private Coroutine zoomCoroutine;

    void Start()
    {
        // If distance is not set in the inspector, use the initial distance to the target
        if (distance == 0)
        {
            distance = Vector3.Distance(transform.position, target.position);
        }
    }

    void Update()
    {
        // Define the middle area based on a percentage of the screen size
        //float middleAreaPercentage = 0.3f; // Adjust this value as needed
        float middleAreaWidth = Screen.width * middleAreaPercentage;
        float middleAreaHeight = Screen.height * middleAreaPercentage;
        float middleAreaX = (Screen.width - middleAreaWidth) / 2;
        float middleAreaY = (Screen.height - middleAreaHeight) / 2;
        Rect middleArea = new Rect(middleAreaX, middleAreaY, middleAreaWidth, middleAreaHeight);

        // Check if the mouse position is within the middle area
        if (middleArea.Contains(Input.mousePosition))
        {
            if (Input.GetMouseButton(0))
            {
                yaw += rotationSpeed * Input.GetAxis("Mouse X");
                pitch -= rotationSpeed * Input.GetAxis("Mouse Y");
                pitch = Mathf.Clamp(pitch, 5f, 90f); // Limit the pitch angle between -90 and 90 degrees
            }
        }

        if (Input.touchCount == 2)
        {
            Touch touchZero = Input.GetTouch(0);
            Touch touchOne = Input.GetTouch(1);

            // Check if both touches are within the middle area
            if (middleArea.Contains(touchZero.position) && middleArea.Contains(touchOne.position))
            {
                if (touchZero.phase == TouchPhase.Moved && touchOne.phase == TouchPhase.Moved)
                {
                    Vector2 touchZeroPrevPos = touchZero.position - touchZero.deltaPosition;
                    Vector2 touchOnePrevPos = touchOne.position - touchOne.deltaPosition;

                    float prevTouchDeltaMag = (touchZeroPrevPos - touchOnePrevPos).magnitude;
                    float touchDeltaMag = (touchZero.position - touchOne.position).magnitude;

                    float deltaMagnitudeDiff = touchDeltaMag - prevTouchDeltaMag;

                    float scrollInput = deltaMagnitudeDiff * zoomSpeed * Time.deltaTime;

                    if (scrollInput != 0.0f)
                    {
                        float targetZoom = Mathf.Clamp(distance + scrollInput, minZoom, maxZoom);
                        if (zoomCoroutine != null)
                        {
                            StopCoroutine(zoomCoroutine);
                        }
                        zoomCoroutine = StartCoroutine(ZoomToTarget(targetZoom));
                    }

                    // Only perform orbiting if both touches are within the middle area
                    if (middleArea.Contains(touchZero.position) && middleArea.Contains(touchOne.position))
                    {
                        yaw += rotationSpeed * (touchZero.deltaPosition.x + touchOne.deltaPosition.x) / 2;
                        pitch -= rotationSpeed * (touchZero.deltaPosition.y + touchOne.deltaPosition.y) / 2;
                        pitch = Mathf.Clamp(pitch, 5f, 90f); // Limit the pitch angle between -90 and 90 degrees
                    }
                }
            }
        }
        else
        {
            // Zoom in and out with mouse scroll wheel
            float scrollInput = Input.GetAxis("Mouse ScrollWheel");
            if (scrollInput != 0.0f)
            {
                float targetZoom = Mathf.Clamp(distance - scrollInput * zoomSpeed, minZoom, maxZoom);
                if (zoomCoroutine != null)
                {
                    StopCoroutine(zoomCoroutine);
                }
                zoomCoroutine = StartCoroutine(ZoomToTarget(targetZoom));
            }
        }

        // Smooth the rotation
        float smoothYaw = Mathf.SmoothDampAngle(transform.eulerAngles.y, yaw, ref rotationVelocityX, rotationSmoothTime);
        float smoothPitch = Mathf.SmoothDampAngle(transform.eulerAngles.x, pitch, ref rotationVelocityY, rotationSmoothTime);

        Quaternion rotation = Quaternion.Euler(smoothPitch, smoothYaw, 0.0f);
        transform.position = target.position - rotation * new Vector3(0, 0, distance);

        transform.LookAt(target);
    }

    IEnumerator ZoomToTarget(float targetZoom)
    {
        float startZoom = distance;
        float startTime = Time.time;
        while (Time.time < startTime + zoomTime)
        {
            distance = Mathf.Lerp(startZoom, targetZoom, (Time.time - startTime) / zoomTime);
            yield return null;
        }
        distance = targetZoom;
    }
}
