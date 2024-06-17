# MINS: Efficient and Robust Multisensor-aided Inertial Navigation System

## Overview

### Objective
Develop a tightly-coupled Multisensor-aided Inertial Navigation System (MINS) capable of fusing common navigation sensors.

### Challenges Addressed
- Computational complexity
- Sensor asynchronicity
- Intra-sensor calibration

### Features
- High-order on-manifold interpolation for efficient asynchronous sensor fusion
- Dynamic cloning for computational efficiency
- Online intrinsic and extrinsic sensor calibration
- Robust initialization using proprioceptive measurements

### Performance
Validated through simulations and real-world datasets, outperforming existing methods in localization accuracy, consistency, and computation efficiency.

## Key Contributions

### Efficient and Robust Filtering-Based MINS Estimator
- Fuses IMUs, wheel encoders, cameras, LiDARs, and GPS in a tightly-coupled fashion
- Resilient to sensor failures and measurement outliers

### High-Order On-Manifold State Interpolation
- Processes asynchronous and delayed measurements
- Reduces computational complexity while minimizing trajectory representation errors

### Online Calibration
- Intrinsic and extrinsic calibration of all onboard sensors
- Adapts to poor prior calibration and time-varying calibration

### Robust Initialization
- Uses IMU and wheel-encoder measurements for initialization in dynamic environments

## System Components and Fusion Methods

### IMU
Used for state propagation with high-rate measurements.

### Camera
Provides visual features for MSCKF updates.

### Wheel Encoder
Supplies 2D linear and angular velocities.

### LiDAR
Offers 3D point clouds for environment mapping and localization.

### GNSS
Provides absolute position information.

## Adaptive On-Manifold Interpolation

### Interpolation Concept
Instead of augmenting the state with poses at every possible measurement time (which increases computational complexity), interpolation provides a way to estimate the state at any arbitrary time based on a limited number of known states.

### On-Manifold Interpolation
The system's state (e.g., pose) is represented on a manifold (a mathematical space that locally resembles Euclidean space but can have a more complex structure globally). Interpolation on this manifold respects the underlying geometry of the state space, ensuring accurate and consistent estimation.

### High-Order Polynomials
The interpolation uses polynomials of a higher degree (e.g., cubic or quintic) to model the state transition more accurately over time. Higher-order polynomials capture more complex motion profiles, reducing interpolation errors, especially in dynamic scenarios.

## Dynamic Cloning

### Cloning Concept
Cloning involves creating copies of the system's state at different times, which are then used for interpolation and measurement updates. Cloning frequency determines how often these state copies are created.

### Adaptive Cloning Frequency
The frequency of state cloning is adjusted dynamically based on the system's motion characteristics (e.g., linear and angular accelerations). During slow or steady motion, the cloning frequency can be reduced to save computational resources. During dynamic or rapid motion, the cloning frequency is increased to reduce interpolation errors and maintain accuracy.

## Camera Calibration

### Lens Distortion Correction
Corrects distortions such as radial and tangential distortions caused by the camera lens.

### Intrinsic Parameters
Determines the camera's internal characteristics, such as focal length, optical center (principal point), and skew coefficient.

### Extrinsic Parameters
Establishes the position and orientation of the camera in the world coordinate system.

## Detailed Explanation of Online Calibration

### Intrinsic and Extrinsic Calibration
Intrinsic calibration involves refining the internal parameters of sensors, such as the focal length and distortion coefficients for cameras. Extrinsic calibration involves determining the relative positions and orientations of the sensors with respect to a common reference frame.

### Joint State and Calibration Estimation
The state vector is augmented to include both the navigation state (position, orientation, velocity) and the calibration parameters (intrinsic and extrinsic). An Extended Kalman Filter (EKF) or a similar filtering technique is used to estimate these parameters jointly. The filter continuously updates the state and calibration parameters based on new sensor measurements, ensuring that the system remains accurate.

## Workflow and Necessity of Each Step in Online Calibration

1. **Sensor Data Collection**
   - Workflow: Collect data simultaneously from all sensors in the system.
   - Necessity: Provides the raw input needed for calibration. Ensures that data from different sensors are synchronised for accurate fusion and analysis.

2. **Preprocessing and Filtering**
   - Workflow: Apply filtering techniques to remove noise and outliers from the raw sensor data. Standardise the data format and align timestamps if necessary.
   - Necessity: Improves the quality and reliability of the sensor data. Ensures that the data fed into the calibration algorithm is clean and accurate, which is crucial for precise calibration.

