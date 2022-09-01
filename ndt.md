![image-20220901160941469](ndt.assets/image-20220901160941469.png)

### Nonparametric Dynamic Thresholding

非参数动态阈值

![image-20220901162157485](ndt.assets/image-20220901162157485.png)

![image-20220901162215009](ndt.assets/image-20220901162215009.png)

![image-20220901164524205](ndt.assets/image-20220901164524205.png)

![image-20220901164638423](ndt.assets/image-20220901164638423.png)

![image-20220901164706407](ndt.assets/image-20220901164706407.png)

![image-20220901164715273](ndt.assets/image-20220901164715273.png)

![image-20220901164725583](ndt.assets/image-20220901164725583.png)

![image-20220901164734018](ndt.assets/image-20220901164734018.png)

![image-20220901165229571](ndt.assets/image-20220901165229571.png)

![image-20220901165352975](ndt.assets/image-20220901165352975.png)

![image-20220901165643090](ndt.assets/image-20220901165643090.png)

![image-20220901165650755](ndt.assets/image-20220901165650755.png)

![image-20220901165757678](ndt.assets/image-20220901165757678.png)

![image-20220901165804272](ndt.assets/image-20220901165804272.png)

- 第一步：用LSTM学习时序数据做预测
    1. 单通道模型
    2. 预测通道的值
- 第二步：收集每一步误差构成误差向量
- 第三步：对误差作加权平均的平滑处理
- 第四步：根据平滑后的数据计算阈值
- 第五步：高于阈值标为样本
- 为降低误报率，找出假阳性数据，提出“修剪策略”
- 实验参数设置及实验结果分析