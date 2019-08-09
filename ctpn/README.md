# Text-detection-ctpn

text detection mainly based on ctpn (connectionist text proposal network). It is implemented in tensorflow. I use id card detect as an example to demonstrate the results, but it should be noticing that this model can be used in almost every horizontal scene text detection task. The origin paper can be found [here](https://arxiv.org/abs/1609.03605). Also, the origin repo in caffe can be found in [here](https://github.com/tianzhi0549/CTPN). For more detail about the paper and code, see this [blog](http://slade-ruan.me/2017/10/22/text-detection-ctpn/)
***
# setup
- requirements: tensorflow1.3, cython0.24, opencv-python, easydict,(recommend to install Anaconda)
- if you do not have a gpu device,follow here to [setup](https://github.com/eragonruan/text-detection-ctpn/issues/43)
- if you have a gpu device, build the library by
```shell
cd lib/utils
chmod +x make.sh
./make.sh
```
***
# parameters
there are some parameters you may need to modify according to your requirement, you can find them in ctpn/text.yml
- USE_GPU_NMS # whether to use nms implemented in cuda or not
- DETECT_MODE # H represents horizontal mode, O represents oriented mode, default is H
- checkpoints_path # the model I provided is in checkpoints/, if you train the model by yourself,it will be saved in output/
***
# demo
- put your images in data/demo, the results will be saved in data/results, and run demo in the root 
```shell
python ./ctpn/demo.py
```
***
# training
## prepare data
- First, download the pre-trained model of VGG net and put it in data/pretrain/VGG_imagenet.npy. you can download it from [google drive](https://drive.google.com/open?id=0B_WmJoEtfQhDRl82b1dJTjB2ZGc) or [baidu yun](https://pan.baidu.com/s/1kUNTl1l). 
- Second, prepare the training data as referred in paper, or you can download the data I prepared from previous link. Or you can prepare your own data according to the following steps. 
- Modify the path (/home/ubuntu/ajinkya/chinese_ocr/ctpn/data/pretrain_model/VOCdevkit/VOC2007/JPEGImages) and gt_path ("/home/ubuntu/ajinkya/chinese_ocr/ctpn/data/pretrain_model/VOCdevkit/VOC2007/Annotations/gt_img_1001.txt") , heres how gt_img_1001.txt should look (referenced from: https://github.com/eragonruan/text-detection-ctpn/blob/banjin-dev/data/readme/gt_img_859.txt)
```
234,162,234,183,307,162,307,183,english
318,159,318,183,374,159,374,183,english
442,165,442,184,487,165,487,184,english
499,166,499,185,559,166,559,185,english
235,187,235,204,280,187,280,204,english
281,186,281,206,300,186,300,206,english
302,187,302,206,372,187,372,206,english
443,190,443,207,485,190,485,207,english
488,191,488,208,508,191,508,208,english
512,190,512,210,556,190,556,210,english
249,218,249,233,272,218,272,233,english
276,220,276,232,331,220,331,232,english
335,220,335,233,374,220,374,233,english
377,222,377,235,435,222,435,235,english
252,284,252,299,311,284,311,299,english
251,324,251,337,327,324,327,337,english
251,298,251,308,296,298,296,308,english
300,299,300,309,333,299,333,309,english
335,297,335,308,368,297,368,308,english

```

in prepare_training_data/split_label.py according to your dataset. And run
```shell
cd prepare_training_data
python split_label.py
```
- it will generate the prepared data in current folder, and then run
```shell
python ToVoc.py
```
- to convert the prepared training data into voc format. It will generate a folder named TEXTVOC. move this folder to data/ and then run
```shell
cd ../data
ln -s TEXTVOC VOCdevkit2007
```
## train 
Simplely run
```shell
python ./ctpn/train_net.py
```
- you can modify some hyper parameters in ctpn/text.yml, or just used the parameters I set.
- The model I provided in checkpoints is trained on GTX1070 for 50k iters.
- If you are using cuda nms, it takes about 0.2s per iter. So it will takes about 2.5 hours to finished 50k iterations.
***
# roadmap
- [x] cython nms
- [x] cuda nms
- [x] python2/python3 compatblity
- [x] tensorflow1.3
- [x] delete useless code
- [x] loss function as referred in paper
- [x] oriented text connector
- [x] BLSTM
- [ ] side refinement
***
# some results
`NOTICE:` all the photos used below are collected from the internet. If it affects you, please contact me to delete them.
<img src="data/oriented_results/001.jpg" width=320 height=240 /><img src="data/oriented_results/002.jpg" width=320 height=240 />
<img src="data/oriented_results/003.jpg" width=320 height=240 /><img src="data/oriented_results/004.jpg" width=320 height=240 />
<img src="data/oriented_results/009.jpg" width=320 height=480 /><img src="data/oriented_results/010.png" width=320 height=320 />
***
## oriented text connector
- oriented text connector has been implemented, i's working, but still need futher improvement.
- left figure is the result for DETECT_MODE H, right figure for DETECT_MODE O
<img src="data/oriented_results/007.jpg" width=320 height=240 /><img src="data/oriented_results/007.jpg" width=320 height=240 />
<img src="data/oriented_results/008.jpg" width=320 height=480 /><img src="data/oriented_results/008.jpg" width=320 height=480 />
***