3. **State and Calibration Estimation**
   - Workflow: Use algorithms like the Extended Kalman Filter (EKF) to estimate the system's state and calibration parameters. Integrate data from multiple sensors to refine these estimates.
   - Necessity: Provides an initial estimate of the system’s state and calibration parameters. Integrates data from various sensors to improve the overall accuracy and reliability of the state estimation.

4. **Error Analysis**
   - Workflow: Compare the estimated state and calibration parameters with actual measurements to calculate residuals (errors). Analyse these residuals to identify any deviations or inaccuracies.
   - Necessity: Detects deviations in sensor behaviour and calibration over time. Identifies specific areas where the calibration parameters need adjustment to maintain accuracy.

5. **Parameter Adjustment**
   - Workflow: Based on the error analysis, adjust the calibration parameters to correct any detected deviations. Update the calibration parameters dynamically to reflect the current state of the system.
   - Necessity: Corrects for any deviations detected during the error analysis. Ensures that the calibration parameters remain accurate and reflect the current state and behavior of the sensors.

6. **Update Calibration Parameters**
   - Workflow: Update the system with the newly adjusted calibration parameters. Ensure that these updated parameters are used in subsequent data collection and state estimation cycles.
   - Necessity: Applies the adjustments made to the calibration parameters, maintaining the system's accuracy. Ensures that future data collection and state estimation benefit from the most accurate calibration parameters.

## Summary of Workflow
- **Sensor Data Collection:** Collect raw data from all sensors simultaneously, ensuring synchronisation.
- **Preprocessing and Filtering:** Clean and standardise the data to remove noise and align timestamps.
- **State and Calibration Estimation:** Estimate the system’s state and calibration parameters using integrated sensor data.
- **Error Analysis:** Identify deviations by comparing estimated states with actual measurements.
- **Parameter Adjustment:** Adjust calibration parameters based on identified errors to correct deviations.
- **Update Calibration Parameters:** Update the system with the new calibration parameters for future cycles.

## Overall Necessity
Each step in the online calibration workflow is crucial to ensure that the system remains accurate and reliable over time. As sensors deviate in behavior and physical placement due to environmental changes and mechanical wear, continuous online calibration dynamically adjusts parameters to correct these deviations, maintaining the overall performance and accuracy of the multisensor system.

## Three Important Highlights of Activities in Online Calibration

### Real-Time Error Detection and Correction
- **Highlight:** Continuous monitoring and analysis of sensor data to detect deviations in real-time.
- **Importance:** Ensures immediate identification and correction of errors, maintaining system accuracy and reliability during operation.

### Dynamic Adjustment of Calibration Parameters
- **Highlight:** Adaptive adjustment of intrinsic and extrinsic calibration parameters based on real-time error analysis.
- **Importance:** Allows the system to remain calibrated despite changes in sensor behavior or physical placement, enhancing long-term stability and performance.

### Integrated Multisensor Data Fusion
- **Highlight:** Utilization of data from multiple sensors to refine state estimation and calibration parameters.
- **Importance:** Leverages the complementary strengths of different sensors, improving overall accuracy and robustness of the system’s state estimation and calibration.

## Sensor Statistics and Error Metrics

### [CAM0] Avg res/std
- **Avg res:** The average residual, which is the mean of the differences between the observed and predicted values. Here, it is \(2 \times 10^{-3}\) (0.002).
- **std:** The standard deviation of the residuals, which measures the spread or variability of the residuals. Here, it is \(1 \times 10^{0}\) (1.0).
- **R std:** Residual standard deviation, which indicates the typical size of the residuals. Here, it is \(1 \times 10^{0}\) (1.0).
- **Chi 95 %:** The chi-squared value for the 95% confidence interval, indicating how well the observed data fits the expected model. A lower value suggests a better fit. Here, it indicates the fit of the model to the data.
- **mean/std:** The mean and standard deviation of the residuals. Here, the mean is \(4 \times 10^{0}\) (4.0) and the standard deviation is \(3 \times 10^{0}\) (3.0).
- **# meas:** The number of measurements used in the calculation. Here, it is 3501.

## Summary of Each Term
- **Avg res:** Average difference between observed and predicted values.
- **std (for Avg res/std):** Spread of the residuals around their mean.
- **R std:** Standard deviation of the residuals.
- **Chi 95 %:** Chi-squared value for the 95% confidence interval, indicating the model fit.
- **mean (for mean/std):** Average of the residuals.
- **std (for mean/std):** Spread of the residuals around their mean.
- **# meas:** Total number of measurements considered.

# Sensor Data Accuracy and Reliability Metrics

