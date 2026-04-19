# 🚲 서울시 따릉이 데이터 분석: 역세권 대여소는 자전거 쏠림 현상이 더 심할까?

## 💡 프로젝트 소개
서울시 공공자전거 '따릉이'의 이용 내역 데이터를 바탕으로 대여소별 이용 현황을 파악하고, 특정 지리적 요인(지하철역 인접 여부)이 자전거의 대여/반납 불균형에 미치는 영향을 분석한 미니 프로젝트입니다.

* **사용 언어 및 라이브러리:** Python, Pandas, Matplotlib, Seaborn, Scipy
* **주요 분석 내용:** 1. 자치구별 및 대여소별 이용량 파악
    2. 대여건수와 반납건수의 차이(불균형) 분석
    3. 가설 검증: 역세권 대여소와 비역세권 대여소의 쏠림 현상 통계적 비교

---

## 🛠 1. 환경 설정 및 데이터 준비

matplotlib 시각화 시 한글 폰트가 깨지는 현상을 방지하기 위해 나눔고딕 폰트를 설치하고 적용했습니다.

```python
import pandas as pd
import matplotlib.pyplot as plt
import matplotlib.font_manager as fm
import seaborn as sns

# 나눔 폰트 설치 및 설정 (Colab 환경)
!apt-get -qq install fonts-nanum
!rm -rf ~/.cache/matplotlib

fe = fm.FontEntry(
    fname=r'/usr/share/fonts/truetype/nanum/NanumGothic.ttf',
    name='NanumGothic')
fm.fontManager.ttflist.insert(0, fe)
plt.rcParams.update({'font.size': 12, 'font.family': 'NanumGothic'})
plt.rcParams['axes.unicode_minus'] = False
