# Deep SORT Architecture

## Architecture Overview

The Deep SORT algorithm is structured into interconnected modules, that each play a role in tracking objects in video sequences. These are the key components:

### 1. **Input Frame**
- The video feed is divided into individual frames.
- Each frame serves as the input to the pipeline.

---

### 2. **Object Detection**
- It detects objects in the current video frame.
- Outputs:
  - **Bounding box coordinates**: Defines the location and size of detected objects.
  - **Class labels**: Identifies the type of objects (such as ball, person, etc.).
- This assignment used Faster R-CNN as the object detection model.

---

### 3. **Feature Extraction**
- A deep Convolutional Neural Network (CNN) extracts an appearance descriptor (embedding) for each detected object.
- The embedding is a high-dimensionality vector uniquely identifying the object's appearance.
- Helps to distinguish objects that are spatially close or overlapping.

---

### 4. **Kalman Filter**
- Tracks the motion and predicts the next position of each tracked object.
- Operates in two phases:
  1. **Prediction**: Estimates the next state of the object where the state is position and velocity.
  2. **Update**: Corrects the predicted state using new measurements from object detection.

#### **Core Equations of the Kalman Filter**

![Deep SORT Architecture](https://www.kalmanfilter.net/img/summary/KalmanFilterDiagram.png)

| Term | Name                        | Alternative Term      | Dimensions  |
|------|-----------------------------|-----------------------|-------------|
| x    | State Vector               |                       | nx×1        |
| z    | Measurements Vector        | y                     | nz×1        |
| F    | State Transition Matrix    | Φ, A                  | nx×nx       |
| u    | Input Variable             |                       | nu×1        |
| G    | Control Matrix             | B                     | nx×nu       |
| P    | Estimate Covariance        | Σ                     | nx×nx       |
| Q    | Process Noise Covariance   |                       | nx×nx       |
| R    | Measurement Covariance     |                       | nz×nz       |
| w    | Process Noise Vector       | y                     | nx×1        |
| v    | Measurement Noise Vector   |                       | nz×1        |
| H    | Observation Matrix         | C                     | nz×nx       |
| K    | Kalman Gain                |                       | nx×nz       |
| n    | Discrete-Time Index        | k                     |             |

---

### 5. **Hungarian Algorithm**
- It solves the assignment problem to find the optimal pairing of detections to tracks by minimizing the overall cost found by the cost matrix.
- Matches detected objects with existing tracks.
- Operates on a cost matrix, where each entry represents the cost of associating a detection with a track.
- Objective is to minimize the total assignment cost.
- Results:
  - Updates tracks with matched detections.
  - Marks tracks as "lost" if no suitable match is found.

---

### 6. **Track Management**
- Manages the lifecycle of object tracks.
  - **Creation**: Initializes new track for unmatched detections.
  - **Update**: Updates existing tracks with matched detections.
  - **Deletion**: Removes tracks that haven't been matched for a predefined number of frames.
- Ensures efficient handling of object entries, exits, and occlusions in the scene.

---

## Diagram
![image](https://github.com/user-attachments/assets/408d708e-e69b-4d8a-b1f1-bb2da0ff5d7a)

Or use Excalidraw link if image does not show up: https://excalidraw.com/#json=9JTui1JdaadUqa09ohHHy,k6GOKJ5lyhAaWlM3EyyQnA 
