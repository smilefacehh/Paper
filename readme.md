========
### 2018
《FutureMapping: The Computational Structure of Spatial AI Systems》，Andrew J. Davison



========
### 2020
#### 0001 《A Survey on Deep Learning for Localization and Mapping Towards the Age of Spatial Machine Intelligence》
vo、mapping、global localization、slam。
分类
    odometry estimation
        vo
            监督vo
            无监督vo
            混合vo
        vio
        io
        lo
    mapping
        几何map
            深度
            voxel
            点
            mesh
        语义map
        通用意义map
    global localization
        2d-2d
            explicit map
            implicit map
        2d-3d
            描述子匹配
            scene coordinate regression
        3d-3d
    slam
        局部优化
        全局优化
        关键帧&闭环
        不确定度估计

1.1深度学习slam的必要性
特征的鲁棒性，纹理缺失、光照变化、运动模糊。
深度学习适用于不同大小的场景，也就是模型深一点、浅一点的问题。


========
### 2021
#### 0665 《Deep Online Correction for Monocular Visual Odometry》
完全深度学习的单目视觉VO，效果在某些数据集上表现不错，但相比传统方法、混合DL+传统的方法还是差一些。本文方法是一个纯DL的VO，不需要后端优化。
introduction
VO的分类：传统、DL、混合方式。
传统方法存在的问题，当连续帧运动过小时，位姿恢复通常不太准确。混合方法在线更新大量参数，太耗费计算资源了。本文方法只更新6自由度位姿几个参数。

related
传统vo，分为直接法、非直接法。直接法最小化光度误差，非直接法最小化几何误差。挑战是纹理缺失、动态场景。
DL+传统结合方式，CNN-SVO第一个，深度估计用CNN、其他用传统方法计算。DVSO、D3VO、DFVO等。
基本上都是Depth-CNN、Pose-CNN两个结合。


#### 1076 《Visual Semantic Localization based on HD Map for Autonomous Vehicles in Urban Scenarios》
自动驾驶中精确的、鲁棒的定位非常重要。传统视觉方法难以应对光照、天气、视角、形状变化，当这些条件发生变化时，特征点通常无法匹配上。本文提出基于语义的定位，语义信息具有较好的鲁棒性，以应对这些条件变化。具体实现上，一是在使用结构一致性、全局模式一致性、时间一致性来提升匹配精度（Data Association），二是采用滑窗因子图优化框架融合数据关联、里程计观测。结果：定位误差，行进方向上0.43m，横向0.12m，偏航0.11°。

困难场景：峡谷，隧道，高架。GNSS在开放场景下，可以取得厘米级精度。但是在城市高楼林立环境中，遮挡、多路径效应，都使得GNSS结果十分不准确。GNSS+IMU可以解决部分问题，但长时间累积漂移也会导致误差过大。为了解决漂移问题，引入了先验地图，比如高精地图、特征点地图。那点云地图的存储是个大问题，特征点地图又无法解决光照、天气、视角、形状变化的问题。因此有了本文的基于语义匹配的方案。语义信息相对比较鲁棒。

related work
基于传统视觉特征的方法，一笔带过了。
基于语义特征的方法，语义特征包括：路标、交通信号灯、交通标识牌、灯柱等。

DA流程
1、采样pose，重投影
初始估计pose周围采集一堆候选pose，将map中的点投影到pose对应图像坐标系下
2、粗匹配
检测到的特征点记为perceived，重投影的特征点记为reprojection，首先特征点横向排序，按序匹配
特征点的匹配，涉及到的参数包括（物体类别，得分，包围盒），一个公式来计算相似度
对于pose的评价，该pose下所有特征点匹配误差，以及匹配成功的数量来计算
3、因子图优化
搞成一个因子图，一个复杂的优化公式
4、特征跟踪
连续帧之间的特征跟踪，与3类似，一起优化了吧
5、滑窗优化
一段时间内的同一个特征点，优化啥的

#### 2320 《Robust Localization for Planar Moving Robot in Changing Environment A Perspective on Density》
introduction
室内环境，视觉slam的挑战是光照变化、无纹理、重复相似环境、物体移动。
1、稀疏特征点在无纹理、重复相似环境下难以工作
2、深度学习带来稠密特征点匹配和建图，效率是个问题
3、稠密特征点匹配，稀疏建图，也就是不光是3d-2d，也加入2d-2d匹配，ransac剔除外点
3d-2d提供2个等式，2d-2d提供一个等式，这两对点是求解3自由度的平面运动最少需要的点数

贡献：
- 3自由度平面运动，最小数量构成的闭式解形式
- 概率解释
- 系统流程

related
深度特征代替传统手工特征，因为他们性能更好 representation。可以提取到global特征。
虽说稠密特征匹配更准确，但相应的需要建稠密的图把这些特征保存下来，下一帧来匹配。建稠密图太耗时了，存储也是个问题。
那么稠密特征+稀疏图怎么搭配，是本文解决的问题。


总结
没啥实际用处，视觉中主流的2d-2d初始化，3d-2d跟踪。他这里想在跟踪的时候，也把稠密2d-2d考虑进来，但是3d点还是稀疏的。
并且论文考虑的是3自由度平面运动，参数少，好解。稠密2d特征点没法保存到地图中，是不是只能相邻帧之间匹配。或者保存关键帧的稠密2d特征点。
