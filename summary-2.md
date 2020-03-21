# 花朵分类任务-2
## 1. 图像数据的获取和转换
### 1.1 文件
flowers/category_name/image  
文件夹地址单独设定：

```python
FLOWER_DAISY_DIR='./flowers/daisy'
FLOWER_SUNFLOWER_DIR='./flowers/sunflower'
FLOWER_TULIP_DIR='./flowers/tulip'
FLOWER_DANDI_DIR='./flowers/dandelion'
FLOWER_ROSE_DIR='./flowers/rose'
```
### 1.2 图片数据的转换
+ X=[ ]是存放图片数据的list, Z=[ ]是存放图片标签的list<br>
+ X.shape = (m, 150, 150, 3) &ensp; 使用`cv2`读取图片<br>
+ 分别读取每一类的所有图片
```python
img = cv2.imread(path,cv2.IMREAD_COLOR) # 读取图片
img = cv2.resize(img, (IMG_SIZE,IMG_SIZE)) # 统一大小 150 * 150
#img.shape = (150, 150, 3)
#X.shape = (m, 150, 150, 3) m是图片数量
#此时X的形式已经固定-tensor
X.append(np.array(img)) # 存放图片数据 
Z.append(str(label)) # 存放图片标签
``` 
此时X已是tensor形式，Z中元素为`str`形式，按**读取顺序排列**<br>
如：Z = ['daisy','daisy'...'rose','rose'...]
### 1.3 数据的编码
将数据转换为可以进行训练的格式:

```python
# Label都在Z[]中，将标签转为数字编码，再变为one-hot编码
le=LabelEncoder()
Y=le.fit_transform(Z) # 将标签变为数字，按标签的首字母排序，从0-4
Y=to_categorical(Y,5) # one-hot
# 现在X还是list，里面的元素是np.array(img)
X=np.array(X) # 将其转为np数组
X=X/255 # 归一化

# 此时X是图片数据，Y是标签数据
```
X不多赘述，Y进行了两次编码，从`str`到`数字编码`到`one-hot`<br>
所以最后才能用`数字编码`进行反变换生成`标签`
### 1.4 数据集划分
+ 此时已经得到可以用于训练的格式，但还是**顺序**的，还需要划分训练集和验证集，以及乱序<br>
+ 使用sklearn的`train_test_split()`函数，具体用法自行搜索
得到训练集和测试集，**打乱顺序，并且可以按照比例划分**
## 2.模型构建
### 2.1 数据增强
使用Keras自带的`ImageDataGenerator`函数，用法自行查看手册<br>
+ 随机旋转、随机缩放、随机水平垂直平移三种方法

### 2.2 CNN构建
Use your imagination :)
### 2.3 优化算法-LR Annealer
和学习率衰减类似，当学习停滞时，降低学习率<br>
利用Keras的Callback实现
### 2.4 结果评估
```python
_________________________________________________________________
Layer (type)                 Output Shape              Param #   
=================================================================
conv2d_1 (Conv2D)            (None, 150, 150, 32)      2432      
_________________________________________________________________
max_pooling2d_1 (MaxPooling2 (None, 75, 75, 32)        0         
_________________________________________________________________
conv2d_2 (Conv2D)            (None, 75, 75, 64)        18496     
_________________________________________________________________
max_pooling2d_2 (MaxPooling2 (None, 37, 37, 64)        0         
_________________________________________________________________
conv2d_3 (Conv2D)            (None, 37, 37, 96)        55392     
_________________________________________________________________
max_pooling2d_3 (MaxPooling2 (None, 18, 18, 96)        0         
_________________________________________________________________
conv2d_4 (Conv2D)            (None, 18, 18, 96)        83040     
_________________________________________________________________
max_pooling2d_4 (MaxPooling2 (None, 9, 9, 96)          0         
_________________________________________________________________
flatten_1 (Flatten)          (None, 7776)              0         
_________________________________________________________________
dense_1 (Dense)              (None, 512)               3981824   
_________________________________________________________________
activation_1 (Activation)    (None, 512)               0         
_________________________________________________________________
dense_2 (Dense)              (None, 5)                 2565      
=================================================================
Total params: 4,143,749
Trainable params: 4,143,749
Non-trainable params: 0
________________________________________________________________
```

```python
Epoch 40/50
25/25 [==============================] - 22s 880ms/step - loss: 0.3884 - acc: 0.8630 - val_loss: 0.7071 - val_acc: 0.7484
Epoch 41/50
25/25 [==============================] - 22s 874ms/step - loss: 0.3818 - acc: 0.8590 - val_loss: 0.6854 - val_acc: 0.7660
Epoch 42/50
25/25 [==============================] - 21s 853ms/step - loss: 0.4079 - acc: 0.8448 - val_loss: 0.7174 - val_acc: 0.7687
Epoch 43/50
25/25 [==============================] - 22s 873ms/step - loss: 0.3498 - acc: 0.8696 - val_loss: 0.7160 - val_acc: 0.7650
Epoch 44/50
25/25 [==============================] - 23s 903ms/step - loss: 0.3450 - acc: 0.8747 - val_loss: 0.7768 - val_acc: 0.7493
Epoch 45/50
25/25 [==============================] - 21s 851ms/step - loss: 0.3604 - acc: 0.8639 - val_loss: 0.6932 - val_acc: 0.7734
Epoch 46/50
25/25 [==============================] - 22s 899ms/step - loss: 0.3395 - acc: 0.8734 - val_loss: 0.7107 - val_acc: 0.7697
Epoch 47/50
25/25 [==============================] - 21s 853ms/step - loss: 0.3410 - acc: 0.8761 - val_loss: 0.7248 - val_acc: 0.7567
Epoch 48/50
25/25 [==============================] - 23s 900ms/step - loss: 0.3373 - acc: 0.8778 - val_loss: 0.6975 - val_acc: 0.7613
Epoch 49/50
25/25 [==============================] - 22s 874ms/step - loss: 0.3271 - acc: 0.8755 - val_loss: 0.7100 - val_acc: 0.7789
Epoch 50/50
25/25 [==============================] - 22s 876ms/step - loss: 0.2939 - acc: 0.8887 - val_loss: 0.7437 - val_acc: 0.7761
```
```
batch_size=128
epochs=50
```

```
model.compile(optimizer=Adam(lr=0.001),loss='categorical_crossentropy',metrics=['accuracy'])
```
+ 使用比较简单的网络结构，得到了77.61%的验证集识别率<br>
+ 分析结果可知，明显处于过拟合状态<br>
+ 使用**batch**和**Adam**，与前一个任务相比，加快了训练速度。  
虽然这个结论并不严谨，但是在参数变多的情况下，速度确实变快了。