---
layout: page
title: Robotics and Computer Vision
permalink: /education/rcv/
keywords: rutgers, robotics, computer, vision, dana, motion, affine, camera, calibration, stereo, epipolar, geometry, picture, images
description: A page with some notes from my robotics and computer vision class at Rutgers University
mathjax: true
---

**Note**: This class was taken during the fall semester of 2017 and was taught by professor Kristin Dana.

### Exam 2 Study Guide

#### Coordinate Frame Transformations


#### Camera Calibration

Remember the image formation pipeline:

> $$ World Coordinates -> Camera Coordinates -> 2D Coordinates -> Pixel Coordinates $$

Goal: find $$M$$, a 3x4 matrix

> $$ M = KR [ I | t ] $$

$$K$$ is the intrinsic parameter matrix and R is the 3x3 rotation matrix. $$I$$ is a 3x3 identity matrix and $$t$$ is the 3x1 translation vector.

Need a set of corresponding camera and world points in order to do this.

Recall the equation for finding the image points 

> $$ im_{P} = M W_P $$

In other words, given pixel coordinates and world coordinates for a corresponding point

> $$ \begin{bmatrix} x' \\ y' \\ w' \end{bmatrix} = MW_p $$

We then divide by $$w'$$ components to take care of the homogeneous coordinates (This is where we lose the information about the 3d component). Thus:

> $$ x_i = \frac{x'}{w'} = \frac{m_{11}X_i + m_{12}Y_i + m_{13}Z_i + m_{14}}{m_{31}X_i + m_{32}Y_i + m_{33}Z_i + m_{34}} $$

From here you can rearrange the equation for x and set the expression equal to 0.

> $$ 0 = m_{11}X_i + m_{12}Y_i + m_{13}Z_i + m_{14} -x_i(m_{31}X_i + m_{32}Y_i + m_{33}Z_i + m_{34}) $$

Similarly we get an equation using $$y_i$$

> $$ 0 = m_{21}X_i + m_{22}Y_i + m_{23}Z_i + m_{14} -y_i(m_{31}X_i + m_{32}Y_i + m_{33}Z_i + m_{34}) $$

Then, for each point in the matrix we can get 2 rows in a $$2Nx12$$ column matrix where the 12 columns come from the 12 parameters of the M matrix.

Thus we should be be able to solve for the matrix with around 6 corresponding points where the matrix is determined. (Use SVD and take the last column of V to get a solution in the null space) 

#### Next problem: Finding intrinsic/extrinsic parameters from M

- Position of camera in camera coordinates is always $$(0, 0, 0)$$
- Want to find camera in world coordinates. SVD of camera matrix gives camera center

If we want to find the intrinsic parameters we can use QR decomposition in order to determine the intrinsic matrix.

**To recover the intrinsic parameters of the rotation matrix**:

1. Invert the first 3x3 columns of the 3x4 rotation matrix
2. Apply the QR decomposition to $$(KR)^{-1}$$
3. The result of QR gives an orthonormal matrix Q, and the R matrix.
  - The orthonormal matrix $$ Q = R^{-1}$$ (of $$w_{R_c}$$ - the camera to world roation) and $$R = K^{-1}$$


To summarize then, in order to calibrate and find the camera matrix parameters $$K$$ and $$R$$ and $$W_{t_c}$$ 

- First find a set of corresponding points and solve the 2Nx12 matrix's null space to get M
- SVD M and use a null space equation to find $$W_{t_c}$$
- Use QR decomposition with the first 3x3 matrix inverted in order to get the $$K$$ and $$R$$ matrices.
  - $$Q = R^{-1}$$
  - $$R = K^{-1}$$


#### Real-World Calibration

Because lenses are not perfect and real cameras do not exactly follow the pinhole-camera model, there are other parameters that need to be taken into account

While we won't go in depth on those in this course we still should know how other systems work. The Caltech calibration system which is included in the OpenCV distribution is available and uses checkerboard patterns in order to properly calibrate cameras account for lens distortions. It uses nonlinear optimizations in order to solve the problem.

Typically about 30 images are needed for proper calibration.

**Lens distortions**: 

Lens distortions - radial, barrel, and pincushion distortions are all possible results of camera lenses and must be taken into account to properly calibrate a camera.

