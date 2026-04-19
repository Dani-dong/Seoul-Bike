import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
from scipy import stats
import matplotlib.font_manager as fm

# 1. 환경 설정 (한글 폰트 설정)
# 시스템에 설치된 나눔폰트를 사용하거나, 경로를 지정하세요.
plt.rcParams['font.family'] = 'NanumGothic'
plt.rcParams['axes.unicode_minus'] = False

def run_analysis(data_path, fault_data_path):
    # 2. 데이터 로드
    df = pd.read_csv(data_path)
    df_fault = pd.read_csv(fault_data_path, encoding='euc-kr')

    print(f"따릉이 이용 데이터: {df.shape}")
    print(f"고장 신고 데이터: {df_fault.shape}")

    # 3. 자치구별 이용 현황 분석
    gu_rent = df.groupby('자치구')['대여건수'].sum().sort_values(ascending=False)
    
    # 시각화: 자치구별 총 대여건수
    plt.figure(figsize=(12, 6))
    gu_rent.plot(kind='bar', color='skyblue')
    plt.title('서울시 자치구별 따릉이 총 대여건수')
    plt.ylabel('대여 건수')
    plt.xticks(rotation=45)
    plt.tight_layout()
    plt.show()

    # 4. 대여/반납 불균형 분석
    gu_balance = df.groupby('자치구').agg(
        total_rent=('대여건수', 'sum'),
        total_return=('반납건수', 'sum')
    )
    gu_balance['diff'] = gu_balance['total_rent'] - gu_balance['total_return']
    print("\n[자치구별 대여-반납 차이 상위 5개구]")
    print(gu_balance.sort_values('diff', ascending=False).head())

    # 5. 가설 검정: 역세권 대여소의 불균형 분석
    # 가설: 대여소명에 '역'이 포함된 곳은 대여/반납의 차이(불균형)가 더 클 것이다.
    
    df['diff_abs'] = (df['대여건수'] - df['반납건수']).abs()
    df['is_station'] = df['대여소명'].str.contains('역').map({True: '역세권', False: '비역세권'})

    # 통계량 확인
    station_stats = df.groupby('is_station')['diff_abs'].mean()
    print("\n[역세권 vs 비역세권 대여반납차이 평균]")
    print(station_stats)

    # T-test 검정
    station_group = df[df['is_station'] == '역세권']['diff_abs']
    non_station_group = df[df['is_station'] == '비역세권']['diff_abs']
    t_stat, p_val = stats.ttest_ind(station_group, non_station_group)

    print(f"\n검정 결과 (T-test):")
    print(f"T-statistic: {t_stat:.4f}, P-value: {p_val:.4f}")

    if p_val < 0.05:
        print("=> 결과: 통계적으로 유의미한 차이가 있음. 역세권 대여소의 불균형이 더 심함.")
    else:
        print("=> 결과: 통계적으로 유의미한 차이가 없음.")

    # 시각화: 역세권 여부에 따른 불균형 비교
    plt.figure(figsize=(8, 5))
    sns.barplot(data=df, x='is_station', y='diff_abs', palette='muted')
    plt.title('역세권 여부에 따른 대여/반납 불균형 비교')
    plt.show()

if __name__ == "__main__":
    # 데이터 파일 경로 설정 (본인의 환경에 맞게 수정)
    RENT_DATA = '따릉이_분석용.csv'
    FAULT_DATA = '따릉이_고장신고 내역.csv'
    
    run_analysis(RENT_DATA, FAULT_DATA)
