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
  * The image below shows various Bird Eye View (BEV) vehicle shots sorted by how recognizable it is and how many point-cloud it has.
    * ![various vehicles](/img/s1_ex2_vehicles.png)
  * The image below shows vehicles varying number of point-clouds and view directions.
    * ![various vehicles2](/img/s1_ex2_vehicles2.png)
  * As you can see from the above images, steep height gradient caused by bumpers of the car are very recognizable pattern.  
  * Also, rectangular shape and oval shape seen from BEV are also a good feature for vehicles.
  
## Section 2 : Create Birds-Eye View from Lidar PCL 
  * ### Convert sensor coordinates to BEV-map coordinates (ID_S2_EX1)
    * Implemented part of `bev_from_pcl()` inside the file `student/objdet_pcl.py`.
    * Creates BEV representation from pcl data in sensor coordinates as shown below.
      * ![sensor2bev coordinate transform](/img/s2_e1_run.png)
  * ### Compute intensity layer of the BEV map (ID_S2_EX2)
    * Implemented part of `bev_from_pcl()` inside the file `student/objdet_pcl.py`.
    * Creates intensity layer from BEV transformed point cloud data as shown below.
      * ![intensity layer](/img/s2_ex2_run.png)
    * As you can see, the intensity values are in `np.uint8` format. To cut the extreme values, I've only 2% ~ 98% percentile values, and rescaled them accordingly.
      * ![intensity in u8](/img/s2_ex2_u8.png)
  * ### Compute height layer of the BEV map (ID_S2_EX3)
    * Implemented part of `bev_from_pcl()` inside the file `student/objdet_pcl.py`.
    * Creates height layer from BEV transformed point cloud data as shown below.
      * ![height layer](/img/s2_ex3_run.png)
    * As you can see, the height values are in `np.uint8` format. To cut the extreme values, I've only 2% ~ 98% percentile values, and rescaled them accordingly.
      * ![height in u8](/img/s2_ex3_u8.png)
  

## Section 3 : Model-based Object Detection in BEV Image
  * ### Add a second model from a GitHub repo (ID_S3_EX1)
    * I've included [SFA3D](https://github.com/maudzung/SFA3D) repository as submmodule by running the below code in project directory.
      * ```git submodule add https://github.com/maudzung/SFA3D.git```
    * Edited `load_configs_model()` as shown below to serve our purpose in this project.
      * ![resnet config](/img/s3_ex1_config.png)
    * If inspected with debugging tool in PyCharm as shown below, we can see that the detection model works as expected.
      * ![debug detection](/img/s3_ex1_debug.png)
  * ### Extract 3D bounding boxes from model response (ID_S3_EX2)
    * Created a function `bev2meters()` in `misc/objdet_tools.py` to transform birds-eye view pixel coordinate to meters.
    * Converted model output to expected bounding box format in the `detect_objects()` function.
    * As you can see from the image below it works as expected.
      * ![detection result](/img/s3_ex2_run.png)



## Section 4 : Performance Evaluation for Object Detection
  * ### Compute intersection-over-union between labels and detections (ID_S4_EX1)
    * Implement part of `measure_detection_performance()` in `objdet_eval.py`
    * Implement IOU calculations using `Polygon` inside the `shapely` module as shown in the image below.
      * ![iou debug](/img/s4_ex1_debug.png)
  * ### Compute false-negatives and false-positives (ID_S4_EX2)
    * 
  * ### Compute precision and recall (ID_S4_EX3)
    * 
