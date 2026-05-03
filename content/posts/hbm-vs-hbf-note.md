+++
title = "HBM vs HBF"
date = 2026-03-15T12:07:00+09:00
draft = false
+++

지식인사이드 보다가 김대식 교수님이 HBM/HBF 얘기하는걸 듣고
조금 더 궁금해져서 정리해본다.

처음엔 HBF가 HBM 다음 버전 같은 건가 했는데, 그건 좀 아닌 것 같다.

## 먼저 요약하면

**HBM = GPU 옆에 붙는 빠른 작업대**

**HBF = Flash로 만든 큰 작업대 후보**

HBM은 DRAM을 여러 층으로 쌓아서 GPU/AI 가속기 가까이에 붙여두는 고대역폭 메모리다.
이미 AI 가속기에서 많이 쓰이는 그쪽.
*(출처: [SK hynix MWC 2026](https://news.skhynix.com/mwc-2026/))*

HBF는 조금 다르다.
NAND Flash 기반인데, HBM처럼 빠르게 읽게 만들고 용량은 훨씬 크게 가져가려는 쪽이다.
SK hynix랑 Sandisk는 이걸 **HBM과 SSD 사이의 새 memory layer**라고 설명한다.
*(출처: [SK hynix·Sandisk HBF 표준화 발표](https://news.skhynix.com/sk-hynix-and-sandisk-begin-global-standardization-ofnext-generation-memory-hbf/), [Sandisk HBF Fact Sheet](https://documents.sandisk.com/content/dam/asset-library/en_us/assets/public/sandisk/collateral/company/Sandisk-HBF-Fact-Sheet.pdf))*

## HBM은 왜 빠른가

조금 더 기술적으로 보면,
HBM이 빠른 이유는 단순히 "DRAM이라서"만은 아니다.
DRAM 칩을 위로 쌓고, TSV 같은 연결을 써서 데이터가 지나가는 통로 자체를 엄청 넓게 만든다.
그래서 GPU가 계산할 때 필요한 데이터를 바로 옆에서 계속 밀어 넣어줄 수 있다.
*(출처: [SK hynix HBM3 설명](https://news.skhynix.com/sk-hynix-at-nvidia-gtc-2022-demonstrating-the-worlds-fastest-dram-hbm3/), [SK hynix HBM4 설명](https://news.skhynix.com/sk-hynix-completes-worlds-first-hbm4-development-and-readies-mass-production/))*

이쯤에서 die라는 말이 나오는데,
die는 그냥 얇게 잘라낸 반도체 칩 한 장이라고 보면 된다.
HBM은 이 DRAM die를 여러 장 위로 쌓는다.
`8H`, `12H` 같은 표현은 대충 8단, 12단으로 쌓았다는 뜻으로 보면 된다.
삼성 HBM3E 12H는 24Gb DRAM die 12장을 TSV로 쌓아서 36GB를 만든다고 설명한다.
*(출처: [Samsung HBM3E 12H 설명](https://news.samsung.com/kr/%EC%82%BC%EC%84%B1%EC%A0%84%EC%9E%90-%EC%97%85%EA%B3%84-%EC%B5%9C%EC%B4%88-36gb-hbm3e-12h-d%EB%9E%A8-%EA%B0%9C%EB%B0%9C))*

TSV는 Through Silicon Via, 말 그대로 실리콘을 관통하는 전극이다.
예전처럼 옆으로 선을 길게 빼는 게 아니라,
쌓아올린 칩 사이를 위아래로 뚫어서 연결한다.
그래서 "고층 아파트에 엘리베이터를 많이 뚫어둔 느낌"에 가깝다.
삼성은 HBM2에서 DRAM 칩에 5,000개 이상의 구멍을 뚫는 TSV 기술을 썼다고 설명했고,
SK hynix HBM3는 stack당 8,000개 이상의 TSV를 쓴다고 설명한다.
*(출처: [Samsung HBM2 양산 설명](https://news.samsung.com/kr/%EC%82%BC%EC%84%B1%EC%A0%84%EC%9E%90-%EC%84%B8%EA%B3%84-%EC%B5%9C%EC%B4%88-4%EA%B8%B0%EA%B0%80%EB%B0%94%EC%9D%B4%ED%8A%B8-hbm-d%EB%9E%A8-%EC%96%91%EC%82%B0), [SK hynix HBM3 설명](https://news.skhynix.com/sk-hynix-at-nvidia-gtc-2022-demonstrating-the-worlds-fastest-dram-hbm3/))*

그리고 HBM은 보통 GPU 안에 들어간다기보다는,
GPU 옆에 HBM stack들을 같이 올려놓고 아주 촘촘한 배선으로 연결하는 구조에 가깝다.
여기서 자주 나오는 게 interposer다.
interposer는 GPU와 HBM 사이를 이어주는 얇은 중간 판 같은 거다.
삼성 I-Cube 설명을 보면 로직 칩과 HBM 4개를 실리콘 인터포저 위에 배치해서 하나의 반도체처럼 동작하게 한다고 나온다.
*(출처: [Samsung I-Cube4 설명](https://news.samsung.com/kr/%EC%82%BC%EC%84%B1%EC%A0%84%EC%9E%90-%EC%B0%A8%EC%84%B8%EB%8C%80-%EB%B0%98%EB%8F%84%EC%B2%B4-%ED%8C%A8%ED%82%A4%EC%A7%80-%EA%B8%B0%EC%88%A0-i-cube4-%EA%B0%9C%EB%B0%9C))*

그러니까 HBM을 그림으로 외우면:

- GPU 옆에 HBM stack이 붙어 있음
- HBM stack 안에는 DRAM die가 여러 층으로 쌓여 있음
- die끼리는 TSV로 위아래 연결됨
- GPU와 HBM은 interposer 같은 중간 판을 통해 넓게 연결됨

이 구조 때문에 HBM은 "한 번에 지나갈 수 있는 차선 수"가 엄청 많아진다.
대신 비싸고, 패키징이 어렵고, 용량을 무한정 키우기도 어렵다.

## HBF는 뭐가 다른가

반대로 HBF는 "DRAM처럼 동작하는 Flash"라기보다는,
NAND Flash를 엄청 병렬로 읽어서 AI 추론에 필요한 데이터 공급량을 맞춰보려는 시도에 가깝다.
여기서 중요한 건 지연시간(latency)보다 **읽기 대역폭 + 용량**인 것 같다.
특히 LLM 추론에서는 모델 weight[^model-weight]나 KV cache[^kv-cache]처럼 계속 읽어야 하는 데이터가 커지니까,
HBM만으로 다 들고 있기엔 비싸고 용량도 빡빡해진다.
Sandisk는 HBF가 HBM 대비 8~16배 용량을 목표로 하면서, AI inference 쪽을 주요 사용처로 잡고 있다.
*(출처: [Sandisk HBF Fact Sheet](https://documents.sandisk.com/content/dam/asset-library/en_us/assets/public/sandisk/collateral/company/Sandisk-HBF-Fact-Sheet.pdf), [Sandisk HBF 설명](https://www.sandisk.com/en-gb/company/newsroom/blogs/2025/scaling-beyond-the-wall-inside-sandisks-high-bandwidth-flash-for-ai), [SK hynix TSMC Symposium 2026](https://news.skhynix.com/tsmc-technology-symposium-2026/))*

HBF도 stack 구조를 쓴다.
다만 쌓는 재료가 DRAM die가 아니라 NAND Flash 쪽이다.
Sandisk Investor Day 자료를 보면 HBF stack 안에 HBF core die들이 있고,
아래쪽에는 logic die[^logic-die]와 PHY[^phy]가 붙는 식으로 그려져 있다.
목표는 HBM처럼 GPU/CPU/TPU[^tpu] 같은 계산 칩 가까이에 붙이되,
NAND의 장점인 큰 용량을 가져오는 것이다.
*(출처: [Sandisk Investor Day 2025](https://documents.sandisk.com/content/dam/asset-library/en_us/assets/public/sandisk/corporate/Sandisk-Investor-Day_2025.pdf))*

여기서 내가 조심해야 할 포인트는,
HBF가 Flash 기반이라고 해서 그냥 SSD를 빠르게 만든 거랑은 또 다르다는 점이다.
SSD는 보통 저장장치에 가깝고, PCIe를 통해 멀리서 데이터를 가져오는 느낌이다.
HBF는 그보다 훨씬 GPU 가까운 메모리 계층을 노린다.
그래서 SK hynix가 말한 것처럼 HBM과 SSD 사이의 layer라는 표현이 제일 덜 헷갈린다.

다만 Flash는 기본적으로 DRAM보다 latency가 불리하다.
그래서 모든 일을 HBF로 바꾸겠다는 느낌은 아니고,
대량으로 읽어야 하는 데이터, 특히 inference에서 계속 참조하는 큰 데이터에 맞추는 쪽으로 이해하는 게 맞아 보인다.

## 그래서 어디에 맞나

그래서 느낌상 이렇게 나뉜다.

- **학습(training)** 쪽은 여전히 HBM이 핵심일 가능성이 커 보임.
- **추론(inference)** 쪽에서 모델이 너무 커질수록 HBF 같은 중간 계층이 의미가 생김.
- HBF가 HBM을 바로 없앤다기보다는, HBM 옆에 붙어서 "다 못 올리는 큰 데이터"를 맡는 그림에 가깝다.

내가 외울 땐 이렇게 외우는 게 제일 편함.

- **HBM = 작업대**

  지금 바로 계산할 재료 올려놓는 곳.

- **HBF = 큰 창고 겸 작업대 후보**

  많이 쌓아두고, GPU 근처에서 빠르게 꺼내 쓰려는 것.

## 내 결론

즉, 원래 외웠던 감각은 대충 맞다.

주의할 점은 하나다.
HBF는 아직 HBM처럼 완전히 굳어진 시장 카테고리라기보다는 표준화/상용화 준비 단계에 가까워 보인다.
그래서 "HBF가 HBM을 대체한다"보다는,
**HBM이 감당하기 힘든 용량 문제를 Flash 쪽에서 풀어보려는 후보** 정도로 기억하는 게 안전하겠다.

[^model-weight]: **model weight**: AI 모델이 학습하면서 얻은 숫자값들. 추론할 때도 계속 읽어와야 해서, 모델이 커질수록 메모리에 올려둘 weight도 커진다.

[^kv-cache]: **KV cache**: Transformer 모델이 이전 토큰에서 계산한 Key/Value 값을 저장해두는 캐시. 매번 처음부터 다시 계산하지 않으려고 들고 있는 중간 결과라고 보면 된다. 참고로 cash가 아니라 cache.

[^logic-die]: **logic die**: 데이터를 저장하는 셀 자체라기보다는, 그 셀들을 제어하고 외부와 연결하는 로직 회로 쪽 die라고 보면 된다.

[^phy]: **PHY**: Physical layer의 줄임말. 칩 안팎에서 실제 전기 신호로 데이터를 주고받는 인터페이스 회로 쪽이라고 보면 된다.

[^tpu]: **TPU**: Tensor Processing Unit. Google이 만든 AI 연산 가속기 이름이다. GPU처럼 행렬/텐서 계산을 빠르게 하려고 만든 계산 칩이라고 보면 된다.
