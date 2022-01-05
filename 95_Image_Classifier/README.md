# 93_Image_Classifier

## 기본 정보

- 목적 : 딥러닝의 기본 개념을 익히고 pytorch에 익숙해지자

- 기간 : 2021/08/30 ~ 2021/09/02

- Ref :

  ​	CIFAR-10 

  ​	파이썬 딥러닝 파이토치



## 딥러닝 개념

- Initialization 

  어디서 시작하느냐에 따라 최적값이 달라질 수 있다.

- Batch Normalization

  output을 정규화해준다. 안 해주면 간혹 첫 ReLU나 두번째 ReLU나 차이가 없어져서, 기껏 여러 층을 쌓은 것이 무용지물이 될 수도 있다. 

- Dropout

  어떤 층의 column에서 일부를 0으로 바꾸겠다는 뜻. overfitting을 방지하기 위함

- 비선형함수 

  레이어 사이사이에 끼어있다. 함수를 통과하면 표현력이 증폭된다. 미분하면 0에 수렴하는 값이나 무한대가 나오기도 한다. 기울기 소실이나 폭주의 원인이 된다. 이에 대한 대안으로 ReLU 함수를 쓰고 있다.

- 역전파 

  학습이 param 선정 > cost 계산 으로 이루어진다면, cost 미분 > param 조정하겠다는 뜻

- momentum 

  gradient 방향으로 가되, learning rate에 관성을 추가한다. 기존의 optimization보다 더 빠르고, local minimum에 도달할 위험이 감소한다.



## CNN

- 전체적인 아이디어 
  - 이미지에서 특징을 추출(Feature Map) ➡️  Fully Connected Layer에 input하겠다는 것이다.
  - 이미지를 저화질로 바꾸어도 강하게 남아있는 특징만을 추출하겠다

```python
import torch.nn as nn
import torch.nn.functional as F

# Req. 1-2	분류기 모델 설계
class Classifier(nn.Module):
    def __init__(self):
        super(Classifier, self).__init__()       # Child 클래스의 __init__() 메소드내에서 super().__init__()을 입력하면 Parent 클래스의 __init__()에 있는 클래스 변수들을 가지고 올 수 있다. 
        
        # 이미지 3-channel 입력
        self.conv1 = nn.Conv2d(           # 이미지를 직접 넣는다
            in_channels = 3,              # RGB이므로 3
            out_channels = 8,             # 한 칸을 필터가 몇번 훑느냐
            kernel_size = 3,              # 필터의 크기
            padding = 1)                  # 이미지 가는 중앙에 비해 덜 연산되는 것을 막기 위함. 숫자 맞추는 거 어려움.
        self.conv2 = nn.Conv2d(
            in_channels = 8,              # 이전 레이어의 output이 8-depth 이므로
            out_channels = 16,            
            kernel_size = 3,
            padding = 1
        )
        self.pool = nn.MaxPool2d(
            kernel_size = 2,              # pooling하는 필터의 크기
            stride = 2)
                                          # 이 과정을 거쳐 탄생한 feature map은 합해지고 겹쳐지는 과정에서 주변 정보를 반영한다.
        # 최종 10개의 class에 대한 확률
        self.fc1 = nn.Linear(8*8*16, 64)  # input 숫자는 계산해서 나와야 함
        self.fc2 = nn.Linear(64, 32)
        self.fc3 = nn.Linear(32, 10)      # 최종적으로 10개의 클래스로 분류하므로
    

    def forward(self, x):
        x = self.conv1(x)                 
        x = F.relu(x)
        x = self.pool(x)
        x = self.conv2(x)
        x = F.relu(x)
        x = self.pool(x)

        x = x.view(-1, 8*8*16)            # FC layer는 1차원을 입력받는다. 2차원 데이터를 1차원 데이터로 변환한다.
        x = self.fc1(x)
        x = F.relu(x)
        x = self.fc2(x)
        x = F.relu(x)
        x = self.fc3(x)
        x = F.log_softmax(x)              # softmax는 각 클래스에 속할 확률을 나타내며, 총합은 1이다.

        return x

classifier = Classifier()
answer = classifier.forward(x)
```





- CNN에서 숫자들을 계산하는 방법에 대해 조금더 조사해보았다.
- forward > view에서 8 * 8 * 16인 이유에 대해서 알아보자



- 맨 처음에 이미지 사이즈는 [가로:32, 세로:32, 채널:3]이다

  ```
  images.size()
  > torch.Size([32, 3, 32, 32])
  ```

  

- conv1을 지나면 [가로:32, 세로:32, 채널:8]이 된다

- kernel로 훑으면서 가로와 세로가 줄어들지만, padding을 1로 주어 줄어들지 않았다.

- padding 값은 가로 세로가 줄어들지 않도록 하는 값을 넣어주어야 한다.

  ```
  self.conv1 = nn.Conv2d(in_channels = 3, out_channels = 8, kernel_size = 3, padding = 1)
  self.pool = nn.MaxPool2d(kernel_size = 2, stride = 2)
  ```

  

- pool을 지나면 [가로:16, 세로:16, 채널:8]이 된다

- pool의 kernel 사이즈가 2, stride가 2이므로 그렇다.



- 실전에서는.. 헷갈리기 때문에 중간중간 .size나 .shape를 찍으며 확인해야 한다.

- input값, padding은 정확히 계산해주어야 한다
- out_channel은 보통 이미지 사이즈와 반비례하게끔 설정해준다.





### python 문법

[super](https://velog.io/@gwkoo/클래스-상속-및-super-함수의-역할)
