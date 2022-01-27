# Writeup: Track 3D-Objects Over Time

## Step 1. Implement EKF to track a single target with lidar
  * ### implement `predict()` in `student/filter.py`
    * `F()` and `Q()` in n dimension linear motion are defined as in the equation below. (I_n is the identity matrix in n dimension)
      * ![f](/img2/fqn.png) 
    * The prediction function use these F, Q values as shown in the equation below.
      * ![prediction](/img2/prediction_step.png)
    * We get the P value for prediction from `track.P` 
    * `predict()` function updates values of _x_ and _P_ by using `track.set_x()`, and `track.set_P()` method respectively.
  * ### implement `update()` in `student/filter.py`
    * `gamma()` and `S()` as well as whole update step is defined as in equation below.
      * ![update](/img2/update_step.png)
    * `gamma()` function takes in the measurement object `meas` and uses it's specific _h_ matrix by `meas.sensor.get_hx(x)`
      * This is because in EKF _h_ is used instead of _H_ and this matrix is different for camera and lidar.
    * `S()` function use _H_ matrix as it is.
    * `update()` function updates values of _x_ and _P_ by using `track.set_x()`, and `track.set_P()` method respectively. 
  * ### Tracking Result
    * The tracking results are shown as in image below with mean RMSE is 0.35. 
    * ![step1 resut](/img2/step1.png)
    

## Step 2. Track Mangement
  * ### Initialize Tracks in the class `Track` inside `student/trackmanagement.py`
    * Transformed the sensor coordinate to vehicle coordinate.
      * ![track initialization](/img2/initialize_track.png)
  * ### Track score
    * I've modified how the track score is calculated.
      * The score is now how many times it was associated with measurement for past _w_ measurements. (_w_: window size)
  * ### Track state update
    * I've implemented `Trackmanagement.handle_updated_track()` method to handle updated tracks.
      * ![handle updated track](/img2/handle_updated_track.png)
  * ### Track Management Result
    * There is a single track initialized and deleted as shown in the image below.
      * ![track delete](/img2/s2_track_delete.png)
      * In the above image there is only a single track without track losses in between, which is nice.
      * Also here is the tracking RMSE results shown below
      * ![track result](/img2/s2_rmse.png)
      * The vehicle is tracked well when it was approaching the vehicle, but once it starts go past the vehicle the tracking error goes up.
      
## Step 3. Association
  * ### Nearest neighbor data association
    * The association matrix is initialized in `Associate.associate()` method as below.
      * ![association matrix](/img2/nnmatrix.png)
    * With the above matrix, the nearest neighbor association is done inside `Associate.get_closest_track_and_meas()` method.
      * ![associate](/img2/associate.png)
    * For the distance we use the Mahalanobis Distance (MHD)
      * ![MHD](/img2/MHD.png)
    * For improving execution time, I've implemented gating using chi-square-distribution method.
    
  * ### Association Result
    * As shown in the image below, many measurement (nearly 20) gets initialized, but only few of them gets to be confirmed. Thus it is dealing with ghosts well.
      * ![Association result](/img2/associated_tracks.png)

## Step 4. Measurements
  * ### Camera measurements
    * Implemented `in_fov()` method to check if object should be in the field of view (fov).
    * Implemented `get_hx()` method to use in EKF algorithm 
      * To stop division by zero, a small number `0.001` is used to insure it is bigger than 0.
      * Also for the same reason an error is raised if x is zero
      * ![hx](/img2/hx.png)
    * updated the code for `generate_measurement()` and in the `Measurement.__init__()` to include camera measurements.
  * ### Camera measuremente results
    * The final result after including camera measurement is shown below. 
      * ![final result MSE](/img2/s4_final_result.png)
      * [final result video google-drive link](https://drive.google.com/file/d/1PAz0LBa2Vj3PBkFIu3s4HFQRIkVRe2CB/view?usp=sharing)
      * It deals much better with ghost tracks than the association result from step 3.
      
## Conclusion
Implementing the EKF and making it work with camera sensor was most challenging for me as it involves a lot of math. But as it is seen by step 3 and step 4, the result was worth it.  
with only lidar measurements, there was a lot of ghost tracks, but with camera measurement, it has been reduced.  
But in a real world scenarios, self driving cars have only a split second to react to dangerous scenarios. It might not have enough time to wait for tentative track to become confirmed track.

## Future work
It will be great to include 3-D object detection model's confident score be initial track score as it will help the tracks to confirm important tracks faster.
Also It would be nice to use deep learning based vehicle trajectory prediction algorithm such as one shown in [here](https://arxiv.org/abs/1805.06771).
