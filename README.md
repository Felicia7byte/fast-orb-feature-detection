# fast-orb-feature-detection
This project demonstrates corner/keypoint detection using FAST and ORB algorithms with OpenCV. The detected keypoints are visualized on sample images to compare the characteristics of both methods.
# Objective
This project aims to:
- Understand the concept of corner and keypoint detection in computer vision.
- Implement FAST and ORB feature detection using OpenCV.
- Visualize detected keypoints on different images.
- Compare the number and spatial distribution of keypoints detected by FAST and ORB.
- Analyze how image characteristics affect corner/keypoint detection.
- Understand the differences between FAST and ORB and their applications in computer vision.
# Methods
This project uses OpenCV to detect and visualize distinctive points in images using two feature detection methods: FAST (Features from Accelerated Segment Test) and ORB (Oriented FAST and Rotated BRIEF).

The overall process consists of loading the input images, detecting keypoints using FAST and ORB, visualizing the detected keypoints, and comparing the results based on the number and distribution of detected keypoints.

1. Image Input
The input images are stored in the images/ directory. Each image is read using OpenCV and processed individually.

The image is loaded using:

img = cv2.imread(os.path.join(folder_path, filename))

The images are then used as input for both FAST and ORB feature detection.

2. FAST Corner Detection
FAST stands for Features from Accelerated Segment Test. It is a corner detection algorithm designed to detect corners efficiently and with low computational cost.

The basic idea of FAST is to examine the pixels surrounding a candidate pixel and determine whether the candidate represents a corner.

For each candidate pixel, FAST considers a circle of 16 pixels around it. The intensity of the candidate pixel is compared with the pixels in this circle.

A pixel can be classified as a corner when a sufficient number of contiguous pixels in the circle are significantly brighter or darker than the candidate pixel.

Conceptually:

The center pixel is selected as a candidate.
A circle of 16 surrounding pixels is examined.
The intensity of the surrounding pixels is compared with the center pixel.
If a sufficient number of contiguous pixels are significantly brighter or darker, the center pixel is considered a corner.
Non-maximum suppression can then be used to remove weaker or redundant corner responses.
In this project, FAST is implemented using OpenCV:

fast = cv2.FastFeatureDetector_create()
keypoints_fast = fast.detect(img, None)

The resulting keypoints_fast contains the detected corner points.

3. ORB Feature Detection
ORB stands for Oriented FAST and Rotated BRIEF. ORB is a feature detection and description method that combines the FAST keypoint detector with an orientation-aware version of the BRIEF descriptor.

In this project, ORB is used to detect distinctive keypoints from the input images.

The ORB detector is initialized using:

orb = cv2.ORB_create()
keypoints_orb = orb.detect(img, None)

ORB internally uses FAST to identify candidate keypoints. However, unlike basic FAST detection, ORB adds additional processing to make the detected features more useful for computer vision tasks.

The main stages of ORB are:

FAST Keypoint Detection

ORB first uses the FAST detector to find potential corner/keypoint locations in the image.

Keypoint Selection

ORB ranks the detected keypoints and keeps the strongest features according to its configuration. In the default OpenCV configuration, the maximum number of retained features is approximately 500 because nfeatures defaults to 500.

Orientation Assignment

ORB estimates the orientation of each keypoint using the intensity distribution around the keypoint. This gives each feature an orientation, allowing ORB to handle changes in image rotation more effectively.

BRIEF Descriptor

ORB uses a modified and rotated version of BRIEF to generate a binary descriptor for the detected keypoints. The descriptor represents the local appearance around each keypoint and can later be used for feature matching.

In the current implementation, only the keypoint detection stage is explicitly retrieved using orb.detect(). Descriptor extraction is not performed because detectAndCompute() is not used.

4. Keypoint Visualization
After detecting keypoints, the detected points are drawn onto the original images using OpenCV's drawKeypoints() function.

FAST and ORB keypoints are displayed using different colors so that their distributions can be visually compared.

fast_result = cv2.drawKeypoints(
    img, keypoints_fast, None, color=(255, 0, 0)
)

orb_result = cv2.drawKeypoints(
    img, keypoints_orb, None, color=(0, 0, 255)
)

The resulting images are converted from BGR to RGB before being displayed using Matplotlib.

The FAST and ORB results are then displayed side by side for each input image.

5. Keypoint Count
The number of detected keypoints is calculated using Python's len() function:

len(keypoints_fast)
len(keypoints_orb)

This allows the number of keypoints detected by FAST and ORB to be compared for each image.
# Installation
```bash
pip install opencv-python matplotlib
```
# Results
<img width="990" height="912" alt="image" src="https://github.com/user-attachments/assets/b657dfd9-af49-401b-826d-057a88e4e827" />

| Image | FAST Keypoints | ORB Keypoints |
|:------|---------------:|--------------:|
| `1.jpg` | 15,412 | 500 |
| `2.jpg` | 23,348 | 500 |

The difference in FAST keypoint counts indicates that the number of detected corners can vary depending on the visual characteristics of an image, such as edges, textures, and structural details.

The ORB results are approximately 500 keypoints in this experiment because the default OpenCV ORB configuration uses nfeatures=500, which limits the number of features retained.
