# Lab 2 – Part 1: Jetson-Inference Setup and Image Classification


[Return to the Lab 2 overview](README_lab2.md)

---

## Part 1 Objectives

By the end of this session, you will be able to:

- Verify the Jetson software environment.
- Explain where Jetson-Inference fits in the edge AI software stack.
- Obtain and build the Jetson-Inference source code.
- Install the compiled library and example programs.
- Run image classification using a pretrained ResNet-18 model.
- Interpret the predicted class and confidence value.

---

## Background

**Inference** is the process of applying a trained AI model to new data to produce a prediction.

For example:

```text
Input image
     │
     ▼
Pretrained neural network
     │
     ▼
Predicted class and confidence
```

The model used in this part has already been trained. You will focus on deploying and executing the model on the Jetson.

Image classification assigns one primary class label to an entire image. It does not identify where the object is located.

---

# Step 1 – Verify the Jetson Environment

Open a terminal.

## 1.1 Check the JetPack release

Run:

```bash
cat /etc/nv_tegra_release
```

Record the output:

```text
__________________________________________________________
```

The expected release for the current course image is similar to:

```text
R32 (release), REVISION: 7.1
```

## 1.2 Check available storage

Run:

```bash
df -h
```

Record the available space on the main filesystem:

```text
Available storage: __________________
```

## 1.3 Check the camera

Run:

```bash
v4l2-ctl --list-devices
```

Expected output is similar to:

```text
vi-output, imx219 8-0010 (platform:54080000.vi:4):
    /dev/video0
```

## 1.4 Check system resources

Run:

```bash
tegrastats
```

Observe the output for approximately 10 seconds.

Record:

| Metric | Observed value |
|--------|----------------|
| RAM usage | |
| CPU utilization | |
| GPU temperature | |
| CPU frequency | |

Press:

```text
Ctrl+C
```

to stop `tegrastats`.

---

## Question 1

What JetPack/L4T release is installed?

## Question 2

Why should the JetPack version be verified before installing an AI library?

## Question 3

What does `/dev/video0` represent?

---

# Step 2 – Obtain Jetson-Inference

> [!NOTE]
> The instructor may provide a pre-downloaded copy of the repository if the campus network is unavailable or slow.

Navigate to your home directory:

```bash
cd ~
```

Check whether Jetson-Inference is already present:

```bash
ls
```

If a `jetson-inference` directory is already present, ask the instructor whether to use it or replace it.

Clone the repository and its required submodules:

```bash
git clone --recursive https://github.com/dusty-nv/jetson-inference
```

Enter the repository:

```bash
cd ~/jetson-inference
```

Confirm that the repository contains files:

```bash
ls
```

Record the source-code version:

```bash
git rev-parse HEAD
```

```text
Commit ID: ______________________________________________
```

---

# Step 3 – Install Required Build Tools

Run:

```bash
sudo apt-get update
```

Install the basic build dependencies:

```bash
sudo apt-get install -y \
    git \
    cmake \
    build-essential \
    libpython3-dev \
    python3-numpy
```

> [!NOTE]
> Some dependencies may already be installed on the course image.

---

# Step 4 – Configure the Build

Return to the Jetson-Inference source directory:

```bash
cd ~/jetson-inference
```

Create the build directory:

```bash
mkdir -p build
```

Enter the build directory:

```bash
cd build
```

Configure the project:

```bash
cmake ../
```

During configuration, the software may ask which pretrained models should be downloaded.

Select only the models required for this laboratory, including:

- ResNet-18 image classification
- SSD-Mobilenet-v2 object detection
- FCN-ResNet18 Cityscapes segmentation
- Pose estimation model

> [!NOTE]
> The exact selection interface may depend on the installed Jetson-Inference version.

---

# Step 5 – Compile Jetson-Inference

Compile using two parallel jobs:

```bash
make -j2
```

The build may take several minutes.

While waiting, discuss the following with your group:

1. What is the role of the compiler?
2. Why might compilation take longer on a Jetson Nano than on a desktop workstation?
3. Why are only two parallel jobs used on a 4 GB Jetson?

Record the approximate build time:

```text
Build time: __________________ minutes
```

---

# Step 6 – Install the Library

After the build completes successfully, run:

```bash
sudo make install
```

Then update the shared-library cache:

```bash
sudo ldconfig
```

Verify that the example applications exist:

```bash
cd ~/jetson-inference/build/aarch64/bin
```

```bash
ls imagenet* detectnet* segnet* posenet*
```

Record the programs that are present:

```text
__________________________________________________________
__________________________________________________________
```

---

## Question 4

What is the difference between building and installing a software library?

## Question 5

Why is the `--recursive` option used when cloning Jetson-Inference?

## Question 6

Why is `sudo ldconfig` used after installation?

---

# Step 7 – Run Image Classification

Create an output directory:

```bash
cd ~/jetson-inference/build/aarch64/bin
```

```bash
mkdir -p images/test
```

Run image classification using ResNet-18:

```bash
./imagenet.py \
    --network=resnet-18 \
    images/orange_0.jpg \
    images/test/classification_orange.jpg
```

If your installation provides the compiled executable rather than the Python program, use:

```bash
./imagenet \
    --network=resnet-18 \
    images/orange_0.jpg \
    images/test/classification_orange.jpg
```

Open the output image using the Ubuntu file manager:

```text
~/jetson-inference/build/aarch64/bin/images/test/classification_orange.jpg
```

Record the result:

| Input image | Predicted class | Confidence |
|-------------|-----------------|------------|
| `orange_0.jpg` | | |

---

# Step 8 – Test a Second Image

Select one additional sample image from the `images` directory.

List available images:

```bash
ls images
```

Run the classifier:

```bash
./imagenet.py \
    --network=resnet-18 \
    images/INPUT_IMAGE.jpg \
    images/test/classification_second.jpg
```

Replace `INPUT_IMAGE.jpg` with the selected filename.

Record the result:

| Input image | Predicted class | Confidence | Correct? |
|-------------|-----------------|------------|----------|
| | | | |

---

## Question 7

What information does the confidence value provide?

## Question 8

Does image classification identify the location of an object? Explain.

## Question 9

What might happen if an image contains several different objects?

## Question 10

Why is ResNet-18 appropriate for an introductory edge AI experiment?

---

# Session Reflection

Complete the following software-stack diagram by describing the role of each layer.

| Layer | Role |
|------|------|
| Ubuntu | |
| JetPack | |
| CUDA/cuDNN | |
| TensorRT | |
| Jetson-Inference | |
| ResNet-18 | |

---

# Part 1 Deliverables

Submit:

- Output from `cat /etc/nv_tegra_release`
- Jetson-Inference commit ID
- Approximate compilation time
- Screenshot showing successful compilation or installation
- Classification output for `orange_0.jpg`
- Classification output for one additional image
- Completed classification tables
- Answers to Questions 1–10
- Completed software-stack table

---

## Exit Question

In two or three sentences, explain the difference between:

- Installing an AI framework
- Running an AI model
- Training an AI model
