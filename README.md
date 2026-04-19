import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
from scipy import stats

# 1. 데이터 로드 및 전처리
df = pd.read_csv('따릉이_분석용.csv')
df['대여반납차이'] = (df['대여건수'] - df['반납건수']).abs() # 수급 불균형 정도 계산

# 2. 자치구별 이용량 시각화
gu_rent = df.groupby('자치구')['대여건수'].sum().sort_values(ascending=False)
plt.figure(figsize=(12, 6))
sns.barplot(x=gu_rent.index, y=gu_rent.values, palette='coolwarm')
plt.title('서울시 자치구별 따릉이 총 대여량')
plt.xticks(rotation=45)
plt.show()

# 3. 역세권 가설 검정 (T-test)
# 대여소명에 '역'이 포함된 데이터를 역세권으로 분류
df['역세권여부'] = df['대여소명'].str.contains('역').map({True: '역세권', False: '비역세권'})

station_group = df[df['역세권여부'] == '역세권']['대여반납차이']
non_station_group = df[df['역세권여부'] == '비역세권']['대여반납차이']

# 독립표본 T-검정 수행
t_stat, p_val = stats.ttest_ind(station_group, non_station_group)

print(f"역세권 평균 불균형: {station_group.mean():.2f}")
print(f"비역세권 평균 불균형: {non_station_group.mean():.2f}")
print(f"P-value: {p_val:.4f}")

# 4. 가설 검정 결과 시각화
plt.figure(figsize=(8, 5))
sns.barplot(data=df, x='역세권여부', y='대여반납차이', ci=None)
plt.title('역세권 여부에 따른 수급 불균형 비교')
plt.ylabel('대여/반납 차이의 평균')
plt.show()
