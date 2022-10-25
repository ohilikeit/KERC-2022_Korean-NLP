![image](https://user-images.githubusercontent.com/37128004/197667144-14df50d1-e2b5-415a-904d-b29e8ef9989d.png)
# KERC 2022
## Introduction
전남대학교 인공감정지능 기초연구실 주최의 제 4회 한국인 감정인식 국제경진대회에 참여한 결과물이다.
- 데이터셋은 드라마 '수상한 삼형제'의 1,513개 씬에서 12,289개 문장을 각각 dysphoria, euphoria, neutral 3 가지 감정 중 하나로 라벨링하여 제공되었다. 
- senetence_id, person, sentence, scene, context으로 구성되었으며 학습에는 sentence-context pair 형식으로 활용하였다. 
### Environments
- Google Colab Pro(TPU)
- torch-xla, accelerator
- cloud-tpu-client==0.10 
- torch==1.9.0

## Model description 
### Model
- klue/roberta-large
- stratified 5 fold
- AdamW optimizer
- get_cosine_with_hard_restarts_schedule_with_warmup
- Focalloss(gamma=4) 
- soft voting(fold 별 학습 후 추론한 결과 앙상블) 

### Hyperparameter setting

    batch_size = 16
    max_len = 64
    epoch = 3
    learning_rate = 2e-5
    weight_decay = 0.01
    warmup_rate = 0.1

### Structure





