# 97 Interpretable_Models

## 기본 정보

- 목적 : 모델을 해석하자

- 기간 : 2021/05/16 ~ 2021/05/19

- Ref : 

  - [TextBook](https://christophm.github.io/interpretable-ml-book/agnostic.html)



## Model-Agnostic Methods



### Global Surrogate Model

- approximate black box model in interpretable models
- Disadvantage
  - not clear what the best cut-off for R-squared is
  - when very close for one subset, but not widely

### LIME

- local surrogate model
  - generate perturbed samples
  - minimize loss
  - select the nb of features using Lasso, Forward/Backward selection
- Disadvantage
  - not clear what the best kenel width & neighborhood is
- Advantage
  - works for tabular data, text and images
  - text : generate new text by randonly removing words
  - image : segment image into superpixels, which is interconnected pixels with similar colors

### Scoped Rules

- IF feature 1 2 ... ( 3 4 fixed ) THEN result ... rules
  - if changes in other feature values do not affect the prediction, a rule anchors a prediction
  - express their boundaries, where the same rule is applicable
- Algorithm
  - generate rules by extending previous best candidate
  - satisfying condition,
  - maximizing boundaries
- Case
  - near decision boundary : need more mb of feature, lower coverage
  - unbalanced set : use subset not all
- Disadvantages
  - what perturbation function to use
  - require discretization

### Shapley Values

- the j-th feature contributed j-amount to the prediction
  - marginal contribution : the instance - average of the coalitions
  - average thesemarginal contribution of all possible coaltions
- Disadvantage
  - Be careful of interpretation. it does not mean the difference of the predicted value after removing the feature.
  - when some feature value is missing, we achieve values by sampling. But if the feautres are dependent, sampled values may not make sense. In this case, get one mutual value of use conditional sampling

### SHAP

- Shapley value를 linear하게 합산한 것. Individual instance 내에서 합할 수도, globally 합할 수도 있다. Shapley value가 individual instance를 해석하는 것이라면, SHAP는 global 해석도 가능하다. 

- SHAP 라이브러리는 여러가지 시각화 기능을 제공한다. 

  - Individual instance에서 positive, negative 값
  - SHAP 에 근거한 feature importance
  - 모든 변수와 SHAP의 상관관계를 살펴보는 그래프
  - 각 변수와 SHAP의 상관관계를 살펴보는 그래프

  *분류 문제에서는 각 단계별로 1)을 제공한다

- perturbed function ?!?



