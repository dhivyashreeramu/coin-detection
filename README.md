# WORKSHOP 4 – COIN DETECTION USING OPENCV IN PYTHON
# Name : DHIVYASHREE R
# REGNO : 212225040077

## Aim

To detect and count coins in an image using Python and OpenCV.

## Software Required

1. Anaconda – Python
2. OpenCV
3. NumPy
4. Matplotlib

## Algorithm

**Step 1:** Import the necessary packages such as OpenCV, NumPy, Matplotlib and OS.

**Step 2:** Read the input coin image using `cv2.imread()`.

**Step 3:** Convert the original image into a grayscale image using `cv2.cvtColor()`.

**Step 4:** Apply Gaussian Blur to remove noise and use Canny Edge Detection to identify the edges of the coins.

**Step 5:** Find the contours in the image using `cv2.findContours()`.

**Step 6:** Calculate the contour area and filter out small contours to remove unwanted noise.

**Step 7:** Detect the coins using the minimum enclosing circle and draw a green circle around each detected coin.

**Step 8:** Count the detected coins and display the count in red colour with increased font size and thickness.

**Step 9:** Display the original image, grayscale image, blurred image, edge detection image and final detected coin image.

## Program

```python
# Step 1: Load the necessary packages
import cv2
import numpy as np
import matplotlib.pyplot as plt
import os

# Step 2: Read the image
image_path = "coin.jpeg"

image = cv2.imread(image_path)

if image is None:
    raise FileNotFoundError(f"Image not found at {image_path}")

# Step 3: Convert the image to grayscale
gray = cv2.cvtColor(image, cv2.COLOR_BGR2GRAY)

# Step 4: Apply Gaussian Blur and Canny Edge Detection
blurred = cv2.GaussianBlur(gray, (11, 11), 0)
edges = cv2.Canny(blurred, 30, 150)

# Step 5: Find contours
contours, _ = cv2.findContours(
    edges.copy(),
    cv2.RETR_EXTERNAL,
    cv2.CHAIN_APPROX_SIMPLE
)

# Step 6: Detect and count coins
output = image.copy()

coin_count = 0

for contour in contours:
    # Filter out small contours
    area = cv2.contourArea(contour)

    if area < 300:
        continue

    # Draw contour using minimum enclosing circle
    ((x, y), radius) = cv2.minEnclosingCircle(contour)

    if radius > 15:
        coin_count += 1

        cv2.circle(
            output,
            (int(x), int(y)),
            int(radius),
            (0, 255, 0),
            2
        )

        # Display coin number in red colour
        cv2.putText(
            output,
            f"{coin_count}",
            (int(x - 10), int(y + 10)),
            cv2.FONT_HERSHEY_SIMPLEX,
            2.6,
            (0, 0, 255),
            6
        )

print(f"Total coins detected: {coin_count}")

# Step 7: Display original and detected coin images
plt.figure(figsize=(12, 6))

plt.subplot(1, 2, 1)
plt.title("Original Image")
plt.imshow(cv2.cvtColor(image, cv2.COLOR_BGR2RGB))
plt.axis("off")

plt.subplot(1, 2, 2)
plt.title(f"Detected Coins: {coin_count}")
plt.imshow(cv2.cvtColor(output, cv2.COLOR_BGR2RGB))
plt.axis("off")

plt.show()

# Step 8: Display all processing stages
fig, axs = plt.subplots(1, 5, figsize=(20, 6))

axs[0].imshow(cv2.cvtColor(image, cv2.COLOR_BGR2RGB))
axs[0].set_title("Original Image")
axs[0].axis("off")

axs[1].imshow(gray, cmap='gray')
axs[1].set_title("Grayscale Image")
axs[1].axis("off")

axs[2].imshow(blurred, cmap='gray')
axs[2].set_title("Blurred Image")
axs[2].axis("off")

axs[3].imshow(edges, cmap='gray')
axs[3].set_title("Edge Detection (Canny)")
axs[3].axis("off")

axs[4].imshow(cv2.cvtColor(output, cv2.COLOR_BGR2RGB))
axs[4].set_title(f"Detected Coins: {coin_count}")
axs[4].axis("off")

plt.tight_layout()
plt.show()
```

## Output

### 1. Original Image 
<img width="950" height="464" alt="image" src="https://github.com/user-attachments/assets/f215ed8c-c9c5-4a83-9c3f-c20179b3d4f5" />

### 2. Grayscale Image
<img width="332" height="355" alt="image" src="https://github.com/user-attachments/assets/12a9e3a2-afb3-4fbf-b549-95b9988c2069" />

### 3. Blurred Image
<img width="333" height="353" alt="image" src="https://github.com/user-attachments/assets/5fa3557f-b134-49c2-8001-35fa4c9d92d6" />

### 4. Edge Detection using Canny
<img width="342" height="362" alt="image" src="https://github.com/user-attachments/assets/7f27b6dc-99a8-462f-b848-2eab7c816b37" />

### 5. Detected Coins
<img width="325" height="358" alt="image" src="https://github.com/user-attachments/assets/866008b5-9f58-41c4-bd00-6a022be59ca2" />



## Result

Thus, the coins are successfully detected and counted in the input image using Python and OpenCV. Gaussian Blur, Canny Edge Detection and Contour Detection techniques are used to identify the coins.
