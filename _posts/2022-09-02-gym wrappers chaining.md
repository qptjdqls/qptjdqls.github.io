---
layout: post
title: gym wrappers chaining
date: 2022-09-02 11:59:59 +0900
categories: python
permalink: /24
---

## Original code:
https://pytorch.org/tutorials/intermediate/mario_rl_tutorial.html <br>
https://github.com/pytorch/tutorials/blob/master/intermediate_source/mario_rl_tutorial.py <br>

---

## [github](https://github.com/qptjdqls/ddqn-mario/blob/main/README.md)

---

**Things to remember** <br>
-[Gym Wrappers](https://www.gymlibrary.dev/content/wrappers/) <br>
[Chaining previous environment method](https://github.com/pytorch/tutorials/blob/master/intermediate_source/mario_rl_tutorial.py) (chain of responsibility)<br>
-Usage of ```@torch.no_grad()``` <br>

**Things to improve** <br>
-Abuse copying e.g. ```.copy()```, ```__array__()``` <br>
-checking CUDA <br>
-Exploration rate scheduling <br>
Use $\epsilon = \alpha - e^{\beta -x}$ form instead <br>
-Logging with Tensorboard

---

### **gym wrappers chaining** <br>

```python
'''https://github.com/pytorch/tutorials/blob/master/intermediate_source/mario_rl_tutorial.py'''
class GrayScaleObservation(gym.ObservationWrapper):
    def __init__(self, env):
...
    def observation(self, observation):
        observation = self.permute_orientation(observation)
        transform = T.Grayscale()
        observation = transform(observation)
        return observation


class ResizeObservation(gym.ObservationWrapper):
    def __init__(self, env, shape):
...

    def observation(self, observation):
        transforms = T.Compose(
            [T.Resize(self.shape), T.Normalize(0, 255)]
        )
        observation = transforms(observation).squeeze(0)
        return observation


# Apply Wrappers to environment
env = SkipFrame(env, skip=4)
env = GrayScaleObservation(env)
env = ResizeObservation(env, shape=84)
env = FrameStack(env, num_stack=4)
```
위에서 확인할 수 있듯이 ```gym.ObservationWrapper``` 를 상속 받은 두 개의 클래스 ```GraySCaleObservation``` 과 ```ResizeObservation``` 에서 각각 ```observation``` method 를 override 하고 있다. <br>
그렇다면 ```env = ResizeObservation(env, shape=84)``` 를 통과했을 때 ```env.observation``` 이 이전 ```env = GrayScaleObservation(env)``` 를 덮어쓰기 때문에 의도한 대로 동작하지 않는지 의문이 들었다. <br>
이런 의문점은 gym 의 [core.py](https://github.com/openai/gym/blob/master/gym/core.py) 코드를 확인함으로써 해결할 수 있었다. <br>

```python
'''https://github.com/openai/gym/blob/master/gym/core.py'''
class Wrapper(Env[ObsType, ActType]):
    """Wraps an environment to allow a modular transformation of the :meth:`step` and :meth:`reset` methods.
    This class is the base class for all wrappers. The subclass could override
    some methods to change the behavior of the original environment without touching the
    original code.
    Note:
        Don't forget to call ``super().__init__(env)`` if the subclass overrides :meth:`__init__`.
    """

    def __init__(self, env: Env):
        """Wraps an environment to allow a modular transformation of the :meth:`step` and :meth:`reset` methods.
        Args:
            env: The environment to wrap
        """
        self.env = env
    ...

class ObservationWrapper(Wrapper):
    ...

    def reset(self, **kwargs):
        """Resets the environment, returning a modified observation using :meth:`self.observation`."""
        obs, info = self.env.reset(**kwargs)
        return self.observation(obs), info

    def step(self, action):
        """Returns a modified observation using :meth:`self.observation` after calling :meth:`env.step`."""
        observation, reward, terminated, truncated, info = self.env.step(action)
        return self.observation(observation), reward, terminated, truncated, info

    def observation(self, observation):
        """Returns a modified observation."""
        raise NotImplementedError
```
```class ObservationWrapper(Wrapper)``` 은 ```class Wrapper``` 를 상속받고 ```observation``` 의 구현을 요구한다. <br>
주의해서 봐야할 것은 ```reset``` 과 ```step``` 메서드 안에서 ```self.env.reset``` 과 ```self.env.step``` 를 호출하고 새롭게 구현된 ```observation``` 을 호출하는 것을 확인할 수 있다. <br>
```env = GrayScaleObservation(env)``` 와 같이 ```self.env``` 는 인자로 받은 이전 ```env``` 를 가르키고, 즉 이는 linked list 와 같이 이전 env 를 기억하여 작업이 순차적으로 누적될 수 있도록 한다. <br>
이러한 chain 형태의 구현과 유사한 것으로 디자인 패턴 중 ***chain of responsibility*** 라는 패턴이 유사하고, 이는 웹개발에 자주 사용된다고 한다. <br>
추가적으로 ```env.observation``` 과 같이 ```observation``` 직접적으로 호출하는 방식을 사용해서는 안된다는 것을(혹은 이를 고려해서 구현을 해야하는 것을) 알 수 있다. <br>
```class Wrapper```의 주석에서 적혀있듯이 'Don't forget to call ``super().__init__(env)`` if the subclass overrides'. <br>
