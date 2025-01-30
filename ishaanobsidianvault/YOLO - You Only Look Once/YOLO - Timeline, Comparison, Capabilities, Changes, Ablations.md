### YOLOV1
- 2016
- Pros and Cons - Good Classification, Low background localization errors, Bad mAP (small object localization)

- input image $i \rightarrow S \times S$ grid of cells $\rightarrow S \times S \times (B * 5 + C) \text{ tensor}$ 
- Every grid cell predicts 
	1. class $C$ - $\textcolor{red}{BCE Loss}$
	2. $B$ bounding boxes $(b_x, b_y, b_w, b_h)$ - $\textcolor{blue}{MSELoss}$
	3. $B$ confidence scores - $\textcolor{red}{BCELoss}$
		*Note: Objects whose centres fall within a grid cell are predicted by it.*

### YOLOV2


### YOLOV3


### YOLOV4


### YOLOV5


### YOLOV6


### YOLOV7
> *[**Medium Blog** ~ YOLOv7: A Deep Dive into the Current State-of-the-Art for Object Detection](https://towardsdatascience.com/yolov7-a-deep-dive-into-the-current-state-of-the-art-for-object-detection-ce3ffedeeaeb)*
> 	1. Feature Pyramid Networks (FPN)
> 	2. Anchor Boxes
> 	3. EMA, Cosine LR, grouped weight decay, Centre Priors
> 	4. Data Augmentation
> 		1. Mosaic Augmentation
> 		2. Mixup Augmentation
> 	5. Explanation of loss function

- **FPN outputs**
	![[Pasted image 20240604165631.png]]
	```
	>>> print([type(fpn_output), len(fpn_output)])
	[list, 3]
	
	>>> print([o.shape for o in fpn_output])
	[
		# batch_size, n_anchor_sizes, n_grid_rows, n_grid_cols, n_features
		[1, 3, 80, 80, 85], # lead head -1
		[1, 3, 40, 40, 85], # lead head -2
		[1, 3, 20, 20, 85], # lead head -3
	]
	```
- $(c_x, c_y): final = 2*\sigma(initial) - 0.5 \in (-0.5, 1.5) \text{ , since anchors located at top left of each cell and offsets are relative to this}$
- $(w_x, w_y): final = (2*\sigma(initial))^2 \in (0, 4)$
- $(obj\_score): final = \sigma(initial) \in (0, 1)$
- $(cls\_score): final = \sigma(initial) \in (0, 1)$


step 1:
something like $c_x = t_x$

step 2:
$c_x = 2 * \sigma(c_x) - 0.5$


- **Center Priors**
asdf

- **Optimal Transport Assignment**
Considers label assignment as a *global optimization problem for each image.*

- **YOLOv7 Loss algorithm**
asdf
### References
1. [[Going deeper with convolutions - GoogLeNet 2014]]
2. 
3. 


