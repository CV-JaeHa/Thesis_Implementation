## Fully Convolutional Networks for Semantic Segmentation

![0](https://miro.medium.com/max/700/1*xWh0LhoDzwyjcxXpnz9lIQ.png)

Semantic Segmentation 문제를 위해 제안된 딥러닝 모델

(입력 이미지에 대해 픽셀 단위로 배경 및 각 개체의 클래스를 분할하는 동시에 예측한다.)

## Process

1. Convolutuonalization
2. Deconvolution (Upsampling)
3. Skip architecture

### Convolutionalization

![1](https://miro.medium.com/max/700/1*8ckDGCdl3YQy1V7sJ0hPTg.png)

Image classification 모델들은 기본적으로 내부 구조와 관계없이 모델의 근본적인 목표를 위해 출력층이 Fully-connected(이하 fc) layer로 구성되어 있다.

그러나 fc layer가 갖는 한계점이 있다.

1. 이미지의 위치 정보가 사라진다.
2. 입력 이미지 크기가 고정된다.

Semantic Segmentation은 입력 이미지의 위치 정보가 유지 되어야한다.
→ fc layer를 Conv-layer로 대체 하였다.

![2](https://miro.medium.com/max/700/1*x6mXFw0k5GuiLxhHjwqg-A.png)

결론 

1. Convolutionalization을 통해 출력 Feature map은 원본 이미지의 위치 정보를 내포할 수 있게 되었다.
2. Coarse map을 원본이미지 크기에 가까운 Dense map으로 변화해줄 필요가 있다.

### Deconvolution

Coarse map에서 Dense map을 얻는 방법

1. Interpolation
2. Deconvolution
3. Unpooling
4. Shift and stitch

Bilinear Interpolation

10 * 10 이미지를 320 * 320 이미지로 확대하려면 Bilinear interpolation을 쓰는데 이것을 이해하기 위해서는 Linear interpolation을 우선으로 이해해야 한다.

![3](https://miro.medium.com/max/700/1*GH-pex9uDkeS11wDIJBeTw.png)

![4](https://miro.medium.com/max/700/1*arKsBkAMZG-EDF2WIVBUNw.png)

Backwards convolution

Dense prediction을 위한 Upsampling 방법에는 Bilinear interpolation처럼 정해진 방법만 있는 것은 아니다. 즉, Up-sampling도 학습이 가능하다.

FCNs에서는 Bilinear interpolation과 Backwards convolution 두 가지 방법을 사용하여 Coarse Feature map으로부터 Dense prediction을 구했다.

![5](https://miro.medium.com/max/700/1*KYhWzQ31KhQf1OUVZ72bGw.png)

마지막 레이어에 Up-sampling 연산을 추가해 Dense prediction을 처리함.

![6](https://miro.medium.com/max/700/1*67WIJexGu6DMmiWrQ2eUNw.png)

보다 정교한 Segmentation을 위해서 추가적인 작업이 필요한 이유다.

### Skip Architecture

정확하고 상세한 구분(Segmentation)을 얻기 위해 **Deep** **&** **Coarse**(추상적인) 레이어의 의미적(Semantic) 정보와 **Shallow & fine** 층의 외관적(appearance) 정보를 결합한 **Skip architecture**를 정의한다.

![7](https://miro.medium.com/max/700/1*pbEMiRkXz65ejneuuToK3g.png)

![8](https://miro.medium.com/max/700/1*0jfFCywsgaJPTzAU5HzmfA.png)

FCNs 연구팀은 이러한 직관을 기반으로 앞에서 구한 Dense map에 얕은 층의 정보를 결합하는 방식으로 Segmentation의 품질을 개선하였다.

각 Pooling에 Prediction을 위해 추가된 Conv layer의 필터는 0으로, Trainable Backwards convolution은 Bilinear interpolation으로 초기화한 후 학습을 진행하였다.

![9](https://miro.medium.com/max/700/1*OXv6w9LaxEOHNhO18A8Zbg.png)

## 정리

FCNs은 기존의 딥러닝 기반 이미지 분류를 위해 학습이 완료된 모델의 구조를 Semantic Segmentation 목적에 맞게 수정하여 Transfer learning 하였다.

Convolutionalized 모델을 통해 예측된 Coarse map을 원본 이미지 사이즈와 같이 세밀(Dense)하게 만들기 위해 Up-sampling을 수행하였다.

또한 Deep Neural Network에서 얕은 층의 Local 정보와 깊은 층의 Semantic 정보를 결함하는 Skip architecture를 통해 보다 정교한 Segmantation 결과를 얻을 수 있었다.
