![image](https://user-images.githubusercontent.com/37128004/197667144-14df50d1-e2b5-415a-904d-b29e8ef9989d.png)
# KERC 2022
## Introduction
전남대학교 인공감정지능 기초연구실 주최의 제 4회 한국인 감정인식 국제경진대회에 참여한 결과물이다.
- 데이터셋은 드라마 '수상한 삼형제'의 1,513개 씬에서 12,289개 문장을 각각 dysphoria, euphoria, neutral 3 가지 감정 중 하나로 라벨링하여 제공되었다. 
- senetence_id, person, sentence, scene, context으로 구성되었으며 학습에는 sentence-context pair 형식으로 활용하였다. 

## Model description 
### Model
- klue/roberta-large
- stratified 5 fold
- Focalloss(gamma=4) 
- soft voting 

### Hyperparameter setting

    batch_size = 16
    max_len = 64
    epoch = 3
    learning_rate = 2e-5
    weight_decay = 0.01
    warmup_rate = 0.1

### Structure


sentence_id	sentence	context	label
0	1	야! 전화 받아. 아무리 바빠도 내전화는 받아야 되는거 아냐? 약속 하나도 못지키는...		dysphoria
1	2	우리 아무래도 안되겠다. 이게 최선인거 같애. 평생 잊지 않을게. 행복하길 바란다.	포기한듯 탁 일어서는데, 띵동 문자. 후다닥 보는 어영. 기막혀 읽어보는	dysphoria
2	3	김경사님, 아직 안가셨어요? 시간 다됐을텐데.	초조하게 시계보면서 왔다갔다 서성이는 김순경. 순찰차(경차)와서 멈추고 내리는 지구대	neutral
3	4	근무중인데 어딜가?	초조하게 시계보면서 왔다갔다 서성이는 김순경. 순찰차(경차)와서 멈추고 내리는 지구대	dysphoria
4	5	다녀오세요. 이런날은 무조건 가서 축하해주셔야죠. 이순경이 대신 근무선다고 나온대요.	초조하게 시계보면서 왔다갔다 서성이는 김순경. 순찰차(경차)와서 멈추고 내리는 지구대	euphoria
...	...	...	...	...
7334	12285	종남이 맛있는것도 사주고 잘 챙겨줘.		euphoria
7335	12286	걱정마라. 먹어도 같이 먹고 굶어도 같이 굶는다.		euphoria
7336	12287	아빠. 우리 저거 타.		euphoria
7337	12288	나 못타는데. 한번도 안타봤어. 툭하면 차멀미까지 하는데, 봐라, 너도 내몸땡이에 ...		euphoria
7338	12289	여기 꼼짝말고 있어. 어디 가면 안돼.	시간경과. 놀이기구 타는 건강종남. 사람들 비명. 신난 종남 소리지르고. 건강 하얗...	euphoria


