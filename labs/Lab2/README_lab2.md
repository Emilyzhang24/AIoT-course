# Lab 2: Real-Time AI Inference on the Edge

## Learning Objectives

By the end of this laboratory, you will be able to:

- Run pretrained deep-learning models using the Jetson-Inference library.
- Distinguish among image classification, object detection, semantic segmentation, and pose estimation.
- Perform object detection on both static images and a live CSI camera stream.
- Monitor CPU, GPU, memory, and temperature during inference.
- Stream processed video from the Jetson to a web browser.
- Explain the advantages and limitations of real-time edge AI.

---

## Background

The **Jetson-Inference** library provides pretrained deep-learning models optimized for NVIDIA Jetson devices using TensorRT.

This lab introduces four computer-vision tasks:

| Task | Output |
|------|--------|
| Image classification | One class label for the overall image |
| Object detection | Object classes, bounding boxes, and confidence values |
| Semantic segmentation | A class label for each image pixel |
| Pose estimation | Human body keypoints and skeletal links |

The primary focus of this laboratory is **real-time object detection**.

---

## AIoT Processing Pipeline

```text
CSI Camera
     │
     ▼
Image Acquisition
     │
     ▼
Deep-Learning Inference
     │
     ▼
Object Labels and Bounding Boxes
     │
     ▼
Local Display or Network Stream
```

All inference is performed locally on the Jetson.

---

## Prerequisites

The instructor has prepared the Jetson with:

- JetPack and Ubuntu
- Jetson-Inference
- Pretrained models
- Sony IMX219 CSI camera
- Network connectivity

> [!NOTE]
> Do not reinstall Jetson-Inference during the laboratory. If a required command or model is unavailable, notify the instructor.

---

# Part 1 – Locate the Jetson-Inference Programs

Open a terminal and navigate to the executable directory:

```bash
cd ~/jetson-inference/build/aarch64/bin
```

Confirm your current directory:

```bash
pwd
```

List the available inference programs:

```bash
ls imagenet* detectnet* segnet* posenet*
```

Create a directory for output images if it does not already exist:

```bash
mkdir -p images/test
```

### Question 1

What is the purpose of the `aarch64/bin` directory?

---

# Part 2 – Image Classification

Image classification assigns one primary class label to an entire image.

Run ResNet-18 on a sample image:

```bash
./imagenet.py --network=resnet-18 \
    images/orange_0.jpg \
    images/test/classification_orange.jpg
```

Use the Ubuntu file manager to open:

```text
images/test/classification_orange.jpg
```

Record the predicted class and confidence value.

| Input image | Predicted class | Confidence |
|-------------|-----------------|------------|
| `orange_0.jpg` | | |

### Question 2

Does image classification identify the location of the object in the image? Explain.

### Question 3

What information does the confidence value provide?

---

# Part 3 – Object Detection on a Static Image

Object detection identifies both the classes and locations of objects.

Run SSD-Mobilenet-v2 on a sample pedestrian image:

```bash
./detectnet.py --network=ssd-mobilenet-v2 \
    images/peds_0.jpg \
    images/test/detection_peds.jpg
```

Open the output image:

```text
images/test/detection_peds.jpg
```

Observe the bounding boxes, class labels, and confidence values.

| Observation | Result |
|-------------|--------|
| Number of detected objects | |
| Object classes | |
| Lowest observed confidence | |
| Highest observed confidence | |

### Question 4

How is object detection different from image classification?

### Question 5

Why are bounding boxes useful in an AIoT application?

---

# Part 4 – Verify the CSI Camera

Before running live inference, verify that the camera is detected:

```bash
v4l2-ctl --list-devices
```

Expected output will be similar to:

```text
vi-output, imx219 8-0010 (platform:54080000.vi:4):
    /dev/video0
```

Confirm that the following device exists:

```bash
ls -l /dev/video0
```

> [!WARNING]
> If the IMX219 camera or `/dev/video0` is not shown, stop and notify the instructor.

### Question 6

What camera sensor is connected to the Jetson?

### Question 7

What does `/dev/video0` represent?

---

# Part 5 – Real-Time Object Detection

Run object detection using the CSI camera:

```bash
./detectnet.py --network=ssd-mobilenet-v2 csi://0
```

If `csi://0` does not work on your system, use:

```bash
./detectnet.py --network=ssd-mobilenet-v2 /dev/video0
```

A video window should appear with bounding boxes, labels, confidence values, and network performance information.

Test several objects, such as:

- Person
- Chair
- Backpack
- Bottle
- Laptop
- Cell phone

Record your results:

| Object | Detected? | Approximate confidence | Comments |
|--------|-----------|------------------------|----------|
| Person | | | |
| Chair | | | |
| Backpack | | | |
| Bottle | | | |
| Laptop | | | |
| Cell phone | | | |

### Question 8

Which objects were detected most reliably?

### Question 9

Which objects were missed or incorrectly classified?

### Question 10

What environmental factors affected detection performance?

Consider:

- Distance
- Lighting
- Camera angle
- Partial obstruction
- Object size

---

# Part 6 – Detection-Threshold Experiment

The detection threshold controls the minimum confidence required before a detection is displayed.

