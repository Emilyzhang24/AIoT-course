# Lab 2 – Part 2: Detection, Segmentation, and Pose Estimation

**Estimated time:** 75 minutes

[Return to the Lab 2 overview](README_lab2.md)

---

## Part 2 Objectives

By the end of this session, you will be able to:

- Perform object detection on a static image.
- Run real-time object detection using a CSI camera.
- Monitor Jetson resource utilization during inference.
- Perform semantic segmentation.
- Perform human pose estimation.
- Compare the inputs, outputs, and applications of different computer-vision tasks.

---

## Computer-Vision Tasks

| Task | Main output |
|------|-------------|
| Image classification | One class label for the complete image |
| Object detection | Classes and bounding boxes |
| Semantic segmentation | A class label for each pixel |
| Pose estimation | Human body keypoints and skeletal connections |

---

# Step 1 – Verify the Installation

Navigate to the Jetson-Inference program directory:

```bash
cd ~/jetson-inference/build/aarch64/bin
```

Confirm that the required programs exist:

```bash
ls imagenet* detectnet* segnet* posenet*
```

Verify the camera:

```bash
v4l2-ctl --list-devices
```

Confirm that `/dev/video0` exists:

```bash
ls -l /dev/video0
```

> [!WARNING]
> If the camera is not detected, stop and notify the instructor.

---

# Task 1 – Object Detection

Object detection identifies both the type and location of one or more objects.

## 1.1 Static Image Detection

Run SSD-Mobilenet-v2 on a sample image:

```bash
./detectnet.py \
    --network=ssd-mobilenet-v2 \
    images/peds_0.jpg \
    images/test/detection_peds.jpg
```

If necessary, use the compiled executable:

```bash
./detectnet \
    --network=ssd-mobilenet-v2 \
    images/peds_0.jpg \
    images/test/detection_peds.jpg
```

Open:

```text
images/test/detection_peds.jpg
```

Record the results:

| Observation | Result |
|-------------|--------|
| Number of detected objects | |
| Object classes | |
| Highest confidence | |
| Lowest confidence | |

### Question 1

How is object detection different from image classification?

### Question 2

Why are bounding boxes useful in an AIoT application?

---

## 1.2 Real-Time Object Detection

Run detection using the CSI camera:

```bash
./detectnet.py \
    --network=ssd-mobilenet-v2 \
    csi://0
```

If `csi://0` does not work with the installed version, try:

```bash
./detectnet.py \
    --network=ssd-mobilenet-v2 \
    /dev/video0
```

Test several objects:

- Person
- Chair
- Backpack
- Bottle
- Laptop
- Cell phone

Record the results:

| Object | Detected? | Approximate confidence | Comments |
|--------|-----------|------------------------|----------|
| Person | | | |
| Chair | | | |
| Backpack | | | |
| Bottle | | | |
| Laptop | | | |
| Cell phone | | | |

### Question 3

Which objects were detected most reliably?

### Question 4

Which objects were missed or incorrectly classified?

### Question 5

How did lighting, distance, camera angle, or obstruction affect the results?

---

## 1.3 Monitor Performance

Keep object detection running.

Open a second terminal and run:

```bash
tegrastats
```

Observe the output for approximately 15 seconds.

Record:

| Metric | Before inference | During inference |
|--------|------------------|------------------|
| RAM usage | | |
| CPU utilization | | |
| GPU utilization | | |
| GPU temperature | | |
| Network FPS | | |

Press:

```text
Ctrl+C
```

to stop `tegrastats`.

### Question 6

Which resource changed the most during inference?

### Question 7

Was the CPU or GPU responsible for most of the inference workload?

### Question 8

Was the observed frame rate adequate for real-time operation? Explain.

---

# Task 2 – Semantic Segmentation

Semantic segmentation assigns a class label to each image pixel.

Run segmentation on a sample urban image:

```bash
./segnet.py \
    --network=fcn-resnet18-cityscapes \
    images/city_0.jpg \
    images/test/segmentation_city.jpg
```

If necessary, use:

```bash
./segnet \
    --network=fcn-resnet18-cityscapes \
    images/city_0.jpg \
    images/test/segmentation_city.jpg
```

Open:

```text
images/test/segmentation_city.jpg
```

