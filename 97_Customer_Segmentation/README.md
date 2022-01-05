# 98 Customer_Segmentation

## 기본 정보

- 목적 : 유사한 데이터로 인턴 프로젝트 정리

- 기간 : 2021/05/02 ~ 2021/05/12

- Ref : 

  - [Kaggle](https://www.kaggle.com/fabiendaniel/customer-segmentation)



## 어려웠던 점

- 정규표현식

정규표현식의 메타문자를 잊기 쉬운데 정리해보자. [참고1](https://wikidocs.net/4308) [참고2](https://wikidocs.net/4309)

```python
[] : 
 . : 모든 문자
 * : 반복(0개 이상)
 + : 반복(1개 이상)
 ^ : ~로 시작하는
 $ : ~로 끝나는
a-zA-Z : 모든 알파벳
  
eg. df[col].str.contains('^[a-zA-Z]+', regex=True)
```



- NLTK 다운로드

뭐든 환경설정이 가장 어려운 것 같다. nltk.download()를 했더니 계속 오류가 발생했다. document나 에러메세지에서 지정한 폴더에 다운로드했으나 경로를 찾을 수 없다고 했고, 다운로드 경로를 /Users/{myname}/nltk_data 로 지정했더니 문제없이 동작하였다. 이후에 direct kernel connection broken 경고가 뜨고 ipynb가 실행되지 않았다. 이는 conda install ipykernel 했더니 해결되었다. [참고](https://stackoverflow.com/questions/67185876/how-to-fix-error-direct-kernel-connection-broken)



- NLP

자연어를 어떻게 수치화할 것인가. 크게 데이터를 전처리하여 어근을 추출하고, 어근을 바탕으로 텍스트를 벡터화한다. 자연어처리를 공부하면서 등장하는 용어에 대해 정리해보았다.



- 클러스터링

여러 기법이 있지만 K-means과 MeanShift를 비교하였다. K-means는 거리를 기반으로 유사도를 측정하며, MeanShift는 밀도를 기반으로 유사도를 측정한다. 몇 개의 클러스터로 나눌 것인지는 모델이 스스로 정한다. 

가장 큰 차이점은 몇 개의 클러스터로 나눌 것인지 정해주어야 하는가 / 모델이 스스로 정하는가 이다. 값의 분포도를 잘 모르니 모델이 적합한 값을 제시하는 MeanShift가 수학적으로는 더 적절하다. 하지만 실무에서는 분류하기에 적정한 개수가 정해져 있는 경우가 있다. 모수인 bandwidth를 조정하여 클러스터의 개수를 간접적으로 조절할 수 있으나, 이때 하나의 집단이 비대해지는 문제가 발생할 수 있다. 

두 모델 모두 Curse of dimensionality로 고차원에서 성능이 나쁘다는 점, 모델을 fit하기 전에 scale을 해주어야 한다는 점에 주의해야 한다.

클러스터링의 metric으로 silhouette 계수를 쓴다. 근처 클러스터와 멀수록, 자기 클러스터 내에서 가까울수록 좋은 클러스터링임을 담은 공식이다. 전체 실루엣 계수는 1에 가까울수록 좋다. 하지만 특정 클러스터의 실루엣 계수만 유난히 높은 경우도 있으므로, 모든 군집의 실루엣 계수 편차가 크지 않은지 살펴보아야 한다. 



- Feature Selection

분류모델의 성능을 높이기 위해서는 어떤 칼럼을 사용하느냐가 매우 중요하다. 나는 영업관리 팀과 회의하여 후보군을 모으고, EDA를 통해 후보군을 추렸다. 이후 수학적으로 계산한 feature importance와 마케팅 팀에서 조언한 바를 바탕으로 9개의 칼럼으로 추렸다. 이전에는 담당자 목표치도 포함했으나 악용될 가능성을 우려해 제하였다.

|    항목     | 구체적인 지표                                   |
| :---------: | :---------------------------------------------- |
|    성장     | 상대적 성장점수, 절대적 성장점수, 시장 성장점수 |
|   수익성    | 우량품목비율, 우량품목수                        |
| 우호적 관계 | 신제품 수용률, 이슈제품 스위칭 여부             |
|    매출     | 당월매출, 이전매출                              |



- PCA와 클러스터링

PCA는 공분산 행렬을 eigenvalue와 eigenvector로 분해한다. PCA는 주로 고차원 행렬을 저차원으로 축소하거나, K-means 클러스터링의 성능을 높이기 위해서 사용한다. 차원을 축소하는 경우 eigenvalue에서 몇 개로 추리느냐가 중요한데, total explained variance가 일정 수준 이상 + explained variance의 기울기가 급격히 변하는 개수를 선택한다.

분석 사례에서 PCA를 해놓고 축소하지 않는 것이 의아했다. 그런데 K-means 클러스터링의 성능을 높이기 위해 PCAf를 적용하기도 한다. K-means 클러스터링의 중심점은 다른 클러스터와 멀고 자기 클러스터 내에서 가까울수록 좋은데, 이는 PCA의 projection 과 유사하다. 또한 PCA를 한 후에 클러스터링을 수행하면 노이즈를 줄일 수 있다는 것이 아이디어이다. 

여담으로 PCA를 클러스터링 외에 feature extraction에도 활용하면 어떨까. 각 pc를 회전시켰을 때 성분의 값이 1 혹은 0에 수렴한다면, 공통된 factor가 존재한다고 가정할 수 있을 것이다. 



- 분류모델

SVC, Logistic, KNN, DecisionTree, RandomForest, AdaBoost, GradientBoosting 등 여러 모델을 적합시켰다. 각 모델은 GridSearch를 이용하여 파라미터 후보군 중에 최적값을 뽑았다. 이후 우수한 모델을 앙상블하여 모델을 만들었다. 



- metric

분류모델을 평가하기 위해 세 가지를 사용하였다. 정확도는 전반적인 성능을 살피기 위해, 오차행렬(Confusion matrix)는 그룹별 성능을 볼 수 있다. 그룹1의 성능만 유독 나쁘거나, 그룹3과 그룹4를 잘 구분하지 못한다거나. 자세한 정보를 제공한다. 

Learning curve는 overfitting, underfitting을 감지하기 위해서 사용한다. good fit은 학습셋과 검증셋의 성능이 수렴하며, 완만한 경사도를 보인다. 이때 학습셋의 성능이 약간더 좋은  것은 당연하다. 반면에, underfitting은 충분히 학습하지 못했기에 학습셋의 성능이 매우 나쁘다. 혹은 학습이 더 필요할 경우 경사가 아주 급할 수 있다. overfitting은 학습셋에 너무 최적화되면서, 어떤 시점부터 검증셋에서의 성능이 오히려 나빠지기 시작한다. 더하여, 데이터가 unrepresentative할 경우 성능이 들쭉날쭉하다. [참고](https://machinelearningmastery.com/learning-curves-for-diagnosing-machine-learning-model-performance/)

이외에도 분류모델 평가에 활용할 수 있는 metric이 많다. 정밀도는 FP를 줄이는 것, 재현율은 FN을 줄이는 것을 목표로 한다. F1 스코어는 이 둘을 합친 것이다. ROC 곡선은 두 분류 집단의 분포에 따라 곡선이 다르게 휜다. 그리고 threshold에 따라 값이 변한다. AUC는 ROC를 적분한 것으로, 1에 가까울수록 두 분포가 분리되어 있음을 의미한다. [참고](https://angeloyeo.github.io/2020/08/05/ROC.html)











