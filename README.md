# Quanvolutional Image Classification
Welcome to our project on classifying images using hybrid quantum-classical models! We are so glad to have you here.

A Google slide presentation of our project is linked [here](https://docs.google.com/presentation/d/15cliQYw5iHnrlowJ_3vq_JCDgef2uBWDGb1he-XRfdE/edit?usp=sharing).

The goal of our project was to utilize a quantum convolutional circuit to augment the performance of a classical image classifier on the MNIST, Fashion-MNIST, and CIFAR-10 datasets.
The quantum convolutional circuit was inspired by the [Pennylane tutorial on quantum neural networks](https://pennylane.ai/qml/demos/tutorial_quanvolution.html) and is structured as follows:

1. We iterate over 2x2 regions of an image and use a region's values for R<sub>y</sub> rotation angles in the first section of our quantum circuit.
2. We apply a unitary gate in the quantum circuit given by [RandomLayers](https://docs.pennylane.ai/en/stable/code/api/pennylane.RandomLayers.html) and parameterized by 4 random values in the second section of our quantum circuit.
3. We collect Z expectation values and map them back to their original locations in the image or compile them to make a multi-channel image.

A diagram of this circuit is given below:
![circuit2x2](https://user-images.githubusercontent.com/42923017/207743095-8435f2bd-e877-4ad3-acbb-78191bd1e5fa.png)


After feeding the images through our quantum convolutional circuit, we can either directly feed these quantum convolution results to a classical classifier as a baseline model (example of this preprocessing is done in `quanv_kernel_2_preprocess_mnist.ipynb` and the classical classifiers are in `lenet_baseline.ipynb`) or integrate them into the training of a few different hybrid quantum-classical convolutional models (done in `mnist_torch.ipynb` and **fashion-mnist_experiments**). When integrating the quantum convolution results into a hybrid quantum-classical CNN, we tried 4 different approaches and compared their test accuracies to those of the baseline CNN. Below are our 4 different approaches/experiments:

1. Quanv(x) + x: skip connection between the quantum convolution results and the original input image
2. Quanv(x) + Conv(x): skip connection between the quantum and classical convolution results of the input image
3. Concat(Quanv(x), x): concatenate quantum convolution results and the original input image
4. Concat(Quanv(x), Conv(x)): concatenate quantum and classical convolution results

Below are the test accuracies our experiments achieved on a 1000-600 train-test subset of the MNIST dataset:
![mnist_experiments_accuracy](https://user-images.githubusercontent.com/42923017/207742990-faeba8c2-9723-4032-b9c1-2d00522d00b6.png)

We see that experiments 1 and 3, performing the skip connection or concatenation at the beginning of forward function, achieve an accuracy of approximately 93.5%, higher than the baseline CNN model's accuracy of approximately 93.1%.

Below are the test accuracies our experiments achieved on a 1000-600 train-test subset of the Fashion-MNIST dataset:
![fashion_mnist_accuracy](https://github.com/muirheadmaster/quanvolution/blob/main/fashion_mnist.png)

We see that experiment 2, performing the addition of quanv output and conv output, achieve comparable performance as CNN.

While the training of hybrid quantum-classical models is markedly slower than training a classical model, we have demonstrated that they can achieve comparable or possibly better image classification with good experimentation.
