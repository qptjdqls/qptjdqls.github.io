---
layout: post
title: pytorch Module, Sequential, ModuleList, and ModuleDict
date: 2022-09-19 11:59:59 +0900
categories: python
permalink: /27
---

# pytorch Module, Sequential, ModuleList, and ModuleDict

## 원본 글: [medium](https://towardsdatascience.com/pytorch-how-and-when-to-use-module-sequential-modulelist-and-moduledict-7a54597b5f17)

**[주의]: 여러개의 네트워크를 list 로 정의할 때 nn.ModuleList 사용** <br>

---

### Module: the main building block
The Module is the main building block, it defines the base class for all neural network and you MUST subclass it. <br>

### Sequential: stack and merge layers
Sequential is a container of Modules that can be stacked together and run at the same time.
```python
self.conv_block = nn.Sequential(
    nn.Conv2d(in_c, 32, kernel_size=3, stride=1, padding=1),
    nn.batchNorm2d(32),
    nn.ReLU(),
)
```

We could create a function that returns a ```nn.Sequential``` to even simplify the code. <br>
```python
def conv_block(in_f, out_f, *args, **kwargs):
    return nn.Sequential(
        nn.Conv2d(in_f, out_f, *args, **kwargs),
        nn.BatchNorm2d(out_f),
        nn.ReLU(),
    )
...
self.conv_block1 = conv.block(in_c, 32, kernel_size=3, padding=1)
self.conv_block2 = conv.block(32, 64, kernel_szie=3, padding=1)
```

We could merge them using ```nn.Sequential``` again <br>
```python
self.encoder = nn.Sequential(
    conv_block(in_c, 32, kernel_size=3, padding=1),
    conv_block(32, 64, kernel_size=3, padding=1),
)
```

### Dynamic Sequential: create multiple layers at once
```python
self.encoder = nn.Sequential(
    conv_block(in_c, 32, kernel_size=3, padding=1),
    conv_block(32, 64, kernel_size=3, padding=1),
    conv_block(64, 128, kernel_size=3, padding=1),
    conv_block(128, 256, kernel_size=3, padding=1),
)
```

We can create an array and pass it to ```Sequential``` <br>

```python
self.enc_sizes = [in_c, 32, 64]

conv_blocks = [conv_block(in_f, out_f, kernel_size=3, padding=1)
    for in_f, out_f in zip(self.enc_sizes, self.enc_sizes[1:])]

self.encoder = nn.Sequential(*conv_blocks)
```

### ModuleList: when we need to iterate

```ModuleList``` allows you to store ```Module``` as a list. <br>
It can be useful when you need to iterate through layer and store/use some informationm like in **U-net**. <br>
The main difference between ```Sequential``` is that ```ModuleList``` have not a ```forward``` method so the inner layers are not connected. <br>
Assuming we need each output of each layer in the decoder, we can store it by: <br>
```python
class MyModule(nn.Module):
    def __init__(self, sizes):
        super().__init__()
        self.layers = nn.ModuleLIst([nn.Linear(in_f, out_f) for in_f, out_f in zip(sizes, sizes[1:])])
        self.trace = []

    def forward(self, x):
        for layer in self.layers:
            x = layer(x)
            self.trace.append(x)
        return x
```

### ModuleDict: when we need to choose
What if we want to switch to ```LeakyReLU``` in our ```conv_block```? <br>
We can use ```modlueDict``` to create a dictionary of ```Modlue``` and dynamically switch ```Module``` when we want <br>

```python
def conv_block(in_f, out_f, activation='relu', *args, **kwargs):
    activations = nn.ModuleDict([
        ['lrelu', nn.LeakyReLU()],
        ['relu', nn.ReLU()],
    ])

    return nn.Sequential(
        nn.Conv2d(in_f, out_f, *args, **kwargs),
        nn.BatchNorm2d(out_f),
        activations[activation],
    )
```

### Conclusion
* Use ```Module``` when you have a big block compose of multiple smaller blocks <br>
* Use ```Sequential``` when you want to create a small block from layers <br>
* Use ```ModuleList``` when you need to itertate through some layers or building blocks and do something <br>
* Use ```ModuleDict``` when you need to parametise some blocks of your model, for example and activation function <br>