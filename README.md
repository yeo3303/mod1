修改两处一个是世界文件worlds里的competition_task_random.sdf，另一个是models里面的iris_with_standoffs。
本次舍弃了iris_with_gimbal。我们用iris_with_ardupilot作为模型，而因为其实体是iris_with_standoffs，所以我们需要改这个。
世界文件主要是改无人机引用的模型，我的大概是124行附近。
```
   <include>
        <uri>model://iris_with_ardupilot</uri>
        <pose degrees="true">0 0 0.195 0 0 90</pose>
   </include>

```
iris_with_standoffs是加了一个相机模型，从
```
 <link name="downward_cam">
```
后开始添加至</joint>。这样我在我电脑上测试了应该可以
#### 镭射
在iris_with_ardupilot的model.sdf里最后一个<model>（文件末尾）前加上下方代码，因为我自己的model文件有点问题所以先不上传了。使用方法附在代码下面。
```
 <link name="rangefinder_link">
      <gravity>0</gravity>
      <pose>0 0 -0.18 0 0 0</pose>
      <inertial>
        <pose>0 0 0 0 0 0</pose>
        <mass>0.001</mass>
        <inertia>
          <ixx>0.000001</ixx>
          <ixy>0</ixy>
          <ixz>0</ixz>
          <iyy>0.000001</iyy>
          <iyz>0</iyz>
          <izz>0.000001</izz>
        </inertia>
      </inertial>
      <sensor name="rangefinder_sensor" type="gpu_lidar">
        <pose degrees="true">0 0 0 0 90 0</pose>
        <always_on>1</always_on>
        <update_rate>30</update_rate>
        <ray>
          <scan>
            <horizontal>
              <samples>1</samples>
              <resolution>1</resolution>
              <min_angle>0</min_angle>
              <max_angle>0</max_angle>
            </horizontal>
            <vertical>
              <samples>1</samples>
              <resolution>1</resolution>
              <min_angle>0</min_angle>
              <max_angle>0</max_angle>
            </vertical>
          </scan>
          <range>
            <min>0.05</min>
            <max>50</max>
            <resolution>0.01</resolution>
          </range>
        </ray>
        <visualize>true</visualize>
      </sensor>
    </link>
    <joint name="rangefinder_joint" type="fixed">
      <parent>iris_with_standoffs::base_link</parent>
      <child>rangefinder_link</child>
    </joint>
```
使用方法
```
1.确认传感器话题出现
gz topic -l | grep rangefinder
```
```
2. 读取测距数据
gz topic -e -t /world/competition_task_random/model/iris_with_ardupilot/link/rangefinder_link/sensor/rangefinder_sensor/scan
```
注：我用的gazebo版本是Gazebo Sim 8.14 (Harmonic)，如果传统的gazebo命令会有不同。
