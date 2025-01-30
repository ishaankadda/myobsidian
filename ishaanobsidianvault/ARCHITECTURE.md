Creating a new model head or backbone is simple. Let us demonstrate how to create a new `yolov7-tiny` model and add it to **`./models/architectures`**.

## Step 1: Create model directory
1. create **`./models/architectures/yolov7_tiny`** and create empty `local.py`, `head.py`, `backbone.py`, and `__init__.py` files inside it. 
2. Add `from . import yolov7_tiny` to **`./models/architectures/__init__.py`** and include the imported variable in `__all__`.

## Step 2: model definitions

### `./models/architectures/yolov7_tiny/__init__.py`
```
from .backbone import Backbone
from .head import Head
```

### `./models/architectures/yolov7_tiny/local.py`
Helps to define and import all the component modules here.
```
import torch
from torch import nn

from ...common import Conv, MP, SP, Concat, IDetect

class Elan_block_tiny_yolo(nn.Module):
    def __init__(self, in_channel, elan_channel, out_channel):
        
        super().__init__()
        self.in_channel = in_channel
        self.elan_channel = elan_channel
        self.out_channel = out_channel
        
        self.econv1 = Conv(self.in_channel, self.elan_channel, 1,1)
        self.econv2 = Conv(self.in_channel, self.elan_channel, 1,1)
        self.econv3 = Conv(self.elan_channel, self.elan_channel, 3,1)
        self.econv4 = Conv(self.elan_channel, self.elan_channel, 3,1)
        self.econv5 = Conv(4*self.elan_channel, self.out_channel, 1,1)
        
    def forward(self, x):
        
        o1 = self.econv1(x)
        o2 = self.econv2(x)
        o3 = self.econv3(o2)
        o4 = self.econv4(o3)
        
        concat = torch.cat([o1,o2,o3,o4], 1)
        out = self.econv5(concat)
        
        return out
```

### `./models/architectures/yolov7_tiny/backbone.py`
Define a `Backbone` module. `forward(x)` returns a list of intermediate and final layer activations.
```
import torch
from torch import nn

from .local import Conv, Elan_block_tiny_yolo, MP, SP, Concat, IDetect
        
class Backbone(nn.Module):
    def __init__(self, cfg_file, ckpt:str=None):
        super().__init__()
        self.act = nn.LeakyReLU(0.1)
        self.n_classes = cfg_file['nc']
        self.anchors = cfg_file['anchors']
        #
        self.conv1 = Conv(3, 32,3,2)
        self.conv2 = Conv(32,64,3,2)
        #
        self.elan1 = Elan_block_tiny_yolo(64,32,64)  #x7
        self.maxpool1 = MP()
        #
        self.elan2 = Elan_block_tiny_yolo(64,64,128)  #x14
        self.maxpool2 = MP()
        #
        self.elan3 = Elan_block_tiny_yolo(128,128,256)  #x21
        self.maxpool3 = MP()
        #
        self.elan4 = Elan_block_tiny_yolo(256,256,512)  #x28
    
    def forward(self, x):
        x = self.conv1(x)
        x = self.conv2(x)
        #
        x = self.elan1(x)
        x7 = x
        x = self.maxpool1(x)
        #
        x = self.elan2(x)
        x14 = x
        x = self.maxpool2(x)
        #
        x = self.elan3(x)
        x21 = x
        x = self.maxpool3(x)
        #
        x = self.elan4(x)
        x28 = x
        return [x14, x21 ,x28]
    
    def load_checkpoint(self):
        pass
```

