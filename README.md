# Street View ``Image, Pose, and 3D Cities`` Dataset 
**http://3drepresentation.stanford.edu/**

This repository shares a large dataset of street view images **(25 million images and 118 million matching image pairs) with their 6DOF camera pose, 3D models of 8 cities, and extended metadata**. The data comes in bundles of matching images; the content of the matching pairs show the same physical point while the camera viewpoint shows a large baseline (often >120 degrees). The dataset can be used for learning 6DOF camera pose estimation/visual odometry, image matching, and various 3D estimations. You can see a few sample image bundles from the dataset below and more examples [here](https://github.com/amir32002/3D_Street_View/blob/master/misc/sample_data1.pdf) and [here](https://github.com/amir32002/3D_Street_View/blob/master/misc/sample_data2.pdf). 

<img src="https://github.com/amir32002/3D_Street_View/blob/master/misc/data_samplel.gif" width="600">


The dataset was collected automatically without human annotation by developing a system to intergrate georeferenced 3D models of cities with google street view images and their geo-metadata. For more information on how the dataset was collected, please see the [[paper]](http://3drepresentation.stanford.edu/). 

<img src="https://github.com/amir32002/3D_Street_View/blob/master/misc/integrated_crawler.gif" width="600">

### 3D Models of Cities:

The dataset covers the downtown and surrounding areas of New York, Chicago, Washington, Las Vegas, Florence, Amsterdam, San Francisco and Paris. We are releasing the 3D models of these cities along with the street view images and metadata. The models are geo-registered and manually generated. Below you can see sample snapshots of 3D models. You can see more snapshots [here](https://github.com/amir32002/3D_Street_View/blob/master/3D_cities_snapshots/).

<img src="https://github.com/amir32002/3D_Street_View/blob/master/3D_cities_snapshots/3D_models_tile.jpg" width="850">

The following shows the coverage areas for 6 of the 8 cities.

<img src="https://github.com/amir32002/3D_Street_View/blob/master/misc/dataset_coverage.jpg" width="850">
 
 
### Paper:

The datase was used in the following paper to learn a universal/generic 3D representation: 

***Generic 3D Representation via Pose Estimation and Matching,***
*Amir R Zamir, Tilman Wekel, Pulkit Agrawal, Jitendra Malik, Silvio Savarese,*
*ECCV16.*

[[paper]](http://cvgl.stanford.edu/papers/zamir_eccv16.pdf)
[[supplementary material]](http://cs.stanford.edu/~amirz/index_files/0633_supp.pdf)

For more detail on the data collection and the method please visit the [[project webpage]](http://3Drepresentation.stanford.edu).

---


## Data Overview:
The dataset comprises 25 million google street view images forming 118 million corresponding pairs. We collected images on a dense grid in the aforementioned cities. Based on the 3D model of the city, we densely sampled points on facades and performed ray tracing to find all street view panoramas that see the same target point without occlusion. For each image, we know the geo location of the street view camera as well as the location of the focused target point. Since google street view provides 360 panoramas, we compute heading and pitch angles such that we can capture a 640x640 image section (of the panorama) that shows the respective target point in its center. Two images form a pair if they show the same physical target point. Each target point is typically observed by 2-7 corresponding street view images. An image is given by a 640x640 jpg along with an identically named text file that contains meta data such as the geo locations of camera and target point, the distance to the target or the pose of the camera. The image’s filename encodes unique ids for the street view location and for the target point. This allows to easily identify corresponding images. The images are compressed into multiple zip-files such that the resulting file size doesn’t exceed a maximum.

### Test Set:
To ensure the quality of the test set and keep evaluations unimpacted by the potential errors introduced by the automated data collection, every datapoint in the test set was verified by at least three Amazon Mechanical Turkers and noisy instances were removed. The procedure and statistics are elborated in the supplementary material. 
The test pairs are guaranteed to: 
* show the same exact physical point in both patches (re-verified by Turkers)
* the center of two patches are supposed to show the same physical point; the magnitude of (pixel) translation vector between the pixel location of this physical point in the two patches (re-measured by Turker click locations) is guaranteed to be <25 pixels (i.e. ~4% of image’s width). In other words, if there is a misalignment between the two patches, it's guaranteed to be <~4% of image's width, which is negligible. 
* the uncertainty in the end point location of this translation vector (quantified by the disparity among Turker click locations) is <15  pixels (~2% of image’s width). This is useful for the cases where there is uncertainty in the correspondence location, e.g. when the point is on a plain wall. 

You can download visualizations and accuracy analysis of several thousand randomly picked test datapoints [[here]](https://storage.googleapis.com/amirs/3Drep_dataset/testset_pairs_visualization_v1.zip). Below you can see a few sample test pairs where the center of two images/patches should match and three Turkers verified that by their clicks. See details in [[supplementary material]](http://cs.stanford.edu/~amirz/index_files/0633_supp.pdf).

<img src="https://github.com/amir32002/3D_Street_View/blob/master/misc/0018-0071611_0018-0071611.jpg" width="600">
<img src="https://github.com/amir32002/3D_Street_View/blob/master/misc/0057-0225543_0057-0225543.jpg" width="600">
<img src="https://github.com/amir32002/3D_Street_View/blob/master/misc/0006-0087241_0006-0087241.jpg" width="600">
<img src="https://github.com/amir32002/3D_Street_View/blob/master/misc/0057-0169146_0057-0169146.jpg" width="600">
<img src="https://github.com/amir32002/3D_Street_View/blob/master/misc/0006-0087988_0006-0087988.jpg" width="600">
 


### Noise Statistics:
A user study through Amazon Mechanical Turk was performed to analyze the characteristic of noise in the final dataset and quantify its amount. The results of the study and discussions can be found in section 3.2 of [[supplementary material]](http://cs.stanford.edu/~amirz/index_files/0633_supp.pdf). This is useful for the researchers interested in development and evaluation of methods that are capable of utilizing a large amount of automatically, and therefore nearly for-free, collected data at the expense of a modest amount of noise.


## Download
Please note that by downloading this dataset you are consenting to non-commercial use and the license.

### [ Download the full [Dataset] and [Testset] ](https://goo.gl/ERPCsa)  

The dataset is large (800GB). In order to make it more handy, it comes as multiple tar files that contain the image - and meta - data. The test set and 3D models are provided as separate folders.


---



## Dataset Structure
The street view data is given as images and corresponding text files that contain the meta data.  The data is stored in multiple tar files, where each tar file contains the image - and meta - data files of one dataset. A data set corresponds to a rectangular sub region on the map.

For each street view point - target point correspondence, there is an image and a text file with identical names. The structure of the data is as follows:

```
README.md
README.txt
\<DatasetID.tar>
  <DatasetID>_<ImageID>_<ViewID>_<TargetID>.jpg
  <DatasetID>_<ImageID>_<ViewID>_<TargetID>.txt
```

The meaning of the encoded IDs are explained in the following:

**DatasetID ``<int>``** 
The id of the dataset. A dataset corresponds to a rectangular region on the map.

**TargetID ``<int>``**
ID of a target point which is unique within a dataset. Images that have the same target id in their name, show the same physical point in their center.

**PatchID ``<int>``**
ID of the patch itself. A patch has a unique ID within one dataset.

**StreetViewID ``<int>``**
ID of the street view location. A street view location has a unique ID within one dataset. The ID is uniquely associated with a geo location.

All ids (except DatasetID) are only unique within one dataset.  The entire filename is always unique.

The image is acquired, such that the 3D target point should be projected to the center pixel (up to possible alignment errors).

The text file contains two lines (google meta data and alignment infos). The first line starts with  a **d** , the second line with an **a** . The lines contain are space delimited numbers. The meaning of the numbers is explained in the following, same order as in the file:

### Google Meta Data (d)

**DatasetID ``<int>``** 
The id of the dataset. A dataset corresponds to a rectangular region on the map.

**TargetID ``<int>``**
ID of a target point which is unique within a dataset. Images that have the same target id in their name, show the same physical point in their center.

**PatchID ``<int>``**
ID of the patch itself. A patch has a unique ID within one dataset.

**StreetViewID ``<int>``**
ID of the street view location. A street view location has a unique ID within one dataset. The ID is uniquely associated with a geo location.

**Target Point ``<3x1 double>``**
The target point is defined in a global geo-reference system (latitude, longitude, height), where latitude and longitude are given in decimal degrees and the height is given in meters.

**Target Surface Normal ``<3x1 double>``**
normal vector of the underlying geometry (e.g. facade of the building). Similar to a surface normal, the vector is normalized to one. The normal is also defined in a global coordinate system (WebMercator)

**Street View Location  ``<3x1 double>``**
Location of the respective street view panorama. Latitude and longitude are given in decimal degrees. The height is given in meters.

**Distance to Target ``<double>``**
Distance between street view location and target point in meters.

**Heading ``<double>``**
Heading of the street view camera in degrees

**Pitch ``<double>``**
Pitch of the street view camera in degrees

**Roll ``<double>``**
Roll of the street view camera in degrees (always zero)

Heading, pitch, and roll describe the absolute orientation of the (virtual) street view camera such that the target point is focused in the center of the image. The orientation is given in absolute angles.

The entire image of a given street view location is spherical. The images we work with is just a rectangular region of that sphere.

**Heading**: 0 means north, 180 means south

**Pitch**: 0 means looking parallel to the ground plane, -90 means looking to the ground, 90 looking to the sky.

### Alignment Data (a)

The images are released in two versions: raw (without content alignment) and aligned (with content alignement to reduce the issues caused by imperfection in metadata). The second line contains information about the alignment post processing. If the text file does not contain this line, no post processing has been applied. 

We applied a post processing step to compensate the inaccurate alignment of the images due to registration errors in google street view or unreported structures that cause occlusions. The algorithm estimates and applies a linear transformation to the images in order to eliminate the misalignment. Roughly speaking, for a given target point, the algorithm picks one corresponding street view as reference and tries to align all the other views such that the target point is projected to the same image location for all views. Note, that not all images could been processed.


![alignment_figure](https://github.com/amir32002/3D_Street_View/blob/master/misc/alignment.jpg)



Actually, the corrected patch center is the most important information. The other quantities are included for convenience and mostly represent intermediate results. Please refer to our paper for further details on the alignment process.

**Corrected Patch Center `` <2x1 double> ``**  
New (aligned) location of the target point in the given street view image

**Warp Matrix `` <3x3 double> ``**
Matrix that rectifies the street view image such that the target normal is orthogonal to the viewing plane

**Bounding Box Coordinates `` <4x2 double> ``**
coordinates of the virtual, unaligned bounding box in the image. The bounding box represents a region on the infinite plane in space that is defined by target point and normal. The bounding box has the same size (in meters) for one target id.

**Registration Matrix `` <3x3 double> ``**
matrix that transforms the given street view image, such that the target point is projected to the image center. (in order to align the image, we could either move the projection of the target point or transform the image)

The following quantities are heuristically used to say something about the quality and the reliability of the registration process:

**Total Sift Flow Energy `` <int> ``**
Total energy of the sift flow algorithm that registers the warped street view images. The lower the better.

**Transformation Inlier Ratio `` <double> ``**
Inlier ratio of the RANSAC algorithm that founds the best fitting projective transformation matrix for the flow field computed by SIFT flow. The higher the better.

**Similarity Measure `` <double> ``**
Standard similarity measure (ssi) of the warped and registered image and the warped reference image. The higher the better.

**Distortion `` <double> ``**
Lets assume that we define a square-shaped bounding box in the reference view and transform it by the resulting registration matrix R. B' = R * B. Distortion is the ratio of the area of B' and B. Defined to be between 0 and 1. The higher the better.

**Corrected Bounding Box `` <4x2 double> ``**
Aligned version of <bounding box coordinates 4x2 double>.


---

Slightly unclean version of our learning code: [link](https://github.com/pulkitag/egomotion)

---

### Citations
If you use this dataset please cite:

```
@inproceedings{zamir2016generic,
  title={Generic {3D} representation via pose estimation and matching},
  author={Zamir, Amir R and Wekel, Tilman and Agrawal, Pulkit and Wei, Colin and Malik, Jitendra and Savarese, Silvio},
  booktitle={European Conference on Computer Vision},
  pages={535--553},
  year={2016},
  organization={Springer}
}
```

