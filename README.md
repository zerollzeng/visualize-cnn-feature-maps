<!--
 * @Author: zerollzeng
 * @Date: 2019-09-06 20:10:21
 * @LastEditors: zerollzeng
 * @LastEditTime: 2019-09-10 10:58:32
 -->
# visualize_cnn_feature_map
visualize cnn feature maps with tiny-tensorrt

# quick start with mnist sample
Let's start with the classic MNIST example, I use the famous LeNet-5, you can visualize it by [caffe network visualization tool](https://ethereon.github.io/netscope/#/editor), and the model structure look like this.

![image](https://user-images.githubusercontent.com/38289304/64579840-dbffe500-d3b6-11e9-8882-4a9a58ca03a9.png)

you need to compile the project at the beginning.
```bash
# make sure you install cuda and tensorrt 5.x, opencv is not necessary for this sample.
# you can just comment relative line in CMakeLists.txt if you don't want to run yolov3
git clone --recursive https://github.com/zerollzeng/tensorrt-zoo
mkdir build
cd build && cmake .. && make && cd ..
```
 
and after install some site-packages and run vis.py, you're done!

```bash
pip install -r requirements.txt 
python vis.py -i 6.png --prototxt models/mnist/deploy.prototxt --caffemodel models/mnist/mnist.caffemodel --engine_file models/mnist/mnist.trt --mark_type 'convolution' 'pooling' 'innerproduct' 'softmax' --normalize_factor 1 --normalize_bias 0
```
in ./activation you will see a bunch of sub-folders

![image](https://user-images.githubusercontent.com/38289304/64580065-97287e00-d3b7-11e9-90ed-e22d96891b47.png)

and you can see each activation visualization in those sub-folders.

![image](https://user-images.githubusercontent.com/38289304/64580209-0900c780-d3b8-11e9-9189-cc7d10397cca.png)

![image](https://user-images.githubusercontent.com/38289304/64580257-351c4880-d3b8-11e9-9866-d60bbf7346d0.png)
![image](https://user-images.githubusercontent.com/38289304/64580274-49f8dc00-d3b8-11e9-8044-117059be4789.png)
![image](https://user-images.githubusercontent.com/38289304/64580329-8593a600-d3b8-11e9-90fe-0d0f4962fb4b.png)
![image](https://user-images.githubusercontent.com/38289304/64580360-9a703980-d3b8-11e9-84e7-616d82caacec.png)
![image](https://user-images.githubusercontent.com/38289304/64580384-bb388f00-d3b8-11e9-80d4-febc01ea5fa1.png)
![image](https://user-images.githubusercontent.com/38289304/64580441-e7eca680-d3b8-11e9-82b2-65eabd368704.png)
![image](https://user-images.githubusercontent.com/38289304/64580559-5893c300-d3b9-11e9-948e-1583f06ae082.png)
![image](https://user-images.githubusercontent.com/38289304/64580605-7a8d4580-d3b9-11e9-9577-73a4b4083cf4.png)
![image](https://user-images.githubusercontent.com/38289304/64580670-baecc380-d3b9-11e9-82e9-e7715a40beaa.png)


# openpose visualization
I am gonna show you how to use tiny-tensorrt to visualize a model layer activation, hope it will give you an intuitive understanding of tiny-tensorrt and convolutional neural network.

you need to prepare caffe model for openpose and install some python packages
```bash
# download model, you can also download it via browser
wget -P ./models/openpose http://posefs1.perception.cs.cmu.edu/OpenPose/models/pose/body_25/pose_iter_584000.caffemodel
wget -P ./models/openpose https://raw.githubusercontent.com/CMU-Perceptual-Computing-Lab/openpose/master/models/pose/body_25/pose_deploy.prototxt
cd activation-visualization
pip install -r requirements.txt
```

after compile and data prepare, you can run sample now
```bash
python vis.py
```

and you can see output activation images in activation folder, it contain all of the convolution layer of openpose and it should look like this.

![image](https://user-images.githubusercontent.com/38289304/64239641-299dcd00-cf33-11e9-9c75-051fa0c5c13f.png)

 for more intuitive understanding, I recommend you visualize prototxt with this online [caffe network visualization tool](https://ethereon.github.io/netscope/#/editor), you can browse the network architecture like this

 ![image](https://user-images.githubusercontent.com/38289304/64245002-a84b3800-cf3c-11e9-8fcd-cf6915e84232.png)

you can see details activation images in those sub-folders. like this feature map in first convolution layer

![0](https://user-images.githubusercontent.com/38289304/64239906-a761d880-cf33-11e9-8005-542dee105dbc.jpg)

and it's just a channel of the fisrt convolution layer, while the other looks like

![image](https://user-images.githubusercontent.com/38289304/64240061-dd06c180-cf33-11e9-9e27-49a369ad62cd.png)

if you see other activation map, you might see some activation in the middle of the model layers like Mconv3_stage0_L2_1, you know this layer get some keypoints and pose informaion.

![image](https://user-images.githubusercontent.com/38289304/64241470-64553480-cf36-11e9-9142-b3199fa8e7d9.png)

or activation near the end of the model which looks like Mconv6_stage1_L2, which output activation is very close to pose and keypoints.

![image](https://user-images.githubusercontent.com/38289304/64241656-bac27300-cf36-11e9-886e-5687136e1e73.png)

you can test with your own model, maybe need some change in vis.py because of different pre-processing. see
```bash
python vis.py --help
```