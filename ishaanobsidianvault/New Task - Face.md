FACE DATA -> `/home/kadda/face_backup/face.zip` 
3 classes -> 0, 1, 2 -> front, side head

Try yolo11x person detector -> infer on all the images and add a class for person with index 3
**Results**
```
Validating runs/detect/train/weights/best.pt...
Ultralytics 8.3.27 __ Python-3.10.15 torch-2.5.1+cu124 CUDA:0 (NVIDIA RTX 4000 SFF Ada Generation, 20138MiB)
Model summary (fused): 168 layers, 3,006,428 parameters, 0 gradients, 8.1 GFLOPs
                 Class     Images  Instances      Box(P          R      mAP50  mAP50-95): 100%|__________| 26/26 [00:03<00:00,  7.03it/s]
                   all        829      14642      0.788      0.683       0.76      0.523
            front-face        714       2713       0.79      0.737      0.808      0.523
             side-face        555       1491      0.717      0.568      0.656      0.398
                  head        582       3644      0.793      0.506      0.623      0.364
                person        829       6794      0.853      0.921      0.954      0.807
Speed: 0.1ms preprocess, 0.7ms inference, 0.0ms loss, 1.3ms postprocess per image
Results saved to runs/detect/train
__ Learn more at https://docs.ultralytics.com/modes/train
(face_annotation) root@e09ffd94ab28:/workspace/training# Timeout, server 192.168.3.14 not responding.

```