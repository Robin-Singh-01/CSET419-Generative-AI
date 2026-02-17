# Baseline Encoder–Decoder CNN for Image-to-Image Translation (CIFAR10)

## Objective
This project implements a **baseline encoder–decoder CNN** for **paired image-to-image translation** using the CIFAR10 dataset.  
The model is trained using only **reconstruction loss (MSE + L1)**, without using GANs.

As expected, the translated output images appear **blurry**, which is a known limitation of pixel-wise reconstruction loss.

---

## Task Overview
The pipeline performs the following steps:

1. Load CIFAR10 images
2. Create paired training data:
   - **Input:** RGB CIFAR10 image  
   - **Target:** Grayscale version of the same image (converted back to 3 channels)
3. Normalize both input and target to **[-1, 1]**
4. Train an encoder–decoder CNN
5. Compute reconstruction loss:
   - Mean Squared Error (MSE)
   - L1 Loss
6. Visualize results (Input, Target, Output)

---

## Dataset
- **CIFAR10**
- Image size: **32 × 32**
- Channels: **3 (RGB)**

---

## Model Architecture

### Encoder
The encoder compresses the input using strided convolutions:

- Conv2D (stride=2): 32×32 → 16×16  
- Conv2D (stride=2): 16×16 → 8×8  
- Conv2D (stride=2): 8×8 → 4×4  

Latent representation shape:

- **(256, 4, 4)**

---

### Decoder
The decoder reconstructs the output using transpose convolutions:

- ConvTranspose2D: 4×4 → 8×8  
- ConvTranspose2D: 8×8 → 16×16  
- ConvTranspose2D: 16×16 → 32×32  

Final activation:
- **Tanh**, to match the output range **[-1, 1]**

---

## Loss Function
The training loss is:

\[
Loss = MSE(output, target) + L1(output, target)
\]

- **MSE** enforces overall similarity
- **L1** reduces error and slightly improves sharpness

---

## Training
Example training call:

```python
train_model(epochs=20)

![Generated Output]()
