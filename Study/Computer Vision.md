# What is computer vision

**Computer vision**:
- Is an interdisplinary scientific field that deals with how computers can be made to gain high-level understanding from digital images or videos.
- From the perespective of engineering, it seeks to automate tasks that the human visual system can do.

**Computer vision task include**:
- Methods for acquiring, processing, analyzing, and understanding digital image.
- and extraction of high-dimentional data from the real world in order to produce numerical or symbolic information in the form of decision.

**Levels of vision**

- ***Low-level vision***: Image formation, acquisition, image processing
	- Image formation studies the forward process of producing images and videos.
	- Image acquisition: 
		- A digital image is produced by several image sensors.
		- Depending on the type of sensor, the resulting image data is an ordinary 2D image, a 3D volume or an image sequence
	- Image processing focuses on 2D image data processing using point operators such as contrast enhancement, filtering, noise reduction, image transforms. Image processing is considered as pre-processing that usually necessary to process the image data for CV application.
- ***Middle-level vision***: Feature, image matching
	- Feature extraction: Image features at various levels of complexity are extracted from the image data. Examples of such features: Edge. ridge, texture, shape,...
	- Image matching
	- Image segmentation
- ***High-level vision***: High-level vision is to infer the sematics, for example, object recognition and scene understanding.
	- Applications: 
		- Object recognition
		- Detection
		- Motion analysis
		- Scene reconstruction, 3D reconstruction
		- Image-based rendering ...


# Binary image

- 2 pixels values: foreground (1), background (0)
- Be used 
	- To mark regions of interest
	- As results of thresholding method

Thresholding: Given a grayscale image or an intermediate matrix -> threshold to create binary output

=> Issues
- What to do with "noisy" binary outputs ?
	- Holes
	- Extra small fragments
- How to demarcate multiple regions of interest?
	- Count objects 
	- Compute further features per objects

## Morphological operators

- Change the shape of the foreground via intersection/union operation a scanning structuring element and binary image.
- Useful to clean up result from thresholding.
- Main components
	- Structuring elements
	- Operators:
		- Basic operator: Dilation, Erosion, ...
		- Others: Opening, closing, ...


**Structuring elements**
- Masks of varying shapes and sizes used to perform morphology.
- Scan mask over the object border and transform the binary image.

**Dilation**:
- Expands connected components.
- Grow features.
- Fill holes.
- Moving S on each pixel of A:
	- If a pixel of S is onto object pixels, then the central pixel belongs to object.
	- Otherwise, set to background.
- Can be applied both on binary or grayscale image.

**Erosion**: 
- Erode connected components.
- Shrink features.
- Remove bridges, branches, noise.
- We put the element S on each pixel x of A (like convolution)
	- If all pixels of S are onto object pixels, then the central pixel belongs to object.
	- Otherwise, set to background
- Can be applied both on binary or grayscale image.

**Opening**:
- Erode then dilate
- Remove small objects, keep original shape

**Closing**:
- Dilate then erode
- Fill holes, but keep original shape


## Connected component labeling

- We loop over all the image to give a unique number for each region
- All pixels from the same region must have the same number (label)
- Objectifs:
	- Counting objects
	- Separating objects
	- Creating a mask for each object

- First loop over the image 
	- For each pixel in a region, we set 
		- The smallest label from its *top* or *left* neighbors
		- A new label
- Second loop over the image
	- For each pixel in a region, we set
		- The smallest from its own label and the labels from its *down* and *right*.

## CC labeling 

# Contrast enhancement
## Modification of image value 

- Isolated transformations (point operators/point processing) of the pixels in an image
	- Read the value of a pixel -> replace it by another
	- Example: contrast enhancement, histogram egalization
- Local tranformations
	- Read the values of few neighboring pixels -> compute a new value for a pixel
	- Convolutions, correlations
- Global transformation 
	- Read the values of all pixels in the image -> compute a new value for one pixel
	- FFT, ...


## Content

Point operators: 
- Contrast enhancement:
	- Histogram stretching
	- Histogram equalization
	- Non-linear transformations

**Image brightness**

Brightness of a grayscale image is the average intensity of all pixels in an image.
- Refers to the overall lightness or darkness of the image.

**Contrast**

