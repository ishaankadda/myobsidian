Example: Intrusion

# App
model is loaded and then the business logic runs on incoming frames.

# Perceptor
all the hyperfunctions, base class and stuff in percrtpror
all the other common stuff will be in **common_modules.h**
application and stream metadata in **meta.h**

# models
`/opt/mtx/vistack/apps/models` -> spec and onnx inside folders
note that onnx path is with respect to folder

# Then
`/opt/mtx/vistack/apps/apps/fire_and_smoke_detection` -> make the 4 files
and then write the code.

After this, do:
1. `meld cmake tasks` -> makes the .o files

For every single change in your code, do
1. `meld make tasks` -> compiles to binary


# ~~Input and Output shapes (done, consistent)
yolov3-tiny.onnx (`person_head.tiny_yolov3`):
```
Input Shapes: {'000_net': [-1, 3, 640, 640]}
Output Shapes: {'016_convolutional': [-1, 21, 20, 20], '023_convolutional': [-1, 21, 40, 40]}
```
v8n_redh.onnx (`anpr_plate_vehicle_detector.nano_yolov8`):
```
Input Shapes: {'images': [1, 3, 416, 416]}
Output Shapes: {'output': [1, 3549, 11]}

```
### script
```
import onnx

# Load the ONNX model
model = onnx.load("your_model.onnx")

# Get the model's graph
graph = model.graph

# Retrieve input shapes
input_shapes = {}
for input_tensor in graph.input:
    name = input_tensor.name
    shape = [dim.dim_value for dim in input_tensor.type.tensor_type.shape.dim]
    input_shapes[name] = shape

# Retrieve output shapes
output_shapes = {}
for output_tensor in graph.output:
    name = output_tensor.name
    shape = [dim.dim_value for dim in output_tensor.type.tensor_type.shape.dim]
    output_shapes[name] = shape

print("Input Shapes:", input_shapes)
print("Output Shapes:", output_shapes)
```

# ~~class names (done, updated in spec.json)
```
{0: 'Fire', 1: 'default', 2: 'smoke'}
```

# confidence threshold 
(updated to 0.25 from [github](https://github.com/Abonia1/YOLOv8-Fire-and-Smoke-Detection), was 0.6 in piyush `v8n_redh.json`, might need tuning)

# v7 training
```
python train.py --workers 4 --device 0 --batch-size 16 --data /workspace/firesmoke/YOLOv8-Fire-and-Smoke-Detection/datasets/fire-8/data.yaml --img 640 --cfg cfg/training/yolov7.yaml --weights '' --name yolov7_try1 --hyp data/hyp.scratch.p5.yaml

```
**Results:**
```
     Epoch   gpu_mem       box       obj       cls     total    labels  img_size
   299/299     15.5G   0.03195  0.008786  0.004735   0.04547        52       640: 100%|_____________________________________________________________________________________________________________| 55/55 [00:24<00:00,  2.24it/s]
               Class      Images      Labels           P           R      mAP@.5  mAP@.5:.95: 100%|___________________________________________________________________________________________________| 2/2 [00:00<00:00,  3.58it/s]
                 all          47          48       0.868       0.861       0.864       0.383
                Fire          47          18       0.811       0.889       0.848       0.337
               smoke          47          30       0.926       0.833       0.879       0.429
300 epochs completed in 2.132 hours.

```






**FireSmoke**
- Trained yolov7 model, modified onnx located locally at `/home/kadda/Downloads/firesmoke_outputs/onnx-modifier/modified_onnx/modified_modified_best.onnx`
- anchors from the best.pt file are:
```
model.model[-1].anchors = tensor([[[ 1.50000,  2.00000],
         [ 2.37500,  4.50000],
         [ 5.00000,  3.50000]],

        [[ 2.25000,  4.68750],
         [ 4.75000,  3.43750],
         [ 4.50000,  9.12500]],

        [[ 4.43750,  3.43750],
         [ 6.00000,  7.59375],
         [14.34375, 12.53125]]], dtype=torch.float16)
         
model.model[-1].anchor_grid = tensor([[[[[[ 12.,  16.]]],


          [[[ 19.,  36.]]],


          [[[ 40.,  28.]]]]],




        [[[[[ 36.,  75.]]],


          [[[ 76.,  55.]]],


          [[[ 72., 146.]]]]],




        [[[[[142., 110.]]],


          [[[192., 243.]]],


          [[[459., 401.]]]]]], dtype=torch.float16)

```
- TODO: integrate into app with a spec.json
- TODO: after app works, check the channel order with the onnx