Record at least three identified classes:

| Pixel class | Example location in image |
|-------------|---------------------------|
| | |
| | |
| | |

### Question 9

How is semantic segmentation different from object detection?

### Question 10

Why is segmentation useful for autonomous-driving or robotic-navigation applications?

### Question 11

Does segmentation distinguish two separate objects that belong to the same class? Explain.

---

# Task 3 – Human Pose Estimation

Pose estimation identifies human body keypoints, such as shoulders, elbows, knees, and ankles.

Run pose estimation:

```bash
./posenet.py \
    "images/humans_*.jpg" \
    images/test/pose_humans_%i.jpg
```

If necessary, use:

```bash
./posenet \
    "images/humans_*.jpg" \
    images/test/pose_humans_%i.jpg
```

Open the generated images in:

```text
images/test/
```

Record your observations:

| Observation | Result |
|-------------|--------|
| Number of people | |
| Clearly detected keypoints | |
| Missed keypoints | |
| Incorrect skeletal connections | |

### Question 12

What is a body keypoint?

### Question 13

Under what conditions might pose estimation fail?

### Question 14

Identify two AIoT applications that could use pose estimation.

---

# Task 4 – Compare the Four Inference Tasks

Complete the following table:

| Task | Input | Output | Example AIoT application | Relative computational demand |
|------|-------|--------|---------------------------|-------------------------------|
| Classification | | | | |
| Object detection | | | | |
| Segmentation | | | | |
| Pose estimation | | | | |

---

## Question 15

Which task provides the least spatial information?

## Question 16

Which task provides the most detailed information about an image?

## Question 17

Which task appeared to require the most computation?

## Question 18

Which task would you select for each application?

1. Determining whether an image contains a dog
2. Counting people entering a room
3. Identifying drivable road regions
4. Monitoring human exercise posture

---

# Optional Extension – Remote Streaming

Find the Jetson IP address:

```bash
ip addr show
```

Record:

```text
Jetson IP address: _______________________________
```

Run object detection with a WebRTC output:

```bash
./detectnet.py \
    --network=ssd-mobilenet-v2 \
    csi://0 \
    webrtc://@:8554/output
```

On another computer connected to the same network, open:

```text
https://<JETSON-IP>:8554
```

Depending on the installed Jetson-Inference version, the stream may also be available at:

```text
https://<JETSON-IP>:8554/output
```

> [!NOTE]
> Campus network settings or browser security policies may prevent WebRTC streaming. This activity is optional and does not affect completion of the core lab.

---

# System Reflection

Discuss the following with your group:

1. Why is object detection more computationally demanding than classification?
2. Why might segmentation provide more useful information than bounding boxes?
3. Why can pose estimation be difficult when people overlap?
4. What are the advantages of performing these tasks on the Jetson instead of sending every image to a cloud server?
5. What limitations did you observe in the pretrained models?
6. How could model compression improve this system?

---

# Part 2 Deliverables

Submit:

- Static object-detection output image
- Screenshot of real-time object detection
- Screenshot of `tegrastats` during inference
- Completed object-detection tables
- Semantic-segmentation output
- Completed segmentation table
- Pose-estimation output
- Completed pose-estimation table
- Completed four-task comparison table
- Answers to Questions 1–18
- A brief conclusion comparing the four inference tasks

---

# Graduate Extension – ECE 6930

Select one extension.

## Extension A – Performance Comparison

Compare two available object-detection networks.

| Model | FPS | RAM usage | GPU utilization | Detection quality |
|-------|-----|-----------|-----------------|-------------------|
| SSD-Mobilenet-v2 | | | | |
| Second model | | | | |

Discuss the trade-off among model complexity, speed, and detection quality.

## Extension B – Edge-versus-Cloud Analysis

Write approximately one page discussing:

- Expected network bandwidth for cloud-based video inference
- Latency requirements
- Privacy implications
- Reliability during network outages
- Advantages of local inference
- Situations in which cloud inference may still be preferable

## Extension C – AIoT Application Design

Propose an AIoT application based on one task from this laboratory.

Include:

- Application scenario
- Selected inference task
- Sensor or camera placement
- Required latency
- Communication method
- Decision or action produced by the system
- Expected limitations
