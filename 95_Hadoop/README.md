# 95_Hadoop

## 기본 정보

- 목적 : Hadoop의 개념을 이해하고 실습을 해보자

- 기간 : 2021/06/11 ~ 2021/06/

- Ref : 

  - [edureka lecture](https://youtu.be/1vbXmCrkT3Y)





## Installation

- [참고](https://rap0d.github.io/tip/2019/10/01/mac_hadoop_in_mac/)
- Apple M1은 homebrew의 위치가 바뀌었다. 명령어 작성할 때 `/usr/local`를 `/opt/homebrew`로 바꿔주어야 한다.



- 해결하지 못한 오류
- WARN util.NativeCodeLoader: Unable to load native-hadoop library for your platform... using builtin-java classes where applicable



## Architecture

- 방식
  - 여러 개의 서버와 여러 개의 DB를 활용
  - Map Reduce (계산) + HDFS (저장) + YARN (리소스 관리)
- Master - Slave
  - Master는 작업을 효율적으로 분배하는 역할
  - Slave는 실제로 저장하거나, 계산하는 역할
- Map - Reduce
  - Map은 계산을 하고
  - Reduce는 취합을 한다
  - 코드 작성
    - 어떻게 map, reduce, drive 할 것인지 코드로 작성해야 한다
    - 하둡에서 지정한 인풋형식 & 아웃풋 형식 (key, value)에 맞춰서 작성해야 한다
    - python, java 등 다양한 언어로 작성할 수 있다



## Map Reduce Logic

[python](https://3months.tistory.com/521)

[java](https://kamang-it.tistory.com/entry/Hadoop-03맵리듀스-예제WordCount)

