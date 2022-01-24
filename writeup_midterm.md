# Writeup: 3D-Objects (project 2)

This repository is for the Udacity Self Driving Car (SDC) nanodegree project 2 and 3.  
In project 2 we find 3D bounding boxes from a single point cloud data. In project 3, we further develop to go beyond detecting objects and to tracking 3D objects in time.  
This writeup shows how I followed the instructions for the SDC course and implemented the first part, 3D object detection.  
** Please do not copy my project just to pass SDC. It is considered cheating and will violate Udacity's code of honor. If you like the materials in this repository consider taking SDC nanodegree. **

## Section 1 : Compute Lidar Point-Cloud from Range Image
* ### Visualize range image channels (ID_S1_EX1)
  * Implemented function `show_range_image()` inside the file `student/objdet_pcl.py`.
  * Extracted lidar data and range image for the roof-mounted lidar as in course example C1-5-1.
  * range and intensity data is rescaled and represented in `np.uint8` format.
  * `epsilon=0.001` is added in the above rescaling process to prevent division by zero.  
  ![range image](/img/s1_ex1_range_img.png)
* ### Visualize lidar point-cloud (ID_S1_EX2)
  * Implemented `show_pcl()` inside the file `student/objdet_pcl.py`.
  * Creates visualization of point-cloud data in open3d window as in the image below.
    * ![pcl visualization](/img/s1_ex2_run.png)
  * The image below shows various vehicles sorted by how recognizable it is and how many point-cloud it has.
    * ![various vehicles](/img/s1_ex2_vehicles.png)
  * The image below shows vehicles varying number of point-clouds and view directions.
    * ![various vehicles2](/img/s1_ex2_vehicles2.png)
  
## Section 2 : Create Birds-Eye View from Lidar PCL 


## Section 3 : Model-based Object Detection in BEV Image


## Section 4 : Performance Evaluation for Object Detection

