# Lab 2: Real-Time Object Detection on the Edge

## Learning Objectives

By the end of this laboratory, you will be able to:

- Deploy a pretrained object detection model on the NVIDIA Jetson platform.
- Capture and process real-time images from the CSI camera.
- Interpret object detection results produced by a deep learning model.
- Monitor system resource utilization during AI inference.
- Explain the advantages of edge AI over cloud-based inference.

---

## Background

Object detection is one of the most widely used computer vision tasks in AIoT applications. Unlike image classification, object detection identifies **what** objects are present and **where** they are located in an image.

Typical AIoT applications include:

- Smart surveillance
- Autonomous robots
- Intelligent transportation
- Smart manufacturing
- Healthcare monitoring

In this laboratory, you will deploy a pretrained object detection model on the NVIDIA Jetson platform and observe real-time inference using the onboard CSI camera.

---

## AIoT Processing Pipeline

```text
CSI Camera
      │
      ▼
 Image Acquisition
      │
      ▼
 Deep Neural Network
      │
      ▼
 Object Detection
      │
      ▼
 Visualization & Decision
```

Unlike cloud-based AI systems, all inference is performed locally on the Jetson platform.

---

# Part 1 – Verify the Camera

Before running the AI application, verify that the camera is detected.

Run

```bash
v4l2-ctl --list-devices
```

Expected output

```text
vi-output, imx219 ...

/dev/video0
```

### Checkpoint

If the camera is not detected, notify the instructor before proceeding.

---

# Part 2 – Run Real-Time Object Detection

Launch the object detection demo.

```bash
# Command provided by the instructor
```

Observe the live video stream.

Move different objects in front of the camera, such as:

- Person
- Laptop
- Cell phone
- Bottle
- Chair
- Backpack

---

## Record Your Observations

| Object | Detected? | Confidence (if available) |
|---------|-----------|---------------------------|
| Person | | |
| Laptop | | |
| Bottle | | |
| Chair | | |
| Cell Phone | | |

---

### Question 1

Which objects were detected correctly?

### Question 2

Which objects were missed?

### Question 3

Why do you think certain objects were more difficult to detect?

---

# Part 3 – Monitor System Performance

Open another terminal and run

```bash
tegrastats
```

Observe

- RAM usage
- CPU utilization
- GPU utilization
- GPU temperature

Record your observations while object detection is running.

---

### Question 4

Did CPU utilization increase during inference?

### Question 5

What happened to GPU utilization?

### Question 6

How much memory was used?

---

# Part 4 – Measure Inference Performance

Record the following information.

| Metric | Value |
|---------|-------|
| Resolution | |
| FPS | |
| CPU Utilization | |
| GPU Utilization | |
| RAM Usage | |

---

### Question 7

Is the observed frame rate sufficient for real-time applications?

Explain your answer.

---

### Question 8

Suggest one method to improve inference speed.

---

# Part 5 – Discussion

Discuss the following questions with your group.

1. Why is edge AI preferred over cloud AI for real-time applications?

2. Which AIoT applications require low latency?

3. Why might sending every camera frame to the cloud be undesirable?

---

# Deliverables

Submit the following:

- Screenshot of the running object detection system
- Screenshot of the `tegrastats` output
- Completed performance table
- Answers to Questions 1–8

---

# Graduate Extension (ECE 6930)

Read the documentation of the deployed object detection model.

Discuss:

- Which neural network architecture is used?
- Why is this architecture suitable for edge AI?
- How could the model be further optimized for deployment?

---

## Challenge (Optional)

Design an AIoT application that could benefit from real-time object detection.

Your proposal should include:

- Application scenario
- Camera placement
- Objects of interest
- Why edge AI is preferred over cloud AI