- The contrast of a grayscale image indicate how easily object in the image can be distingushed.
- Many different equations for contrast exist
	- Standard deviation of intensity values of pixels in the image.
	- Difference between intensity value maximum et minimum.

![[Screenshot from 2023-02-20 11-16-46.png]]

![[Screenshot from 2023-02-20 11-17-12.png]]

**Histogram**

- If we can somehow equalize the histogram of an image -> we can make it higher contrast.

## Contrast enhancement

- Change pixel values to enhance the image contrast.
- Methods: 
	- Linear stretching of intensity range:
		- Linear stretching
		- Linear stretching with saturation
		- Piecewise linear transformation
	- Histogram equalization
	- Nonlinear transformations (log, gamma correction, ...)

## Point processing 

The output at one point depends only on that point but not other neighborhoods 
			s = T(r)
			r = f(x,y)
			s = g(x,y)
			T: Transformation function

![[Screenshot from 2023-02-20 11-27-30.png]]

![[Screenshot from 2023-02-20 11-28-14.png]]

![[Screenshot from 2023-02-20 11-34-09.png]]

### Piecewise-Linear Transformation

Contrast stretching:
- The idea behind contrast streching is to increase the dynamic range of the gray levels in the image being processed.
Output:
- Output values of input ones from (0,0) to (r1,s1) decrease.
- Output values of input ones from (r1,s1) to (r2,s2) increase.
- If r1 = s1, r2 = s2 the function becomes Homogenous transformation.
- If s1 = 0, s2 = L - 1 the function become thresholding function, output become binary image.
- If (r1,s1) = (r_min,0); (r2,s2) = (r_max,255) => Linear stretching.
- If (r1,s1) = (r_smin,0), r_smin > r_min; (r2,s2) = (r_smax,255), r_smax < r_max => Linear stretching with saturation.

## Histogram equalization - contrast enhancement

- Histogram of modified image: uniform distribution
- No parameters 
- Input: 
	- Image I which have L gray levels $r_{k}$ $\in$ $[0,L-1]$ , k= 0, 1,...,L-1. Totals pixels is n.
	- Assume that the input image is low contrast.
- Output:
	- Image J with gray level $s_k$ $\in$ $[0,L-1]$, k = 0, 1,...,L-1
	- Histogram of modified image: uniform distribution.
	- Each gray level in the image occurs with the same frequency.

**Algorithm**:
- Count the number of pixels which have gray level in image, let say $n_k$ 
- Normalized histogram p(k) $$p(k)=\frac{n_k}{n}$$
- Compute T(k) (cumulative histogram) $$T(k)=\sum_{j=0}^{k}p(j) = \frac{1}{n}\sum_{j=0}^{k}n_j$$
- Gray level $s_k$ of output image, J, responsible to gray level k of input image is calculated by: $$s_{k}= round[(L-1)T(k)]$$
- Build the output image by replacing the grey level k by $s_k$ 

**Note**: If we take the same image with different contrasts, histogram equalization will give the same result for all images

## Non-linear transformations

### Log transformations

- The general form of the log transformation: $s=c.log(1+r)$ 
with c = constant, r >= 0.
- The log transformation maps: 
	- *A narrow range* of low gray-level values in the input image into a *wider range* of output levels.
	- The opposite is true of higher values of input levels.
- Use this log to expand the values of dark pixels in an image while compressing the higher-level values.
- Expand the values of dark pixels in an image while compressing the higher-level values 

### Inverse log transformations

- This transformation maps: 
	- *A wide-range* of low gray-level values in the input image into a *more narrow range* of output levels.
	- The opposite is true of higher values of input levels.
- Use this inverse log if you want to compress the values of dark pixels in an image while expanding the higher-level values.

### Power-Law (Gamma) transformations

- The general form of power-law tranformation is: $s = c.r^\gamma$ 
	- $\gamma$ > 1: compress values in dark area, while expanding values in light area.
	- $\gamma$ < 1: expand values in dark area, while compressing values in light area.
	- r: normalized values to $[0,1]$
	- c: scaling constant to the bit size used



- Don't do histogram equalization on color image
-> Convert to other color space such as Lab, HSL/HSV
-> Apply histogram equalization on brightness/luminance channel as L or V
-> Conver 

# Convolution and spatial filtering

