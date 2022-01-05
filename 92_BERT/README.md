# 92_BERT

#### 기본 정보

- 목적 : BERT를 활용해서 feature 를 추출해보자
- 기간 : 2021/09
- Ref : 



## feature

- 유사한 아이템을 추천해주는 알고리즘을 구현해보자
- 각 아이템은 이미지와 설명을 갖고 있는데, 여기서 feature 를 추출하고자 한다.
- 이미지에서 feature 를 추출하는 것은
  데이터를 구하기가 어렵고
  인풋한 데이터에 비해 아웃풋이 떨어진다고 생각했다.
- 그래서 설명에서 feature 를 추출하는 것을 목표로 삼았다.
  한편 feature 를 추출하는 것을 난이도 별로 단계를 나누었다. 

- 1단계 : 빈출 단어로 벡터를 만든다.
- 2단계 : 의미를 고려하여 문장을 벡터로 만든다
- 3단계 : 벡터화한 문장을 가장 잘 압축하는 단어를 추출하고, 태그로 삼는다



## 빈출 단어

- 벡터화 할 때에 '단어'를 활용한다.
- 사전을 활용해 단어를 추출하고, 전처리한다. 너무 자주 나오는 단어(나, 너, 당신, 이다)는 오히려 의미가 없을 수 있다.
- 이 단어를 속성으로 벡터를 만든다.
- 벡터 간 유사도를 코사인 각도를 기준으로 측정한다.
- 가장 유사한 벡터를 고른다.



## 문장 벡터화

- NLP는 단순히 단어 빈도 뿐만 아니라, 의미를 고려하는 단계에 이르렀다.

  