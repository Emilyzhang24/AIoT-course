# Lab 1: Introduction to Edge AI Computing

## Learning Objectives

By the end of this laboratory, you will be able to:

- Describe the role of an edge AI platform in an AIoT system.
- Identify the major hardware components of an NVIDIA Jetson platform.
- Verify the operating system and JetPack installation.
- Monitor CPU, GPU, and memory utilization.
- Detect and verify the CSI camera.
- Explain why edge computing is important for AIoT applications.

---

## Background

Artificial Intelligence of Things (AIoT) combines sensing, communication, and artificial intelligence to enable intelligent decision-making close to where data is generated.

Unlike cloud AI, edge AI performs inference directly on embedded devices, reducing latency, bandwidth usage, and privacy concerns.

In this laboratory, you will become familiar with the NVIDIA Jetson platform that will be used throughout the semester.

---

## Equipment

Each group should have:

- NVIDIA Jetson (Seeed reComputer)
- CSI Camera (Sony IMX219)
- HDMI monitor
- Keyboard
- Mouse
- Power adapter
- Internet connection

---

# Part 1 – Explore the Hardware
## Part 1 – Hardware Overview

Before powering on the system, identify the following interfaces shown in **Figure 1**.

![Jetson Ports](images/Jetson_ports.png)
**Figure 1.** External interfaces of the Seeed Studio reComputer.

| Label | Interface | Function |
|-------|-----------|----------|
| 1 | DC Power | Supplies power to the Jetson |
| 2 | HDMI | Display output |
| 3 | USB 3.0 | High-speed peripherals |
| 4 | USB 2.0 | Keyboard and mouse |
| 5 | Ethernet | Wired network connection |
| 6 | USB-C | Device interface (if applicable) |


### Question 1 
Why is the CSI camera preferred over a USB webcam for embedded AI applications?

---

# Part 2 – Boot the Jetson

### Procedure

1. Connect the HDMI monitor.
2. Connect the keyboard.
3. Connect the mouse.
4. Connect the power supply.
5. Boot the Jetson.

### Record

- Ubuntu version (if available)
- Desktop appearance

📷 **Take a screenshot of the desktop.**

---

# Part 3 – Verify the JetPack Installation

Open a terminal and run

```bash
cat /etc/nv_tegra_release
```

Record the output.

**Example**

```text
R32 (release), REVISION: 7.1
```

### Question 2

Which JetPack version is installed?

---

# Part 4 – Monitor System Resources

Run

```bash
tegrastats
```

Observe the following:

- RAM usage
- CPU utilization
- GPU temperature
- CPU frequency

**Example**

```text
RAM 1620/3956MB
GPU@25.5C
CPU [2%@1224,2%@1224,4%@1224,3%@1224]
```

Press

```text
Ctrl + C
```

to stop monitoring.

### Question 3

How much RAM is installed?

### Question 4

Why is GPU temperature reported even when the GPU is mostly idle?

### Question 5

Why is monitoring resource utilization important for edge AI systems?

---

# Part 5 – Verify Camera Detection

Run

```bash
v4l2-ctl --list-devices
```

**Expected output**

```text
vi-output, imx219 8-0010 (platform:54080000.vi:4):

    /dev/video0
```

### Question 6

What camera sensor is installed?

### Question 7

Which device corresponds to the camera?

### Question 8

What does `/dev/video0` represent?

---

# Part 6 – System Reflection

Discuss the following questions with your group.

1. What advantages does edge AI offer compared with cloud AI?
2. Why is GPU acceleration important for AI inference?
3. Why might AIoT systems require both sensing and communication?

---

# Deliverables

Submit the following:

- Screenshot of the Ubuntu desktop
- Output of

```bash
cat /etc/nv_tegra_release
```

- Screenshot of

```bash
tegrastats
```

- Output of

```bash
v4l2-ctl --list-devices
```

- Written answers to Questions 1–8

---

## Challenge (Optional)

Explore the Jetson system further by answering the following:

1. What is the CPU model used in the Jetson?
2. How many CPU cores are available?
3. What GPU architecture does the Jetson Nano use?
4. Why is a GPU advantageous for deep learning inference?
