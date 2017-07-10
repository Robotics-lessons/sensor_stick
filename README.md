# Readme file for sensor_stick exercise

Ultimately in this exercise, your goal is to write a ROS node that takes in the camera data as a point cloud, filters that point cloud, then segments the individual objects using Euclidean clustering. In this step, you'll begin writing the node by adding the code to publish your point cloud data as a message on a topic called /sensor_stick/point_cloud.

In the sensor_stick/scripts/ folder you'll find a file called template.py that you can use as starting point for your ROS node. The starter script looks like this:
```
#!/usr/bin/env python

# Import modules
from pcl_helper import *

# TODO: Define functions as required

# Callback function for your Point Cloud Subscriber
def pcl_callback(pcl_msg):

    # TODO: Initialization

    # TODO: Convert ROS msg to PCL data

    # TODO: Voxel Grid Downsampling

    # TODO: PassThrough Filter


    # TODO: RANSAC Plane Segmentation

    # TODO: Extract inliers and outliers

    # TODO: Euclidean Clustering

    # TODO: Create Cluster-Mask Point Cloud to visualize each cluster separately

    # TODO: Convert PCL data to ROS messages

    # TODO: Publish ROS messages


if __name__ == '__main__':

    # TODO: ROS node initialization

    # TODO: Create Subscribers

    # TODO: Create Publishers

    # Initialize color_list
    get_color_list.color_list = []

    # TODO: Spin while node is not shutdown

```

The TODO's are the pieces you'll add following along with this lesson. The first step is to import a bunch of functions from pcl_helper.py. These functions will help you by providing some crucial methods to operate on your point clouds. Check out pcl_helper.py to see what each of these functions does.
Here are the steps you need to take to get your basic ROS node working. First, make a copy of template.py. You can call it whatever you like, but I'll call my copy segmentation.py. Next, make the following changes to the script:

Initialize your ROS node. In this step you are initializing a new node called "clustering".

```
# TODO: ROS node initialization

rospy.init_node('clustering', anonymous=True)
```

Create Subscribers. Here you're creating a subscriber to receive the published data coming out of the pcl_callback() function where you'll be processing your point clouds.

```
# TODO: Create Subscribers

pcl_sub = rospy.Subscriber("/sensor_stick/point_cloud", pc2.PointCloud2, pcl_callback, queue_size=1)
```

Create Publishers. Here you're creating two new publishers to publish the point cloud data from the table and the objects on the table to topics called pcl_table and pcl_objects, respectively.

```
# TODO: Create Publishers

pcl_objects_pub = rospy.Publisher("/pcl_objects", PointCloud2, queue_size=1)
pcl_table_pub = rospy.Publisher("/pcl_table", PointCloud2, queue_size=1)
```

Spin while node is not shutdown. Here you're "blocking" or yielding control to other processes while your node is not active.

```
# TODO: Spin while node is not shutdown
 
while not rospy.is_shutdown():
 rospy.spin()
```

Publish ROS messages from your pcl_callback(). For now you'll just be publishing the original pointcloud itself, but later you'll change these to be the point clouds associated with the objects and the table.

```
# TODO: Publish ROS msg
 
pcl_objects_pub.publish(pcl_msg)
pcl_table_pub.publish(pcl_msg)
```

Once you've made these modifications, it's time to run your node and see if it works! If you still have the environment up in Gazebo and RViz, all you need to do is run your modified script (remember I called mine segmentation.py) like this within the /scripts/ folder:
```
$ ./segmentation.py
```

If everything worked you should see your new topics pcl_objects and pcl_table being published in RViz. To see all published topics click the pulldown menu to the right of the "Topic" field:

If you see these new topics begin published in RViz, you're ready to move on to implementing the filtering steps you performed in the Tabletop Segmentation Exercise in the last lesson!