### `./models/architectures/yolov7_tiny/head.py` 
Define a `Head` module. `forward(x)`  takes a list of layer activations from a compatible `Backbone` module.
```
import torch
from torch import nn

from .local import Conv, Elan_block_tiny_yolo, MP, SP, Concat, IDetect
        
class Head(nn.Module):
    def __init__(self, cfg_file, ckpt:str=None):
        super().__init__()
        self.act = nn.LeakyReLU(0.1)
        self.n_classes = cfg_file['nc']
        self.anchors = cfg_file['anchors']
        #
        self.conv7 = Conv(512,256,1,1)
        self.conv8 = Conv(512,256,1,1)
        self.sp_pool1 = SP(5)
        self.sp_pool2 = SP(9)
        self.sp_pool3 = SP(13)
        #
        self.concat1 = Concat() #concat outputs of conv8, sp_pool1:3
        self.conv9 = Conv(256*4,256,1,1)
        #
        self.concat2 = Concat() #concat outputs of conv7, conv9
        self.conv10 = Conv(256*2,256,1,1) #x37
        #
        self.conv11 = Conv(256,128,1,1)
        self.upsample1 = nn.Upsample(scale_factor = 2, mode = 'nearest')
        #
        self.conv12 = Conv(256,128,1,1) #input is x21
        self.concat3 = Concat() #concat output of conv12, upsample1
        self.elan5 = Elan_block_tiny_yolo(128*2,64,128) #x47
        #
        self.conv13 = Conv(128,64,1,1)
        self.upsample2 = nn.Upsample(scale_factor = 2, mode = 'nearest')
        #
        self.conv14 = Conv(128,64,1,1) #input is x14
        self.concat4 = Concat() #concat output of conv14, upsample2
        self.elan6 = Elan_block_tiny_yolo(64*2,32,64) #x57
        #
        self.conv16 = Conv(64,128,3,2)
        self.concat5 = Concat() #concat x47, conv16 output
        self.elan7 = Elan_block_tiny_yolo(128*2,64,128) #x65
        #
        self.conv18 = Conv(128,256,3,2)
        self.concat6 = Concat() #concat x37, output of conv18
        self.elan8 = Elan_block_tiny_yolo(256*2,128,256) #x73
        #
        self.conv20 = Conv(64,128,3,1)
        self.conv21 = Conv(128,256,3,1)
        self.conv22 = Conv(256,512,3,1)
        #
        self.idetect1 = IDetect(self.n_classes, self.anchors, [128,256,512])
        # self.idetect1 = self.get_idetect_layer()
    
    def get_idetect_layer(self):
        idetect1 = IDetect(self.n_classes, self.anchors, [128,256,512])
        idetect1.stride = torch.tensor([ 8, 16, 32])
        idetect1.anchors /=idetect1.stride.view(-1,1,1)
        return idetect1
    
    def fetch_last_layer(self):
        return self.idetect1
    
    def update_last_layer(self, layer):
        self.idetect1 = layer
    
    def forward(self, x14_x21_x28):
              
        ############# head #############
        x14, x21, x28 = x14_x21_x28
        x1 = self.conv7(x28)
        x2 = self.conv8(x28)
        sp1 = self.sp_pool1(x2)
        sp2 = self.sp_pool2(x2)
        sp3 = self.sp_pool3(x2)
        #
        concat1 = self.concat1([x2,sp1,sp2,sp3])
        concat1 = self.conv9(concat1)
        #
        concat2 = self.concat2([x1,concat1])
        x = self.conv10(concat2)
        x37 = x
        #
        x = self.conv11(x)
        x = self.upsample1(x)
        #
        x21_conv = self.conv12(x21)
        x = self.concat3([x21_conv,x])
        x = self.elan5(x)
        x47 = x
        #
        x = self.conv13(x)
        x = self.upsample2(x)
        #
        x14_conv = self.conv14(x14)
        x = self.concat4([x14_conv, x])
        x = self.elan6(x)
        x57 = x
        #
        x = self.conv16(x)
        x = self.concat5([x47,x])
        x = self.elan7(x)
        x65 = x
        #
        x = self.conv18(x)
        x = self.concat6([x37,x])
        x = self.elan8(x)
        x73 = x
        #
        x57_conv = self.conv20(x57)
        x65_conv = self.conv21(x65)
        x73_conv = self.conv22(x73)
        #
        out = self.idetect1([x57_conv, x65_conv, x73_conv])
        return out 
    
    def load_checkpoint(self):
        pass
```



## Step 3: create configs and train
Create a training run directory at `examples/wrapper_runs/runs_yolov7_tiny`. This directory will contain the master config as well as a directory containing the child configs.

Some important pointers while defining `flags` in the `.yaml` are:
1. `--project`: all training results are stored in the directory **`<--project>/<--name>/`**. 
2. `--name`: only keep inside the child config. ideally it should be same or similar as the child config name.

**How to set up a training run directory:**
First, make a **`master_config.yaml`** file inside the **`runs_yolov7_tiny/`** directory.

**`examples/wrapper_runs/runs_yolov7_tiny/master_config.yaml`**
```
flags:
  data: ../train_data/vehicle_detection/pravar_exp1/coco.yaml
  weights: yolov7-tiny.pt
  img-size: 640
  batch-size: 32
  project: ./examples/wrapper_runs/runs_yolov7_tiny/train/results

hyperparameters:
  lr0: 0.001
  lrf: 0.01
  momentum: 0.937
  weight_decay: 0.0005
  warmup_epochs: 3.0
  warmup_momentum: 0.8
  warmup_bias_lr: 0.1
  box: 0.05
  cls: 0.5
  cls_pw: 1.0
  obj: 1.0
  obj_pw: 1.0
  iou_t: 0.2
  anchor_t: 4.0
  fl_gamma: 0.0
  hsv_h: 0.015
  hsv_s: 0.05
  hsv_v: 0.07
  degrees: 3
  translate: 0.1
  scale: -0.25
  shear: 2.0
  perspective: 0.01
  flipud: 0.1
  fliplr: 0.1
  mosaic: 0.01
  mixup: 0.01
  copy_paste: 0.1
  paste_in: 0.1
  loss_ota: 1

cfg:
  # parameters
  nc: 8  # number of classes
  depth_multiple: 1.0  # model depth multiple
  width_multiple: 1.0  # layer channel multiple
  
  # anchors
  anchors:
    - [10,13, 16,30, 33,23]  # P3/8
    - [30,61, 62,45, 59,119]  # P4/16
    - [116,90, 156,198, 373,326]  # P5/32
  
  # yolov7_tiny backbone
  # [[Module, [*Args]]]
  backbone: [[yolov7_tiny.Backbone, [cfg_dict, 'yolov7_tiny.pt']]]
  
  # yolov7_tiny head
  # [[Module, [*Args]]]
  head: [[yolov7_tiny.Head, [cfg_dict, 'yolov7_tiny.pt']]]
```

Next, create a **`configs`** folder inside **`runs_yolov7_tiny/`** and create the following child configs:

**`examples/wrapper_runs/runs_yolov7_tiny/configs/config1.yaml**
```
flags:
  name: config1 # its mandatory to keep separate names for all the yaml files.

hyperparameters: {}

cfg: {}
```

**`examples/wrapper_runs/runs_yolov7_tiny/configs/config2.yaml**
```
flags:
  name: config2 # its mandatory to keep separate names for all the yaml files.

hyperparameters: {}

cfg: {}
```

## Step 4: start the training
After the training run directory and model has been created, you can start the training using the following command:
```
python train6ishaanwrapper.py --master-dir examples/wrapper_runs/runs_yolov7_tiny --overwrite-project True
```
