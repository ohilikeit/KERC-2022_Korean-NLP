![image](https://user-images.githubusercontent.com/37128004/197667144-14df50d1-e2b5-415a-904d-b29e8ef9989d.png)
# KERC 2022
## Introduction
전남대학교 인공감정지능 기초연구실 주최의 제 4회 한국인 감정인식 국제경진대회에 참여한 결과물이다.
- 데이터셋은 드라마 '수상한 삼형제'의 1,513개 씬에서 12,289개 문장을 각각 dysphoria, euphoria, neutral 3 가지 감정 중 하나로 라벨링하여 제공되었다. 
- senetence_id, person, sentence, scene, context으로 구성되었으며 학습에는 sentence-context pair 형식으로 활용하였다. 
- Leaderboard private score : 0.70344 (13rd / 105 teams)
### Environments
- Google Colab Pro(TPU)
- torch-xla, accelerator
- cloud-tpu-client==0.10 
- torch==1.9.0

## Model description 
### Data augmentation(back translation)
- [PORORO](https://github.com/kakaobrain/pororo/blob/master/README.ko.md) 라이브러리의 translation task 활용하였다.
- 한국어 -> 영어 -> 한국어(7,339)
- 한국어 -> 일본어 -> 한국어(7,339)
```
from pororo import Pororo
mt = Pororo(task="translation", lang="multi")

def translate(text, lang):
    txt = mt(text, src='ko', tgt=lang)
    res = mt(txt, src=lang, tgt='ko')
    return res
```
### Model
- klue/roberta-large([huggingface](https://huggingface.co/klue/roberta-large?text=%EB%8C%80%ED%95%9C%EB%AF%BC%EA%B5%AD%EC%9D%98+%EC%88%98%EB%8F%84%EB%8A%94+%5BMASK%5D+%EC%9E%85%EB%8B%88%EB%8B%A4.))
- stratified 5 fold
- AdamW optimizer
- get_cosine_with_hard_restarts_schedule_with_warmup
- Focalloss(gamma=4) 
- hard voting(fold 별 학습 후 추론한 결과 voting) 

### Hyperparameter setting
```
batch_size = 16
max_len = 64
epoch = 3
learning_rate = 2e-5
weight_decay = 0.01
warmup_rate = 0.1
```
## Conclusion
- public score : 0.70655, private score : 0.70344 로 제출 파일 중 둘 사이가 가장 비슷했다. 
    - 훈련 데이터 수가 7300여 개로 적어 무조건 public score가 높다고 좋은 모델이 아니였다.
    - 기본 데이터만 가지고 학습한 경우보다 back translation 증강 후 학습한 경우가 robust했다. 
- accelerator와 torch-xla를 이용해 cloud tpu를 활용하는 방법을 익혔다. 
- context 데이터의 결측치를 공백으로 대체하여 학습하였는데 이 부분을 적절히 처리했으면 더 나은 결과가 나왔을 것으로 예상된다. 
- 클래스 불균형 해결을 위한 다양한 시도(focal loss, label smoothing, class weight, focalsmoothing, ...)를 하였으며 결과적으론 gamma를 4로 비교적 많이 준 focal loss가 가장 높은 성능을 보였다. 
