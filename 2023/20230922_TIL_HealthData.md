# HealthData

## 배운점
Health 데이터를 받아오기 위한 순서
1. 헬스 데이터 타입별 권한 요청
2. 쿼리 구성
3. 쿼리 실행
4. 데이터 UI에 반영 


HKStatisticsCollectionQuery는 AnchorDate와 Timetreval을 이용해 고정된 기간 안의 여러 HKStatistics를 얻을 수 있고, updateHandler를 통해서 업데이트된 Health 데이터를 수신할 수 있다.

HKStatisticsCollectionQuery를 사용하면 차트를 그리기 좋게 데이터를 받아올 수 있다.


## 작업
HealthKit에는 많은 종류의 건강 데이터들이 있고 애플워치를 사용하면 더 다양한 종류의 데이터를 얻을 수 있다.
그 중에서 휴식에너지와 활동에너지를 가져와서 HKStatisticsCollectionQuery를 사용하여 소모되는 칼로리에 대한 그래프를 그리는 작업을 했다. 
HKStatisticsCollectionQuery 쿼리 외에도 수많은 쿼리가 있다. 쿼리들을 적절히 사용하여 적재적소에 적용할 수 있을 것 같다.
