---
layout: post
title: pytorch sequential, unfold, zip
date: 2022-08-27 11:59:59 +0900
categories: python
permalink: /21
---

# pytorch sequential, unfold, zip

## from [DQN Tutorial](https://pytorch.org/tutorials/intermediate/reinforcement_q_learning.html)

## 원본 글: [sequential](https://github.com/eriklindernoren/PyTorch-GAN/blob/master/implementations/infogan/infogan.py)

기존 Q-network 코드 <br>
```python
class DQN(nn.Module):

    def __init__(self, h, w, outputs):
        super(DQN, self).__init__()
        self.conv1 = nn.Conv2d(3, 16, kernel_size=5, stride=2)
        self.bn1 = nn.BatchNorm2d(16)
        self.conv2 = nn.Conv2d(16, 32, kernel_size=5, stride=2)
        self.bn2 = nn.BatchNorm2d(32)
        self.conv3 = nn.Conv2d(32, 32, kernel_size=5, stride=2)
        self.bn3 = nn.BatchNorm2d(32)
        ...

    def forward(self, x):
        x = x.to(device)
        x = F.relu(self.bn1(self.conv1(x)))
        x = F.relu(self.bn2(self.conv2(x)))
        x = F.relu(self.bn3(self.conv3(x)))
        return self.head(x.view(x.size(0), -1))
```
Sequential 활용한 코드 <br>
```python
class DQN(nn.Module):

    def __init__(self, h, w, outputs):
        super().__init__()
        def conv_block(in_filters, out_filters, kernel_size=5, stride=2):
            block = [
                nn.Conv2d(in_filters, out_filters, kernel_size, stride),
                nn.BatchNorm2d(out_filters),
                nn.ReLU(inplace=True),
            ]
            return block

        self.conv_blocks = nn.Sequential(
            *conv_block(3, 16),
            *conv_block(16, 32),
            *conv_block(32, 32),
        )
        ...

    def forward(self, x):
        x = x.to(device)
        x = self.conv_blocks(x)
        x = self.head(x.view(x.size(0), -1))
        return x
```

---

## 원본 글: [Unfold](https://stackoverflow.com/a/67112505)

```python
    # Take 100 episode averages and plot them too
    if len(durations_t) >= 100:
        means = durations_t.unfold(0, 100, 1).mean(1).view(-1)
        means = torch.cat((torch.zeros(99), means))
        plt.plot(means.numpy())
```
위 식의 의미는 아래 그림과 같이 길이가 100 인 window 를 한칸씩 이동시키며 mean 을 계산하겠다는 의미이다. <br> 
![](/public/img/2022-08-27-pytorchsequential,unfold,zip/2.png){: width="40%" height="40%"} <br>

```unfold``` images a tensor as a longer tensor with repeated columns/rows of values 'folded' on top of each other, which is then "unfolded": <br>
-```size``` determines how large the folds are
-```step``` determines how often it is folded
e.g. for a 2x5 tensor, unfolding it with ```step=1```, and patch ```size=2``` across ```dim=1```: <br>
```python
x = torch.tensor([[1,2,3,4,5],
                [6,7,8,9,10]])

>>> x.unfold(1, 2, 1)
tensor([[[ 1,  2], [ 2,  3], [ 3,  4], [ 4,  5]],
        [[ 6,  7], [ 7,  8], [ 8,  9], [ 9, 10]]])
```
![](/public/img/2022-08-27-pytorchsequential,unfold,zip/1.png){: width="40%" height="40%"} <br>

```fold``` is roughly the opposite of this operation, buy "overlapping" alues are summed in the output. <br>

---

## 원본 글: [zip](https://stackoverflow.com/a/67112505)

```python
Transition = namedtuple('Transition',
                        ('state', 'action', 'next_state', 'reward'))
    ...

    transitions = memory.sample(BATCH_SIZE)
    # Transpose the batch (see https://stackoverflow.com/a/19343/3343043 for
    # detailed explanation). This converts batch-array of Transitions
    # to Transition of batch-arrays.
    batch = Transition(*zip(*transitions))
    ...

    state_batch = torch.cat(batch.state)
    action_batch = torch.cat(batch.action)
    reward_batch = torch.cat(batch.reward)
```

```batch = Transition(*zip(*transitions))``` 의 의미 <br>

```python
>>> zip(*[('a', 1), ('b', 2), ('c', 3), ('d', 4)])
[('a', 'b', 'c', 'd'), (1, 2, 3, 4)]
```
The way this works is by calling ```zip``` with the arguments: <br>
```python
zip(('a', 1), ('b', 2), ('c', 3), ('d', 4))
```