Run the detector with a lower threshold:

```bash
./detectnet.py --network=ssd-mobilenet-v2 \
    --threshold=0.3 \
    csi://0
```

Then run it with a higher threshold:

```bash
./detectnet.py --network=ssd-mobilenet-v2 \
    --threshold=0.7 \
    csi://0
```

Use `/dev/video0` instead of `csi://0` if required.

Record your observations:

| Threshold | Number of detections | False detections | Missed objects |
|-----------|----------------------|------------------|----------------|
| 0.3 | | | |
| 0.5 | | | |
| 0.7 | | | |

### Question 11

What happened when the threshold was reduced?

### Question 12

What happened when the threshold was increased?

### Question 13

How would you select an appropriate threshold for a safety-critical application?

---

# Part 7 – Monitor System Performance

Keep the object detector running.

Open a second terminal and run:

```bash
tegrastats
```

Observe:

- RAM usage
- CPU utilization
- GPU utilization
- GPU frequency
- System temperature

Record approximately 10 seconds of observations.

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

### Question 14

Which system resource changed the most during inference?

### Question 15

Was the CPU or GPU responsible for most of the inference workload?

### Question 16

Why is performance monitoring important for edge AI systems?

---

# Part 8 – Stream the Detection Results to a Browser

Streaming allows inference results to be viewed remotely without requiring a monitor directly connected to the Jetson.

## Step 1: Find the Jetson IP Address

Run:

```bash
ip addr show
```

Locate the IP address associated with the active Ethernet or Wi-Fi interface.

Record the address:

```text
Jetson IP address: __________________________
```

## Step 2: Start WebRTC Streaming

Run:

```bash
./detectnet.py --network=ssd-mobilenet-v2 \
    csi://0 \
    webrtc://@:8554/output
```

If the CSI source does not work, use:

```bash
./detectnet.py --network=ssd-mobilenet-v2 \
    /dev/video0 \
    webrtc://@:8554/output
```

## Step 3: Open the Stream

On another computer connected to the same network, open a web browser and navigate to:

```text
https://<JETSON-IP>:8554
```

Replace `<JETSON-IP>` with the address recorded above.

> [!NOTE]
> The browser may display a security warning because the Jetson uses a local self-signed certificate. Follow the browser option to continue to the local site.
>
> On some installed versions, the stream may also be available at:
>
> `https://<JETSON-IP>:8554/output`

### Question 17

Was the streamed video noticeably delayed relative to the local display?

### Question 18

What additional computing and communication operations are needed for network streaming?

### Question 19

When would remote streaming be useful in an AIoT system?

---

# Part 9 – Optional Inference Tasks

Complete one of the following optional activities.

## Option A – Semantic Segmentation

Run semantic segmentation on an urban scene:

```bash
./segnet.py --network=fcn-resnet18-cityscapes \
    images/city_0.jpg \
    images/test/segmentation_city.jpg
```

Open:

```text
images/test/segmentation_city.jpg
```

Describe three pixel classes visible in the result.

---

## Option B – Pose Estimation

Run pose estimation on the sample human images:

```bash
./posenet.py \
    "images/humans_*.jpg" \
    images/test/pose_humans_%i.jpg
```

Open the generated output images in `images/test`.

Identify:

- Keypoints
- Skeletal links
- Any missed body parts

> [!NOTE]
> The first execution of a model may take several minutes while TensorRT optimizes the network. Later runs should start more quickly.

---

# Part 10 – System Reflection

Discuss the following questions with your group:

1. Why is object detection more computationally demanding than image classification?
2. What are the advantages of performing inference on the Jetson instead of sending every image to the cloud?
3. How do computation, sensing, and communication interact in the WebRTC experiment?
4. Which applications require real-time rather than delayed inference?
5. What limitations did you observe in the pretrained model?

---

# Deliverables

Submit one report per group containing:

- Output image from image classification
- Output image from static object detection
- Screenshot of real-time object detection
- Screenshot of `tegrastats` during inference
- Completed object-detection observation table
- Completed threshold experiment table
- Completed system-performance table
- Screenshot of the WebRTC stream
- Answers to Questions 1–19
- A brief conclusion discussing whether the Jetson provided adequate real-time performance

---

# Graduate Extension – ECE 6930

Select one of the following extensions:

## Extension 1 – Model Comparison

Compare SSD-Mobilenet-v2 with one other available object-detection model.

Record:

| Model | FPS | RAM usage | Detection quality |
|-------|-----|-----------|-------------------|
| SSD-Mobilenet-v2 | | | |
| Second model | | | |

Discuss the relationship among model complexity, speed, and detection quality.

## Extension 2 – Edge-versus-Cloud Analysis

Write approximately one page discussing:

- Expected communication bandwidth for cloud inference
- Privacy implications of transmitting live video
- Latency requirements of the selected application
- Advantages and disadvantages of local inference
- Conditions under which cloud inference may still be preferred

## Extension 3 – Application Design

Propose an AIoT system using real-time object detection.

Include:

- Application scenario
- Camera placement
- Objects to be detected
- Required detection latency
- Expected communication method
- Decision or action taken after detection
