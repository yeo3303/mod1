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
