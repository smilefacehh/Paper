2320
现有的2d range-based定位，都只用到了有效激光点数据，不管是icp、还是基于概率地图的匹配方式。但其实没有激光点的位置也携带了信息是不是！认为这个点是空的。
那么新的匹配方式可以是，按顺序编码成一维向量，同样的在map中匹配点位置模拟提取一圈scan，编码成一维向量。两个一维向量匹配。
《Global Localization with a Single-line LiDAR by Dense 2D Signature and 1D Registration》

0665
vo的挑战：纹理缺失、动态场景。