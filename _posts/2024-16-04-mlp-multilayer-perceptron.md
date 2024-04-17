
### 2. MLP: Multilayer Perceptron

#### Filename: `mlp-multilayer-perceptron.md`


---
layout: post
title: MLP: Multilayer Perceptron
---

# MLP: Multilayer Perceptron

## Overview of Multilayer Perceptron
- Thanks to for providing the in-depth details:  https://d2l.ai/chapter_multilayer-perceptrons/mlp-implementation.html
- If I stack multiple fully-connected layers on top of one another and output of one layer is the input of another where first (n-1)layers is my represenation 
    and final layer as linear predictor. This architecture is commonly called as MULTILAYER PERCEPTRON or MLP.
- The key conceptual difference - I, now concatenate multiple layers.
- It is first truly deep network.
- They are the simplest neural nets, consist of multiple layers of neurons => all fully connected => inputs->hidden{n-1 layers}-> output{nth layer}


## Forward Propagation

- Or forward pass: Refers to the calculation and storage of intermediate variables (including outputs) for a neural network in order from the input layer to the output layer. 

## Back Propagation
- Refers to the method of calculating the gradient of neural network parameters i.e. traversing the network in reverse order, from the output to the input layer, according to the chain rule from calculus. 

## WHY MORE MEMORY COSUMPTION IN NEURAL NETS?

- In training deep learning models, forward propagation and backpropagation are interdependent.
- Backpropagration is also one of the reasons why training requires significantly more memory than plain prediction due to its ability to hold on to the 
  intermediate value calculated during forward propagration.
- Also, the size of such intermediate values is roughly proportional to the number of network layers and the batch size.
- Thus, training deeper networks using larger batch sizes more easily leads to out-of-memory errors.

## Activation Functions

- **ReLU**: 
- **Sigmoid** 
- **Tanh**: 

## Code Notes:
- Resource link: https://d2l.ai/chapter_multilayer-perceptrons/mlp-implementation.html
- State of the art of the late 1980s when fully connected deep networks were the method of choice for neural network modeling.
- To begin, we will implement an MLP with one hidden layer and 256 hidden units.
- Both the number of layers and their width are adjustable (hyperparameters).
- Typically,the layer widths to be divisible by larger powers of 2. This is computationally efficient due to the way memory is allocated and addressed in 
  hardware.
- Fashion-MNIST contains 10 classes, so here, I can think of this as a classification dataset with 784 input features and 10 classes
- nn.Parameter to automatically register a class attribute as a parameter to be tracked by autograd

## Practical Implementation

```python
import torch
from torch import nn
from d2l import torch as d2l

class MLPScratch(d2l.Classifier):
    def __init__(self, num_inputs, num_outputs, num_hidden, lr, sigma=0.01):
        super().__init__()
        self.save_hyperparameters()
        self.W1 = nn.Parameter(torch.randn(num_inputs, num_hidden) * sigma)
        self.b1 = nn.Parameter(torch.zeros(num_hidden))
        self.W2 = nn.Parameter(torch.randn(num_hidden, num_outputs) * sigma)
        self.b2 = nn.Parameter(torch.zeros(num_outputs))

def relu(X):
    return torch.max(X, 0)

@d2l.add_to_class(MLPScratch)
def forward(self, X):
    X = X.reshape((-1, self.num_inputs))
    H = relu(torch.matmul(X, self.W1) + self.b1)
    return torch.matmul(H, self.W2) + self.b2

# Training
model = MLPScratch(num_inputs=784, num_outputs=10, num_hidden=256, lr=0.1)
data = d2l.FashionMNIST(batch_size=256)
trainer = d2l.Trainer(max_epochs=10)
trainer.fit(model, data)
```