- Spatial filtering
	- Affects directly to pixels in image
	- Local transformation in the spatial domain can be represented as $g(x,y)=T(f(x,y))$
	- T may affect the point (x,y) and its neighboorhood (K) and output a new value
	- Same function applied at each position
	- Output and input image are typically the same size
- Convolution: Linear filtering, function is a weighted sum of pixels values $I^{'}=I*K$ 

## Spatial convolution

- Convolution: Linear filtering, function is a weigted sum, difference of pixel values $$f(n,m) = \sum_{k=-\infty}^{\infty}\sum_{l=-\infty}^{\infty} f[k,l].h[n-k,m-l]$$ 
- Convolution: New value of a pixel(i,j) is weighted sum of its neighbors

**Spatial correlation vs Convolution**

- If the mask is symmetric these two operations are identical
- Correlation: 
	- Use to find which part in image match with a certain "template"
	- *Not associative* -> if you are doing template matching
- Convolution:
	- Use for filtering image (denoise, enhancement,...)
	- Associative: allows you to "pre-convolve" the filter -> useful when you need to use multiple filter in succession: $I*h*g=I*(h*g)$

### Kernel types
- 2D spatial convolution
	- Is mostly used in image processing for feature extraction
	- And is also the core block of Convolutional Neural Networks (CNNs)
- Each kernel has its own effect and is usefull for a specific task such as
	- Blurring (noise removing)
	- Sharpening
	- Edge detection

### Smooth filtering
- "Low-pass" filters: 
	- Used for noise removing/bluring,...
	- Mean filters, Gaussian filters, ...
- Average filtering is the simplest of smooth filtering
- To avoid changing the image brightness, the sum of the coefficient must be equal to 1.

**Average (mean) filtering**

![[Screenshot from 2023-02-20 15-25-41.png]]

- Mask : all elements of the mask are equal
- Results: replacing each pixel with an average of its neighborhood, achieve smoothing effect.

**Weighted Average filtering**

- The pixel corresponding to the center of the mask is more important than the other ones.

**Gaussian filtering**

- Low-pass filter: Remove high-frequency component from the image 
	- Image becomes more smooth
	- Better than mean filter
- Convolution with itself is an other Gaussian
	- Repeat the conv. with small-width kernel => get the same result as larger-width kernel would have.
	- Convolving 2 times with Gaussian kernel of width $\sigma$ is the same as convolving once with kernel width $\sigma$$\sqrt{2}$ : $I*G_\sigma*G_\sigma=I*G_{\sigma\sqrt{2}}$ 
- Separable filter: The 2D Gaussian can be expressed as the product of 2 functions: one function of x and a function of y: $G_{\sigma}(x,y)=G_{\sigma}(x)*G_{\sigma}(y)$ 

### Sharpening filter

- Highlight Intensity Transitions
- Base on first and second order derivatives

**First and second order derivative**

- First order derivative:
	- Zero in flat region
	- Non-zero at start of step / ramp region 
	- Non-zero along ramp
	- Strong response for step changes
- Second order derivative:
	- Zero in flat region
	- Non-zero at start / end of step / ramp region
	- Zero along ramp 
	- Double response at step changes
	- Strong response for fine details and isolated points

**First Derivatives**

- Filter used to compute the first derivatives of image
	- Robert
	- Prewitt
		- less sensitive to noise
		- Smoothing with mean filter, then compute $1^{st}$ derivative.
	- Sobel
		- less sensitive to noise
		- Smoothing with Gaussian, then computing $1^{st}$ derivative.

**Second derivatives - Laplacian filtering**

- The Laplacian operator is defined as:
![[Screenshot from 2023-02-20 16-11-59.png]]

![[Screenshot from 2023-02-20 16-12-29.png]]

![[Screenshot from 2023-02-20 16-12-52.png]]

![[Screenshot from 2023-02-20 16-13-28.png]]

### Other filter

**Median filter**
- No new pixel values introduced
- Removes spikes: good for impulse, salt and pepper noise
- Non-linear filter
- Median filter is good at preserving edges

**Max/ Min filters**

- The maximum and minimum filter are shift-invariant
	- The Minimum Filter: replaces the central pixel with the darkest one in the running window.
	- The Maximum Filter: replaces the central pixel with the lightest one.


# Edge detection

- Goal: Identify sudden changes in an image
	- Intuitively, most semantic and shape information from the image can be encoded in the edges
	- More compact than pixels
=> Extract information, recognize objects, recover geometry and view

**How to find edges ?**
=> **Intensity profile** of an image is the set of intensity values taken from regularly spaced points along a line segment or multi-line path in an image.

- An edge is a place of rapid change in the image intensity function

## Image gradient
## Derivatives


# Feature Extraction - Image Matching

- A good feature will help us recognize an object in all the ways it may appear
	- Indentificable
	- Easily tracked and compared 
	- Consistent across different scales, lighting conditions, and viewing angles
	- Still visible in noisy images or when only part of an object is visible

## Local vs Global features

- Two types of features are extracted from the image: Local vs global features
- **Global feature**:
	- Describe the image as a whole to generalize the entire object
	- Include contour representations, shape descriptors, and texture features
- **Local feature:**
	- Describe the image patches (key points in the image) of an object
	- Represents the textures/color in an image patch

## Types of features 

- Global representation, shape features
- Color descriptors
- Texture features

**Pros of histogram**
- Invariant to basic geometric transformations:
	- Rotation
	- Translation
	- Scaling (Zoom)

**Cons of histogram**
- The *similarity between colors in adjacent colors* is not taken into account.
- The *spatial distribution* of pixel values is not considered: 2 different images may have the same histogram.

**Texture features**
- A texture features can be defined as:
	- a region with variation of intensity
	- as a spatial organization of pixels
- There are several methods for analyzing textures:
	- First order: statistics on histogram
	- Co-occurence matrices: searching patterns
	- Frequencial analysis: Gabor filter
- The most difficult is to find a good representation for each texture.

**First order statistics**

- Histogram-based: mean, variance, skewness, kurtosis, energy, entropy,...
- Given an image with N pixels:
	- h(i): represents the number of occurences of the i-th gray level
	- L-1: maximum gray level
	- P(i): normalized histogram
$$P(i)=\frac{h(i)}{n}$$
- Mean = image brightness $\mu=\sum_{i=0}^{L-1}(i.P(i))$

- Variance measures the deviation of gray levels from the Mean $\sigma^{2}=\sum_{i=0}^{L-1}(i-\mu)^2P(i)$

- Skewness is a measure of the degree of histogram asymmetry around the Mean $s=\frac{1}{\sigma^3}\sum_{i=0}^{L-1}(i-\mu)^3.P(i)$

- Kurtosis is a measure of the histogram sharpness $k=\frac{1}{\sigma^4}\sum_{i=0}^{L-1}(i-\mu)^4.P(i)-3$

- Entropy is a measure of randomness $H=-\sum_{i=0}^{L-1}P(i).logP(i)$

- Energy measures the smoothness $E=\sum_{i=0}^{L-1}(P(i))^2$

**GLCM (Gray Level Co-occurence Matrices)**

- Features generated from the first order statistics:
	- Provide information related to the gray-level distribution of the image
	- But do not give any information about the relative positions of the various gray levels within the image
-> Gray level co-occurences matrix that measures second-order image statistic 

- GLCM describes how frequently two pixels with gray-level $c_i$,$c_j$ appear in the image, separate by a distance d in direction $\beta$ 
	- Haralick features
- Matrix of size L x L
	- L is the number of gray level in the image (256x256)
	- We often reduce that number to 8x8, 16x16, 32x32
- Many matrices, one for each distance and direction:
	- Relative distance (in pixel numbers) d: 1,2,3,4,...
	- Relative direction $\beta$ : $0^{\circ}$, $45^{\circ}$, $90^{\circ}$,...
- Processing time can be very long

- Most important/popular parameters computed from GLCM:
- $ASM=\sum_{i}\sum_{j}CM^2(i,j)$ : Angular Seconde Momentum (ASM) minimal when all elements are equal
- $Entropy=-\sum_{i}\sum_{j}CM(i,j)log(CM(i,j))$ : A measure of chaos, maximal when all elements are equal
- $Contrast=\sum_{i}\sum_{j}(i-j)^2CM(i,j)$ : small values when big elements are near the main diagonal
- $idm=\sum_{i}\sum_{j}\frac{1}{1+(i-j)^2}CM(i,j)$ : idm (inverse differential moment) has small values when big elements are far from the main diagonal

- Haralick features: 
	- For each GLCM, we can compute up to 14 parameters characterizing the texture, of which the most important: mean, variance, energy, inertia, entropy, inverse differential moment

**Invariances**

- *All features* are *functions* of the distance d and the orientation $\beta$.
- Rotation ? -> Average on all directions.
- Scaling ? -> Multi-resolutions.

**Shape features**

- Contour-based features:
	- Chain coding, polygone approximation, geometric parameters, angular profile, surface, perimeter,...
- Region based:
	- Invariant moments,... 

## Local features

**Motivation for using local features**

- Global representations have major limitations
- Instead, describe and match only local regions
- Increased robustness to occlusions, articulation, intra-category variations.

**Applications**

- Partial search
- Object recognition/detection
- Image matching
- Panorama:
	- Detect feature point in both images
	- Find corresponding pairs
	- Use these pairs to align images

### Local feature extraction

- Local features: how to determin image patches/local regions

**Requirements**:
- Region extraction needs to be repeatable and accurate
	- Invariant to translation, rotation, scale changes.
	- Robust or covariant to out-of-plane (affine) transformations.
	- Robust to lighting variations, noise, blur quantization
- Locality: features are local, therefore robust to occlusion and clutter.
- Quantity: We need a sufficient number of regions to cover the object
- Distinctiveness: The regions should contain "interesting structure"
- Efficiency: Close to real-time performance


### Interest point detector

**Keypoint Localization**
- Goals:
	- Repeatable detection
	- Precise localization
	- Interesting content
=> Look for two-dimentional signal changes

**Finding Corners** 
- Key property:
	- In the region around a corner, image gradient has two or more dominant directions.
	- Corners are repeatable and distinctive
- Design criteria:
	- We should easily recognize the point by looking through a small window (locality)
	- Shifting the window in any direction should give a large change in intensity (good localization)

![[Screenshot from 2023-02-22 15-19-19.png]]

**Harris corner detector**

![[Screenshot from 2023-02-22 15-20-07.png]]

- Properties: 
	- Translation, rotation invariant 
	- Not in variant to image scale 
	- Partially invariant to affine intensity change


**Automatic scale selection**

- Solution: 
	- Design a function on the region, which is "scale invariant" (the same for corresponding regions, even if they are at different scales)
	- For a point in one image, we can consider it as a function of region size (patch width)
- Common approach:
	- Take a local maximum of this function. 
	- Observation: region size, for which the maximum is achieved, should be invariant to image scale.
	- Important: This scale invariant region size is found in each image independently.

**Scale invariant detection**

- A good function for scale detection has one stable sharp peak
- For usual images: a good function would be one which responds to contrast (sharp local intensity change)

**Characteristic scale**

- We define the characteristic scale as the scale that produces *peak of Laplacian response* 
- Laplacian-of-Gaussian (LoG)
	- Interest point: Local maxima in scale space of LoG
- Harris-Laplacian: 
	- Find local maximum of :
		- Harris corner detector in space (image coordinates)
		- Laplacian in scale

**Alternative approach**

- Approximate LoG with Difference-of-Gaussian (DoG)
	- Blur image with $\sigma$ Gaussian kernel (1)
	- Blur image with k$\sigma$  Gaussian kernel (2)
	- Subtract (2) from (1).
	-> Small k give a closer approximation to LoG, but usually we want to build a scale space quickly out of this k = 1.6 gives an appropriate scale space, k=$\sqrt{2}$ 

**Scale Invariant Detectors**

- Harris-Laplacian: 
	- Find local maximum of:
		- Harris corner detector in space
		- Laplacian in scale
- SIFT (D.Lowe)
	- Find local maximum of: 
		- Difference of Gaussians in space and scale

**DoG (SIFT) keypoint Detector**

- DoG at multi-octaves
- Extrema detection in scale space
- Keypoint location
	- Interpolation
	- Removing instable point
- Orientation Assignment

**DoG (SIFT) Detector**
- DoG at multi-octaves
- Scale-Space Extrema Choose all extrema within 3x3x3 neighborhood
- X is selected if it is larger or smaller thatn all 26 neighbors (its 8 neigbors in the current image and 9 neighbors each in the scale above and below)

![[Screenshot from 2023-02-24 13-54-12.png]]

- Orientation assignmet
	- Create assignment of local gradient directions at selected scale 
	- Assign orientation at peak of smoothed histogram
- Each key specifies stable 2D coordinates (x,y,scale,orientation)


### Local desciptor

- Compact, good representation for local information
- Invariances
	- Geometric transformations: rotation, translation, scaling,...
	- Camera view change
	- Illumination

**SIFT descriptor formation**

- Use the blurred image associated with the *keypoint's scale*
- Take image gradients over the keypoint neighborhood
- In order to achieve orientation invariance, the coordinates of the descriptor and the gradient orientations are rotated relative to the keypoint orientation
- Create array of orientation histogram
	- Each local orientation histogram has n orientation bins 
- The SIFT authors found that the best results were with 8 orientation bins per histogram and a 4x4 histogram array -> a SIFT descriptor: vector of 128 values

**SIFT**

- Extraordinarily robust matching technique
	- Can handle changes in viewpoint: up to about 60 degree out of plane rotation
	- Can handle significant changes in illumination
		- Sometimes even day vs night
	- Fast and efficient - can run in real time


### Feature matching

- Given a feature in $I_1$, how to find the best match in $I_2$ ?
	- Define distance function that compares two descriptors
	-> Use $L_1$, $L_2$, cosine, Mahalanobis, ... distance
	- Test all the features in $I_2$, find the one with min distance 
- How to define the diffence between two features $f_1,f_2$ ?
	- Simple approach: use only distance value d($f_1,f_2$)
		-> Can give good score to very ambiguous matches
   - Better approaches: add additional constraints
	   - Ratio of distance
	   - Spatial constraints between neigborhood pixels
	   - Fitting the transformation, then refine the matches (RANSAC)

### Image matching
- How to define the distance between 2 images
	- Using local feature: easy
	d($I_1,I_2$) = d(feature of $I_1$, feature of $I_2$)
	- Using local features: 
		- Voting strategy
		- Solving an optimization problem (time consuming)
		- Building a "global" feature from local features: BoW(bag-of-word, bag-of-features), VLAD,...


# Motion and Tracking

**Uses of motion**
- Estimating 3D structure
- Segmenting objects based on motion cues
- Video segmentation
- Recognizing events and activities
- Improving video quality (motion stabilization)

**Motion field**
- The motion field is the projection of the 3D scene motion into the image

**Optical flow**
- Definition: optical flow is the apparent motion of brightness patterns in the image
- Ideally, optical flow would be the same as the motion field
- Note: apparent motion can be caused by lighting change without any actual action

GOAL: Recover image motion at each pixel from optical flow

## Motion estimation techniques

- Direct methods
	- Directly recover image motion at each pixel from spatio-temporal image brightness variations
	- Dense motion fields, but sensitive to apperance variations
	- Suitable for video and when image motion is small
- Feature-based methods
	- Extract visual features (corners, textured areas) and track them over multiple frames.
	- Sparse motion fields, but more robust tracking.
	- Suitable when image motion is large.



# Object recognition

**Challenge**:
- Variable viewpoint
- Variable illumination
- Scale
- Deformation
- Occlusion
- Background clutter
- Intra-class variation

**Data driven approach**

- Collect a database of image with labels
- Use ML to train an image classifier
- Evaluate the classifier on test images

## Bag of words

- Local feature ~~ a word 
- An image ~~ a document
- Apply a technique for textual document representation vector model
-> Represent a data item as a histogram over features

**Dictionary Learning**: Learn Visual Words using clustering
1. Extract features from image (SIFT)
2. Learn visual dictionary (K-means clustering)

**Encode**: Build Bag-of-Words (BOW) vectors for each image
1. Quantization: Image features gets associated to a visual word (nearst cluster center)
2. Histogram: count the number of visual word occurences

### TF-IDF 
- Term Frequency Inverse Document Frequency
- $v_{d}=[n(w_{1,d},n(w_{2,d}),...,n(w_{T,d}))]$ weight each word by a heuristic $v_{d}=[n(w_{1,d}\alpha_1,n(w_{2,d})\alpha_2,...,n(w_{T,d})\alpha_T)]$ 
$$n(w_{i,d})\alpha_i=n(w_{i,d}log(\frac{D}{\sum_{d'}1[w_{i}\in d']}))$$
### Scalability: Alignment to large database
- What if we need to align a test image with thousands or millions of image in a model database?
	- Efficient putative match generation 
		- Fast nearest neighbor search, inverted indexes

- Vocabulary tree
	- Multiple rounds of K-Means to compute decision tree (offline)
	- Fill and query tree online

**Classify**: Train and test data using BOWs
- K-nearest neighbors
	- Non-parametric pattern classification approach
	- Consider a two class problem where each sample consists of two measurements (x,y).
- For a given query point q, assign the class of nearest neighbor.
- Compute the k nearest neighbors and assign the class by majority vote.

- The best distance metric between data points? 
	- Typically Euclidean distance
	- Locality sensitive distance metrics
	- Important to normalize. Dimensions have different scales
- Number of K?
	- Typically k = 1 is good 
	- Cross-validation 
- k-nearest neighbor
	- Finde the k closest points from training data
	- Labels of the k points "vote" to classify

**Note**:
- Trying out what hyperparameters work best on the test set is very bad idea. The test set is a proxy for the generalization performance. Use only very sparingly at the end.

-> How to pick hyperparameters?

- Methodlogy
	- Train and test
	- Train, validate and test
- Train for original model
- Validate to find hyperparameters
- Test to understand generalizability

**Pros**
- Simple yet effective
**Cons**
- Search is expensive (can be speed-up)
- Storage requirement
- Difficulties with high-dimensional data

**Complexity and storage**
- N Training images, M test images
- Training: O(1)
- Testing: O(MN)

-> Normally need the opposite
-> Slow training, fast testing


# Object detection

- **Problem**: Detecting and localizing generic objects from various categories, such as cars, people
- Challenge: 
	- Illumination
	- Viewpoint
	- Deformation
	- Intra-class variablity

**Basic Pipeline**
- Build/Train object model
	- Choose a representation
	- Learn or fit parameter of model/classifier
- Generate candinates in new image
- Score the candinate

**Window-based models: Window size**
## Boosting classifiers
### Training
- Initially, weight each training example equally
- In each boosting round:
	- Find the *weak learner* that achieves the lowest weighted training error
	- *Raise weights of training examples misclassified* by current weak learner
- Compute final classifier as linear combination of all weak learners
- Extract formulas for re-weighting and combining weak learners *depend on the particular boosting scheme* 

### Viola-Jones face detector

Main idea:
- Represent *local texture with efficiently computable "rectangular" features* within window of interest
- *Select discriminative features* to be weak classifiers
- Use *boosted combination* of them as final classifier
- Form a *cascade of such classifier*, rejecting clear negatives quickly

**Features**:
- Rectangle filter: feature output is difference between adjacent regions
- Efficiently computable with integral image: any sum can be computed in constant time 
- Avoid scaling images -> scale features directly for same cost
- Use AdaBoost both to select the informative features and to form the classifier

**AdaBoost**

**Cascading classifiers for detection**
- Form a cascade with low fasle negative rates early on 
- Apply less accurate but faster classifier first *to immediately discard* windows that *clearly appear to be negative*

**Training the cascade**
- Set *target detection* and *false positive rates* for each stage
- Keep *adding features* to the current stage until its target rates have been met
	- Need to *lower AdaBoost threshold to maximize detection*
	- Test on a validation set
- If the *overall fasle positive rate is not low enough*, then *add another stage*
- *Use false positives* from current stage as *the negative training examples for the next stage* 

**Summary**
- A seminal approach to real-time object detection
- Training is slow, but detection is very fast
- Key ideas 
	- Integral images for fast feature evaluation
	- Boosting for feature selection
	- Attentional cascade of classifiers for fast rejection of non-face windows

### Boosting: Pros and cons

- Advantages of boosting
	- Integrates classification with feature selection
	- Complexity of training is linear in the number of training examples
	- Flexibility of training is linear in the number of training examples
	- Testing is fast
	- Easy to implement
- Disadvantages 
	- Needs many training examples
	- Other discriminative models may out perform in practice (SVMs, CNNs, ...)
		- Especially for many-calss problems


# Deeplearning for CV
