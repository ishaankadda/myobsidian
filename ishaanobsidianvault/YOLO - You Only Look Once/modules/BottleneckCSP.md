CSPNet is typically explained in the context of a DenseNet DenseBlock.

In DenseNet, features from previous layers are reused by the layers ahead of it inside of a DenseBlock. 

$x_1 = w_1 \cdot [x_0]$
$x_2 = w_2 \cdot [x_1, x_0]$
$...$
$x_k = w_k \cdot [x_1, x_0, ..., x_{k-1}]$ 

In this way, the gradient of all the layer activations after and including layer $i$ is necessary to compute the gradient of $w_i$. The gradient of the weights starts to look like this...

$w_1' = f(w_1, g_0)$
$w_2' = f(w_2, g_0, g_1)$
$...$
$w_k' = f(w_k, g_0, g_1, ..., g_{k-1})$

the contribution of each gradient to the layers previous to it stacks up quite quickly. a **large amount of gradient information is reused** for updating weights of different dense layers. this results in different dense layers **repeatedly learning copied information.**

This is precisely the problem that CSPDenseNet looks to solve. The base layer is separated into two parts. part 1 sends the input into a dense and then a transition layer, and part 2 transmits the input feature map as is. Then we combine the transmitted feature map and the dense block $\rightarrow$ transition layer outputs and send it onto the next stage of the network.

One very important observation to be made here is that the features of both parts are each sent into partial transition layers and **then** concatenated. if we concatenate before the transition layer, in that case the same gradient information will update both the parts. In other words, we want to **maximise the difference of recombination of gradients.**

This observation is quite evident in the entire vein of developments of Yolo modules


![[BottleneckCSP.excalidraw]]

absl-py-2.1.0 contourpy-1.2.1 cycler-0.12.1 fonttools-4.53.1 grpcio-1.64.1 kiwisolver-1.4.5 markdown-3.6 matplotlib-3.9.1 opencv-python-4.10.0.84 pandas-2.2.2 prettytable-3.10.0 protobuf-4.25.3 pyparsing-3.1.2 python-dateutil-2.9.0.post0 scipy-1.14.0 seaborn-0.13.2 tensorboard-2.17.0 tensorboard-data-server-0.7.2 thop-0.1.1.post2209072238 torchsummary-1.5.1 tzdata-2024.1 werkzeug-3.0.3