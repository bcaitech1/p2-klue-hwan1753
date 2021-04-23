# p2-klue-hwan1753

# 기술적인 도전

__최종 리더보드 점수 79.3, 63등__

## 검증(Validation) 전략
1. train_test_split(0.2) 활용하여 최적의 모델 찾기.
2. 모델을 찾은 이후 K Fold 활용하여 cross validation 실행
3. stratified K Fold 활용하여 실행 -> 메모리, 시간 부족으로 인하여 실패

## 사용한 모델 아키텍처 및 하이퍼 파라미터
### Bert-base
  - LB 점수 : 71
  - Training Arguments
    - epochs = 4
    - learning_rate = 5e-5
    - batch_size = 16
    - warmup_steps = 500
    - weight_decay = 0.01
  - Text argument X

### Kobert
  - LB 점수 : 0.67
  - 기타 시도
    - hugging face에서 모델을 불러오는 것이 아닌 monologg/KoBERT-Transformers에서 git clone을 한 이후에 example code를 직접 해봄으로써 이해하려고 노력함.

### kykim/bert-kor-base
  - 피어세션을 통해서 다른 캠퍼님이 해당 아키텍처를 추천해줌. 다른 모델에 비해 성능이 많이 올라감.
  - LB 점수 : 75.5
  - Data Augmentation
    - papago API를 통해 back translation 하려고 함 -> 일일 제한으로 인해 실패
    - papago Selenium 활용하여 크롤링으로 진행 시도 -> 너무 느려서 multiprocessing 활용해서 시도 -> 결과가 좋지 않음. (이후에 papago 주소에 쿼리값이 있다는 것을 나중에 알게 됨.)
    - 토론 게시판의 pororo 라이브러리 활용한 데이터 사용 -> 결과가 좋지 않음.
    -데이터의 특수문자(ex : 한자)를 제거하여 사용한 경우 -> 결과가 더 좋게 나와서 이후 데이터 fix

### R-Bert
- 피어세션과 토론에서 가장 좋은 모델이라는 평을 받아 사용해보려함.
- LB 점수 : 78.3
- Training Arguments
  - epochs = 8
  - learning_rate = 1e-5
  - batch_size = 32
  - warmup_steps = 300
  - weight_decay = 0.01
  - num_workers = 4
  - label_smoothing_factor = 0.5
- K Fold를 통해 cross validation을 실행 -> LB 점수 : 79.3(hard voting)
  - soft voting과 hard voting 두 가지 나눠서 실험해본 결과, hard voting이 더 좋았음 -> 이유를 생각해보면 데이터 class가 이미 unbalancing했고 특정 라벨을 예측 잘 못한 것보다 0을 다른 라벨로 예측하는 경우가 많았음. 따라서 단순히 갯수로 voting을 하고 0과 다른 라벨이 동률인 경우 0이라고 생각하는 것이 결과가 더 좋았음.


# competition 후기
- 피어세션을 진행하면서 창의적인 방법에 대한 아이디어를 많이 얻을 수 있었고 시간상 하지 못한 실험들을 서로 공유하면서 어떤 경우에 더 높게 나왔고 어떻게 하면 안좋게 나왔다는 의견을 공유할 수 있었다. 개인적으로 궁금한데 해결하지 못한 부분도 해결할 수 있었고 생각하지 못한 방향으로 접근할 수 있다는 것도 알게 됐다. data augmentation에 대한 논문을 보면서 진행해보려고 했지만 생각만큼 안된 점과 서버 용량문제로 인해서 모델들을 삭제해 앙상블기법을 하지 못한 점이 아쉽다.
- 그래도 이전 스테이지1 랩업 리포트에서 받은 피드백을 실행할 수 있었다는 점과 wandb를 처음 사용하면서 기록에 대한 중요성을 알게 되어서 좋았다.
- 마지막 피어세션 시간에서 R-Bert의 Loss가 이상하게 계산이 되어서 질문을 했고 서석민 캠퍼님의 도움으로 궁금증을 해결할 수 있었다.(기존 base-line 코드를 그대로 사용했더니 발생한 문제였다.)
