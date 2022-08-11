

**new word**: 

spatial envelope characters?, delineate, allocation, evenly, cascade



#### SIFT (scale-invariant feature transform)

​	**a method for extracting distinctive invariant features from images** that can be used to perform reliable matching between different views of an object or scene. The features are invariant to image scale and rotation, and are shown to provide robust matching across a a substantial range of affine distortion,change in 3D viewpoint, addition of noise, and change in illumination.

1. **Scale-space extrema detection**: The first stage of computation searches over all scales
   and image locations. It is implemented efficiently by using a difference-of-Gaussian
   function to *identify potential interest points* that are *invariant to scale and orientation*.

   by Gaussian,generates features that cover image over full range of scales and locations （*figure 1 left*）and by below equation, DOG(*figure 1 right*)
   $$
   \begin{align}
   &\text{Difference of Gaussian(DOG)}:
   \\&D(x, y, \sigma) = (G(x,y,k\sigma)-G(x,y,k\sigma))*I(x,y)
   \end{align}
   $$
   

   > ***the difference-of-Gaussian function provides a close approximation to the scale-normalized Laplacian of Gaussian***, Lindeberg showed that the normalization of the Laplacian with the factor $\sigma^2$is required for true scale invariance. In detailed experimental comparisons, *Mikolajczyk (2002) found that **the maxima and minima of $\sigma^2∇^2G$** produce the most stable image features compared to a range of other possible image functions*, such as the gradient, Hessian, or Harris corner function. The relationship between D and $\sigma^2∇^2G$ can be understood from the heat diffusion.

   $$
   \sigma^2∇^2G = \frac{\partial G}{\partial\sigma}\approx \frac{G(x, y, k\sigma)-G(x,y,\sigma)}{k\sigma-\sigma}
   \\
   \frac{D(x, y, \sigma)}{I(x,y)}=G(x, y, k\sigma)-G(x,y,\sigma)\approx(k-1)\sigma^2∇^2G
   $$

   ![image-20220806141926607](https://user-images.githubusercontent.com/66621797/184053339-7c5087df-b8ee-48fc-b54d-b2e3bca3a054.png)

   ![image-20220806142159862](https://user-images.githubusercontent.com/66621797/184053378-3ca2a3e9-0cea-4b38-aac8-87ca8c9afea3.png)

   > In order to detect the local maxima and minima of D(x, y, ), each sample point is compared to its eight neighbors in the current image and nine neighbors in the scale above and below (see Figure 2). It is selected only if it is larger than all of these neighbors or smaller than all of them. The cost of this check is reasonably low due to the fact that most sample points will be eliminated following the first few checks.

2. **Keypoint localization**: At each candidate location, a detailed model is fit to determine
   location and scale. Keypoints are selected based on measures of their stability.

   利用泰勒展开式（D（x）），并求导定位准确的极值点, 该二次型的极值为$D(\hat x)$ 
   $$
   D(x)=D + \frac{\partial D^T}{\partial x} + \frac12x^T\frac{\partial^2 D}{\partial x^2}x
   $$

   $$
   \hat x = -\frac{\partial^2 D^{-1}}{\partial x^2}\frac{\partial D}{\partial x}
   $$

   $$
   D(\hat x)=D = \frac12\frac{\partial D^T}{\partial x}\hat x
   $$

   
   ![image-20220806184624515](https://user-images.githubusercontent.com/66621797/184053408-2236e25f-8491-4867-ad55-d268a46daf16.png)



   > - **reject keypoints with low contrast**: all extrema with a value of $|D(\hat x)|$ less than 0.03 were discarded (as before, we assume image pixel values in the range [0,1])
   > - **Eliminating edge responses**:

   ![image-20220806185006406](https://user-images.githubusercontent.com/66621797/184053440-19a277a3-03ce-4599-adb7-353a3d102047.png)

3. **Orientation assignment**: One or more orientations are assigned to each keypoint location based on local image gradient directions. All future operations are performed on image data that has been transformed relative to the assigned orientation, scale, and location for each feature, t32hereby providing invariance to these transformations.
   ![image-20220806191942973](https://user-images.githubusercontent.com/66621797/184053502-9109b565-f387-4f00-bdab-0d9cee21b78e.png)



4. **Keypoint descriptor**: The local image gradients are measured at the selected scale in the region around each keypoint. These are transformed into a representation that allows for significant levels of local shape distortion and change in illumination.

   The descriptor is formed from a vector containing the values of all the orientation histogram entries, corresponding to the lengths of the arrows on the right side of Figure 7. a 2x2 array of histograms with 8 orientation bins in each, so need 2X2X8 = 32 element feature vector.

