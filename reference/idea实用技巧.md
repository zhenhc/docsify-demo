# idea实用技巧

### 加载第三方jar包

![微信截图_20220117140440](images/微信截图_20220117140440.png)

### idea开启assert断言

1. 点击run->Edit Configurations
   
   ![image-20220126171646618](images/image-20220126171646618.png)

2. 如果没有VM options选项，则按照下面步骤操作添加该选项

![image-20220126171923391](images/image-20220126171923391.png)

3. 在VM options选项框里输入`-ea`,开启assert断言。