These metrics collectively provide an insight into the accuracy and reliability of the sensor data and its fit to the expected model.

## Observed Values

Observed values are the actual positions of feature points in the images (for the camera) or in the point cloud (for the lidar) as detected by the sensors. These are the real measurements taken during the simulation.

## Predicted Values

Predicted values are the positions of these same feature points as calculated by the navigation system based on its internal model. These predictions are derived from the system's current understanding of its position, orientation, and the location of the feature points in the 3D environment.

## Reprojection Error

### For the Camera

1. **Detection of Feature Points**: Detect feature points in the 2D image captured by the camera.
2. **3D Reconstruction**: Use the current estimate of the camera's pose and the 3D positions of the feature points to project these points back onto the 2D image plane.
3. **Comparison**: Compare the projected positions (predicted values) with the actual detected positions in the image (observed values).
4. **Residual Calculation**: The difference between these positions is the residual (or reprojection error).

### For the Lidar

1. **Detection of Feature Points**: Detect feature points in the 3D point cloud captured by the lidar.
2. **3D Prediction**: Use the current estimate of the lidar's pose and the 3D positions of the feature points to predict where these points should be in the point cloud.
3. **Comparison**: Compare the predicted positions with the actual detected positions in the point cloud.
4. **Residual Calculation**: The difference between these positions is the residual error.

## In Summary

- **Observed values**: Actual positions of feature points detected by the sensors (camera or lidar).
- **Predicted values**: Positions of these feature points as estimated by the system based on its model.

### Example in Camera Context

- **Observed Value**: The actual pixel coordinates of a corner in the image detected by the camera.
- **Predicted Value**: The pixel coordinates where the system expects this corner to be based on its 3D model and current pose estimation.
- **Residual**: The difference between the observed and predicted pixel coordinates.

By analyzing the residuals and their statistics (average, standard deviation), the system can assess and improve the accuracy of its sensor fusion and navigation algorithms.

## Camera Intrinsics and Distortion Coefficients

### Camera Intrinsics

The intrinsic parameters of a camera describe the camera's internal characteristics that affect how it captures images. These parameters include focal lengths and the optical center of the image.

#### cam0:

- **intrinsics**: [817.050, 811.574, 607.950, 262.369] <= [816.904, 811.568, 608.507, 263.476]
- [fx, fy, cx, cy] <= [fx_prev, fy_prev, cx_prev, cy_prev]
  - fx: Focal length in the x-axis (horizontal) direction (current: 817.050, previous: 816.904).
  - fy: Focal length in the y-axis (vertical) direction (current: 811.574, previous: 811.568).
  - cx: x-coordinate of the optical center (principal point) (current: 607.950, previous: 608.507).
  - cy: y-coordinate of the optical center (principal point) (current: 262.369, previous: 263.476).

### Distortion Coefficients

Distortion coefficients correct for lens distortion, which can cause straight lines to appear curved in the captured images. There are typically radial and tangential distortion coefficients.

#### cam0:

- **distortion_coeffs**: [-0.056, 0.140, -0.001, -0.001] <= [-0.056, 0.140, -0.001, -0.001]
- [k1, k2, p1, p2] <= [k1_prev, k2_prev, p1_prev, p2_prev]
  - k1, k2: Radial distortion coefficients (current: -0.056, 0.140; previous: -0.056, 0.140).
  - p1, p2: Tangential distortion coefficients (current: -0.001, -0.001; previous: -0.001, -0.001).
  - p1, p2: Tangential distortion coefficients (current: 0.000, -0.001; previous: 0.000, -0.001).

### Summary of Terms

- **fx (focal length x)**: Determines how much the image is scaled horizontally.
- **fy (focal length y)**: Determines how much the image is scaled vertically.
- **cx (optical center x)**: The horizontal position of the optical center in the image.
- **cy (optical center y)**: The vertical position of the optical center in the image.
- **k1, k2 (radial distortion)**: Correct for the radial distortion, where points appear farther away or closer than they actually are.
- **p1, p2 (tangential distortion)**: Correct for the tangential distortion, where the image plane is not parallel to the lens.

### Code to run
docker run -it --rm \
    --env="DISPLAY=$DISPLAY" \
    --env="QT_X11_NO_MITSHM=1" \
    --volume="/tmp/.X11-unix:/tmp/.X11-unix:rw" \
    --memory="4g" \
    --name mins_container \
In this output, you see the current intrinsic and distortion parameters compared with their previous values, indicating how the parameters have changed during the calibration process. This comparison helps in assessing the stability and accuracy of the calibration over time.

