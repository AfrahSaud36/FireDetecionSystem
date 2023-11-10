# Fire Detection System

A Drone Solar Powered Fire Detection System is a groundbreaking solution that combines cutting-edge technology, sustainable energy, and Python programming to revolutionize early fire detection and intervention. This system employs a drone as its primary vehicle, seamlessly integrating a high-resolution webcam, solar panels, and advanced Python algorithms to provide intelligent and eco-friendly fire prevention and response.

## Features

- **High-Resolution Webcam**: Captures real-time visuals for enhanced fire detection.
- **Solar Panels**: Enables prolonged aerial surveillance with eco-friendly, sustainable energy.
- **Python Programming**: Powers real-time data analysis, adaptive responses, and intelligent learning.
- **Adaptive Responses**: Swift and precise firefighting maneuvers guided by Python algorithms.
- **Sustainability**: From solar-powered operations to eco-friendly firefighting measures, reducing environmental impact.
- **Continuous Refinement**: Python's open-ended nature allows for ongoing refinement to meet emerging fire detection needs.

## 3D Drone Module

<img width="646" alt="3D_Drone" src="https://github.com/AfrahSaud36/FireDetecionSystem/assets/138797663/d3a70cef-c3ca-4c4b-976a-a3dde3bc8ef2">

```python
import cv2
import numpy as np
import pywhatkit as kit

def filter_fire(frame):

    hsv = cv2.cvtColor(frame, cv2.COLOR_BGR2HSV)

    lower_fire = np.array([0, 120, 70])
    upper_fire = np.array([10, 255, 255])

    mask = cv2.inRange(hsv, lower_fire, upper_fire)
    mask = cv2.erode(mask, None, iterations=2)
    mask = cv2.dilate(mask, None, iterations=2)
    contours, _ = cv2.findContours(mask, cv2.RETR_EXTERNAL, cv2.CHAIN_APPROX_SIMPLE)

 

    fire_detected = False
    for contour in contours:

        area = cv2.contourArea(contour)

        if area > 5000: 

            print("Emergency!! fire has been detected 🔥")
            print("Emergency!! fire has been detected 🔥")
            print("Emergency!! fire has been detected 🔥")
            country_code = "966"
            phone_num = f"+{country_code}use your emergency center number here"
            message = "Hello, this is a fire alarm from drone1!!"
            hour=23 
            minute=57
            kit.sendwhatmsg(phone_num, message, hour, minute)
            fire_detected = True

            break
    return fire_detected

def filter_smoke(frame):

    gray = cv2.cvtColor(frame, cv2.COLOR_BGR2GRAY)
    gray = cv2.GaussianBlur(gray, (15, 15), 0)

    _, thresh = cv2.threshold(gray, 200, 255, cv2.THRESH_BINARY)

    thresh = cv2.erode(thresh, None, iterations=2)
    thresh = cv2.dilate(thresh, None, iterations=2)


    contours, _ = cv2.findContours(thresh, cv2.RETR_EXTERNAL, cv2.CHAIN_APPROX_SIMPLE)

    # Check if smoke is detected 
    smoke_detected = False

    for contour in contours:

        area = cv2.contourArea(contour)

        if area > 50000:  # Adjust this threshold based on your needs
            print("Emergency!! Smoke has been detected💨")
            smoke_detected = True
            break

    return smoke_detected

#video from the camera
cap = cv2.VideoCapture(0)
while True:

    ret, frame = cap.read()
    fire_detected = filter_fire(frame)
    smoke_detected = filter_smoke(frame)
    if fire_detected:

        cv2.putText(frame, "Fire Detected!", (10, 30), cv2.FONT_HERSHEY_SIMPLEX, 1, (0, 0, 255), 2, cv2.LINE_AA)

    if smoke_detected:

        cv2.putText(frame, "Smoke Detected!", (10, 60), cv2.FONT_HERSHEY_SIMPLEX, 1, (255, 165, 0), 2, cv2.LINE_AA)
    cv2.imshow('Fire and Smoke Detection', frame)

    if cv2.waitKey(1) & 0xFF == ord('x'):

        break

cap.release()
cv2.destroyAllWindows()