Simple distortion models:

- $$ \hat{x} = x_c (1 + \kappa_1 r_{c}^2 + \kappa_2 r_{c}^4) $$
- $$ \hat{y} = y_c (1 + \kappa_1 r_{c}^2 + \kappa_2 r_{c}^4) $$


### 3D Reconstruction

- Ideally we want to reverse the typical image formation pipeline (see notes above) where we move from pixel coordinates to world coordinates. However when dividing the image coordinates by the final components we lose the information on the depth of the image.

- Fully Calibrated Cameras

We can perform 3D reconstruction via triangulation if we have 2 images where we have the M matrix of each camera (Or the $$K$$, $$R$$, and $$t$$. However because we're looking to do 3D reconstruction we assume we don't have access to the 3D points in space. So the M matrices must not have been obtained from the the DLT method explained in the calibration section above.

In fully calibrated construction we:

- Multiply a pixel coordinate 1 by $$K_1^{-1}$$ then multiply a corresponding pixel coordinate in the other image by $$K_2^{-1}$$
  - These results represent rays
- Intersect rays to get a 3D point
  - HOWEVER, these rays may not always intersect perfectly
  - Take the midpoint of the location between the location in 3D space where the rays are closest.
  - Midpoint is the reconstructed 3D point


For stereo reconstruction of an x, y coordinate match, do the following

Given $$ K = \begin{bmatrix} \frac{f}{s_x} & s & o_x \\ 0 & \frac{f}{s_y} & o_y \\ 0 & 0 & 1 \end{bmatrix} $$

- Take $$p_l$$ and $$p_r$$
  - $$p_l = (x, y) \rightarrow ( -s_x(x - o_x), -s_y(y-o_y)) = (\hat{x'}, \hat{y'})$$
  - $$ \hat{p'}_{l/r} = \begin{bmatrix} x' \\ y' \\ f \end{bmatrix} $$
- Take the $$M_{ext} = [ R \| w_{t_c} ]$$
  - Convert this into a 4x4 matrix
  - $$ \begin{bmatrix} & c_{R_w} & & c_{t_w} \\ [0 & 0 & 0] & 1 \end{bmatrix}$$
- find $$q = \hat{p'}_l \times (l_{R_r} \hat{p'}_r) $$
- Next, solve the equation:
  - $$a \hat{p'}_l + c(q) = l_{R_r}b \hat{p'}_r + l_{T_r} $$
  - Convert into a Aq = B form
  - Invert $$A$$ to solve for a, b, and c
- After obtaining a, b, and c, find the left half of the equation, from the step above, except divide c by 2 (midpoint)
- Finally to obtain $$w_P$$ we must invert $$M_{ext}$$ from earlier and multiply by the 3D coordinate point we just obtained.

### Uncalibrated Reconstruction and Epipolar Geometry

Using epipolar geometry is a kind of "shortcut" to the full triangulation of fully calibrated cameras. It allows us to get the 3d point without having to find the 11+ parameter matrix that is required as seen in the previous method.

#### Epipolar Geometry

**Terminology**:

- **Epipolar plane**: The 2D plane formed by the 2 camera centers and the world point at which a ray intersects from each camera.
- **Epipolar line**: The intersection of the epipolar plane and the image plane.
- **Epipole**: The point on the image where the epipolar line intersects the line of the epipolar plane coming from the other camera. This point is allowed to be at infinity if the two cameras are parallel.

**Property**: All epipolar lines go through the epipole

Using a fundamental matrix to perform reconstruction is called **projective reconstructions**. Using the essential matrix is **reconstruction up to a scale factor**

We know that a $$p_r = R(p_l - t)$$. However w.r.t the camera matrices the translation $$t = o_r - o_l$$ which is just the difference of the camera centers. Thus the $$R$$ we refer to from here on is $$r_{R_l}$$

- Epipolar Constraint
  - Given the equations above we can derive
    - $$(P_l - t)^T(t\times P_l) = 0$$
    - $$ (R^T P_r)^T (t \times P_l) = 0 $$
  - $$ t \times P_l = SP $$
    - Where $$S = \begin{bmatrix} 0 & -t_z & t_y \\ t_z & 0 & -t_x \\ -t_y & t_x & 0 \end{bmatrix} $$
  - $$ P_r^T E P_l = 0 $$
  - With a couple more steps...
    - **The epipolar constraint**
    - $$im_{p_r}^T F im_{p_l} = 0 $$
- Computation of Essential Matrix
   - Given $$ P_r^T E P_l = 0 $$
   - $$E = RS$$

#### Computing F (or E), the 8 point algorithm

  - $$ F = K_{r}^{-T} E K_{l}^{-1}$$
  - Also for any set of corresponding points, the following holds:
    - $$ p_{l}^T F p_r = 0 $$
  - If we multiply this out and label the $$F$$ matrix parameters as $$f_{11}, f_{12},\dots $$ we get:
    - $$ x_l x_r f_{11} + x_l y_r f_{21} + x_l f_{31} + y_l x_r f_{12} + y_l y_r f_{22} + y_l f_{32} + x_r f_{13} + y_r f_{23} + f_{33} = 0 $$
  - If we use up to 8 different points then we can calculate the parameters for F using the null space solution to the matrix in the form $$Aq = 0$$ where A is the matrix
  - We can follow the same logic to calculate E given that the following is also true:
    - $$ p_{l}^T E p_{r} = 0 $$

#### Fundamental Matrix Properties

The fundamental matrix is of rank 2.

- $$Fp_{l}$$ corresponds to the epipolar line of $$p_l$$
- $$Fp_{r}$$ corresponds to the epipolar line of $$p_r$$

Now for every point in the image, the epipolar constraint is satisfied, even at the epipole.
- Thus $$F e_l = 0$$, and we can take the SVD(F) to find the null space of F, giving us the epipole of the left image.

#### Partial Calibration

- Get intrinsic matrices using corresponding world and image points via calibration
- Get point correspondences between images
- Estimate fundamental matrix via point correspondences
- Obtain Essential matrix from fundamental
- Get camera matrices from essential matrix
- Triangulate points


#### Finding Camera Matrices from F

Right Camera Matrix:

> $$ M_r = K_r [ I | 0 ] $$

Left Camera Matrix:

> $$ M_l = K_l [ e_x F | e ] $$

Where $$e$$ is the epipole and $$e_x$$ is the cross product of the epipole

> $$ e_x = \begin{bmatrix} 0 & -e_3 & e_2 \\ e_3 & 0 & -e_1 \\ -e_2 & e_1 & 0 \end{bmatrix} $$

#### Finding Camera Matrices from F

First define the following:

> $$ W = \begin{bmatrix} 0 & -1 & 0 \\ 1 & 0 & 0 \\ 0 & 0 & 1 \end{bmatrix} $$

> $$ Z = \begin{bmatrix} 0 & 1 & 0 \\ -1 & 0 & 0 \\ 0 & 0 & 0 \end{bmatrix} $$

Factor $$ \rightarrow E = SR $$ where $$SVD(E) = U\Sigma V^T$$:

- $$S = \pm UZU^T $$
- $$R = UWV^T \text{ OR } UW^TV^T $$


From this you must calculate the left and right matrices using all combinations of S and R, and the combinations that result in all triangulated points in front of the camera ($$+Z$$) will result in the correct reconstruction.

> $$ E = SR $$

> $$ M_r = K_r [ I | 0 ] $$

> $$ M_l = K_l [ R | t ] $$

- Where $$R$$ is the matrix from above. $$t$$ is the 3rd column of U from SVD of E or also $$t = [s_{32}, s_{13}, s_{12}] $$

### Image Rectification

Makes correspondence easier if images are parallel.

Given two parallel images which have known intrinsic parameters we can represent the essential matrix as:

> $$ E = [t_x] R = \begin{bmatrix} 0&0&0 \\ 0&0&-B \\ 0&B&0 \end{bmatrix} $$

So in other words, the goal of rectification is to find a homography, $$H$$, which causes the images to 'become' parallel

We can use disparity to help calculate $$B$$ where

> $$ p_l - p_r = \frac{Bf}{z} = \text{disparity} $$

Because of this, correpsondence is easier because corresponding points lie on the same y-value pixels.

To calculate:

```matlab
Trw = [Mextright ; 0 0 0 1];
Tlw = [Mextleft; 0 0 0 1];
Twr = inv(Trw); % can be done using transpose
Twl = inv(Tlw); % can be done using transpose
Tlr = Tlw*Twr;
% Rotation from right to left coordinate frame 
Rlr = Tlr(1:3,1:3);
% translation
tlr = Tlr(1:3,4);

% For rectification, we want the x axis of the new coordinate frame to be the vector connecting the
% optical axes, so we can write the first column as:
v1 = tlr./norm(tlr);
% The y axis should be perpendicular to the optical axis [0 0 1] and the x
% axis, so take the cross product
v2 = [-v1(2) v1(1) 0 ]';
v2 = v2./norm(v2);
% the new optical axis or z axis, must be perpendicular to the x and y axis
v3 = cross(v1,v2);
Rrectleft = [v1'; v2';v3';]'; % Now we can write the rotation matrix by interpreting the columns
% to rectify the points in the right camera, first multiply by Rrect
% so that the relative orientation of the two coordinates is the same;
% then Rlr will align the right coordinate points with the left coordinate
% frame to make parallel cameras. 
```

### Interest Point Detection

Many algorithms rely on point correspondences.

How do we find them?

A few options - most widely used are SIFT (Scale Invariant Feature Transform) and SURF

- **SIFT**
- Uses a 16x16 grid where each pixel gradient is calculated in the x and y direction, then the hitogram of gradient angles is created and the descriptor is a 4x4 patch of weighted gradient orientation histograms. 
- For a 4x4 grid with histograms of 8 bins, the descriptor vector is 128x1 (4 * 4 * 8).
  - The vector is normalized to reduce effects of contrast or gain.
- Efficiency is a large concern for SIFT as computing on every set of pixels can be time and resource consuming
- Many feature matching algorithms will use RANSAC to find the best set of matches.

### Deep Learning

Deep learning is all about gradient optimization. The overall goal is to always minimize some type of loss function.

The basic procedure is as follows:

- Given some labeled training data $$X$$, and the labels for the outputs $$Y$$
- Compute a forward pass on the data $$X$$ to obtain outputs $$Y^*$$
- Compute the difference $$Y^* - Y$$
- Update network weights to reduce loss
- Repeat

These are all usually repeated through stochastic gradient descent (SGD)

The network is really just a composition of functions for each layer represented as something like $$N(X, W) = f1(f2(f3(f4(X,  W))))$$

Weights can be calculated via an iterative process where $$ W_1 = W_0 + \alpha \frac{\partial f(W_0)}{\partial x_i} $$

To calculate SGD on networks the weights are typically a function of the previous layers' weights and outputs and the current layer's inputs

We can use the Jacobian matrix to help us calculate the SGD for our networks. The jacobian matrix paramters are defined as the following:

$$ J_{ij} = \frac{\partial f_i}{\partial x_j} $$

So in order to calculate the backpropagation of the network we

- Multiply the gradient of the input of the _layer_ by the **expected** output, or the input of the layer beforehand.

#### Classifier Accuracy  


### Computational Appearance - Reflectance, Texture, and Light Fields

**Reflectance** is the existence of a BRDF function or **B**idirectional **R**eflectance **D**istribution **F**unction. Basically this means that the angle at which light reflects will change depending on the object.

Mathematically, the function will, given an input angle and an output angle, return the amount of reflected light for the two specific angles.

Work by prof. Dana has been done to entirely measure the reflectance at all viewing angles by using a parabolic mirror.

### Motion Estimation

Given two images where there is movement (via camera, or world movement), we want to find out the function of that motion. For our purposes we assume that the motion is linear or _affine_.

That means the intensity of any given $$(x, y)$$ coordinate pair at a time $$t$$ is given as $$i(x, y, t)$$ and the same intensity should be found at $$i(x+u, y+v, t+\Delta t)$$ in the 2nd image.

These u and v are composed of linear equations such that $$ u = ax + by + c$$ and $$ v = dx + ey + f $$.

Overall this is called the brightness constancy assumption and we use it to solve for our u and v parameters

the equation for this is

> $$ i_t  + i_x (ax + by + c) + i_y (dx + ey + f) = 0 $$

If we do this for all points on the image we can use a least-squares estimate to obtain the best fit parameters for a, b, c, d, e, and f



