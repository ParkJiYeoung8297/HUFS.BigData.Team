# HUFS.BigData.Team

요약
1. 분석 동기 및 목적
: 한국외대 등교하는 학생들의 등교를 편리하게 하기 위한 교통 정보 제공

    1)시간대별 교통 변화    (1시간 단위, 5분 단위)

    2)8시 50분 학교 도착을 위한 다양한 경로에서의 최소한의 출발 시간대 도출

2. 데이터셋 : 교통데이터 
  1) 고속도로
  - 고속도로 콘존 및 차로유형별 교통 소통 데이터(5분 단위) :https://www.bigdata-transportation.kr/frn/prdt/detail?prdtId=PRDTNUM_000000000007
  - 경기도 교통정보 센터 (1시간 단위) : https://gits.gg.go.kr/gtdb/web/trafficDb/trafficVolume/alwaysTrafficVolumeHighway.do
  
  2) 일반도로
  - 국가교통정보센터 (5분 단위) : https://its.go.kr/

3. 데이터 분석
교통 데이터 수집 -> 데이터 전처리 -> 탐색적 데이터 분석(요일별, 월별, 도로별, 시간대별) -> 각 지점 통과시간 계산하는 알고리즘 -> 결론 및 시각화
자세한 것은 발표자료 및 보고서 참고

4. 결론
학교에 늦지 않게 도착하기 위해서는 7시 20분 전에 마지막 차를 배차하여야 한다


<br/>
<br/>

# 한국외국어대학교 등교 셔틀 교통 분석과 대안 제시
### 1. 분석 동기 및 목적


<img src="https://github.com/user-attachments/assets/4640093d-5957-4db9-bfe0-aa4c71414aff" width="80%" height="80%"/>

### 문제점

1. 한국외국어대학교 수업 시간 변경 (09:30 → 09:00)
2. 셔틀 입석 금지 

이 분석을 시작한 이유는 수업시간 변경으로 인해 **셔틀버스가 수업시간에 늦는 경우**가 발생했고,

그 와중에 입석도 금지되면서 셔틀이 만차가 되어 **학생들이 탑승하지 못하는 경우**가 발생하였기 때문이다. 

## 몇시에 출발해야할까?

따라서 등교하는 학생들이 언제 출발해야 할지, 언제 가장 차가 막히는지 등의 정보를 제공하기위해 이 프로젝트를 시작하였다. 

주제 : 
## 한국외대 학생들의 등교를 편리하게 하기 위한 교통 정보 제공

### 2. 프로젝트 범위 설정

1)분석대상

<img src="https://github.com/user-attachments/assets/5d3a7e0d-8e21-4617-bc8f-2bcf38148fc7" width="60%" height="60%"/>

우선 분석 대상을 잠실, 천호, 하남 노선을 선정하였다. 선정한 이유는 다음과 같다.

1. 가장 많은 학생들이 이용
2. 정착 지점이 많아 셔틀 경로 파악이 쉬움
   
<br/>

2)분석기간

분석기간은 방학을 제외한 학기중으로 설정하였다.   

<br/>

3)분석 목표

목표는 다음과 같이 설정하였다.

1. 시간대별 셔틀 구간의 교통체증 파악
2. 지각 이전에 도착할 수 있는 셔틀 출발 시간 제안 

<br/>

4)역할분담

일반도로와 고속도로로 역할을 분담하여 총 2명이서 프로젝트를 진행하였다.

<br/>

## 3. 분석 및 결론

1)경로 추적

우선 구글 지도에서 정차지점의 위도 경도를 찾아서 잠실, 천호 셔틀의 경로를 땄다. 

고속도로는 **하남 IC, 동서울 TG, 산곡 JC, 경기광주 IC** 총 4개의 구간을 지난다.

<br/>

2)데이터 수집

고속도로 명을 기준으로 5분 단위와 1시간 단위의 데이터를 수집하였다. 참고한 데이터 출처는 아래와 같다.

- 고속도로 콘존 및 차로유형별 교통 소통 데이터(5분 단위) :https://www.bigdata-transportation.kr/frn/prdt/detail?prdtId=PRDTNUM_000000000007
- 경기도 교통정보 센터 (1시간 단위) : https://gits.gg.go.kr/gtdb/web/trafficDb/trafficVolume/alwaysTrafficVolumeHighway.do

<aside>

**콘존** : IC(나들목), JC(분기점), 그리고 TG(요금소) 등 통행하는 차량수가 일정한 고속도로 구간

**노드** : 차량이 도로를 주행함에 있어 속도의 변화가 발생되는 곳을 표현한 곳

</aside>

콘존과 노드의 정의가 위와 같이 달라 콘존은 교통량을 보여주고, 노드는 통행 속도를 보여준다.

<br/>

3)데이터 전처리

5분 데이터와 1시간 데이터를 전처리하여 구간명을 기준으로 통합하였다.

<br/>

4)교통시간 계산 알고리즘

출발 시간을 넣으면 도착시간과 총 소요 시간을 반환하는 알고리즘을 생성하였다.

해당 지점을 통과하는 시간의 도로 평균 속도와 도로 길이를 가져와 다음 지점을 통과하는 시간을 계산한다. 

```python
'''
minute_cal()은 통과시간을 계산하는 함수이다. 
만약 link id가 2340010108인 지점을 6:08분에 지난다면 표에서 6:05에 측정된 속도를 가져와 계산한다.
진입시간을 입력값으로 받아 걸린시간을 계산하고 링크를 빠져나가는 시간을 return한다.

recall fun()은 다음과 같이 나온 결과를 다음 minute_cal함수에 넣어주는 재귀함수이다.
start=605   #시작 6:05부터
a=minute_cal(start,2340008000)
b=minute_cal(a,2340010105)
c=minute_cal(b,2340010107)
d=minute_cal(c,2340010108)

print(f'출발 시간 : {start}, 도착 시간 : {d:.2f} , 총 소요시간 : {(d-start):.4f}')
'''

def minute_cal(t,link_id): #시간을 입력받음
    n=h_road[h_road['LINK_ID']==link_id].index[0]
    time_five=605+int(((t-605)//10)*10)  #start는 속도를 찾기위한 10의 배수 시간
    if time_five%100>=60:  
        time_five=(time_five//100)*100+100+int(time_five%100-60)
    #print(time_five)   #재귀함수 돌때마다 영역별 속도 구간 ex)time_five=605 이면 6:05분 기준 속도 사용
    moving_time=((h_road.iloc[n,1]/road_speed[road_speed['LINK_ID']==link_id].loc[time_five][1]))*60   #거리/속력=걸린 시간
    t=t+moving_time   #총 시간
    #print(t)  #재귀함수 돌때마다 총시간 print
    return t
    
    
def recal_fun(total_time,linkID,i):  #재귀함수 
    if i==88:
        linkid=h_road.loc[i-1][0]
        return minute_cal(total_time,linkID)
    else:
        total=minute_cal(total_time,linkID)
        linkid=h_road.loc[i][0]
        i=i+1
        return recal_fun(total,linkid,i)
        
start=850   #시작 6:05부터
i=1
lid=2340008000
final=recal_fun(start,lid,i)
print('회안도로')
print(f'출발 시간 : {start}, 도착 시간 : {final:.2f} , 총 소요시간 : {(final-start):.4f}')
```

**결과**

<img src="https://github.com/user-attachments/assets/aa5449d4-17f4-4ea3-b8c9-ef1413916b84" width="50%" height="50%"/>

위의 알고리즘을 통해 각 도로별 소요시간을 구할 수 있다.

<br/>

5)데이터 분석 및 결론

<img src="https://github.com/user-attachments/assets/44dfa29f-acce-4863-899b-a57f021759ff" width="50%" height="50%"/>

![image](https://github.com/user-attachments/assets/be5ea687-84f6-46c0-b5bb-0bddac6af503)

속도랑 교통량은 반비례하기 때문에 교통량이 없는 링크는 속도를 통해 교통량을 유추할 수 있다.

하남 IC-동서울TG는 7시 20분, 동서울 TG-산곡JC는 7시40분, 산곡 JC-경기광주는 8시에 최대 교통체증을 가진다. 

<img src="https://github.com/user-attachments/assets/5d26d8d8-e376-4826-b3b0-061e2c70734d" width="50%" height="50%"/>


학교에 도착 바로 직전 도로인 회안도로이다.

start time에서 total Duration 을 더한 값이 8시 50분을 넘지 않아야함으로 

**8시 5분이 회안도로로 진입해야하는 최대 진입 시간**이다.

<img src="https://github.com/user-attachments/assets/26bafac1-36cb-45bb-ad43-a9a8edb74362" width="40%" height="40%"/>



회안 도로에 8시 5분 전까지 도착하기 위해서는 **고속도로에 7시 55분 전까지 도착을 해야한다**

<img src="https://github.com/user-attachments/assets/6ecd208b-0aa1-4318-9ede-377af0c1d649" width="80%" height="80%"/>


일반 대로의 통과시간을 그래프로 나타내면 다음과 같다. 

대체로 7시 10분부터 본격적으로 증가하는 모습을 보이며, 가장 시간이 오래 걸리는 도로는 천호대로이다

따라서 7시 55분 고속도로 진입을 위해서는 **7시 20분 전에 마지막 차를 배차하여야 한다**.

<br/>

### 4. 한계 및 느낀점

1)한계

a)셔틀은 버스 전용 차로로 다니기 때문에, 실제적인 분석을 위해서는 추가 고려사항 필요함

b)월별, 시간별,요일별 차이가 있음을 아노바 검증을 통해 확인했으나 적용하지 못함

<br/>

2)느낀점

#실생활 적용   #데이터 수집 및 전처리의 어려움  # 데이터와 실제의 격차

내가 직접을 통학을 하면서 **통학시간이 50분~90분까지 달라져서 불편함을 느끼고 진행했던 프로젝트**였다. 내가 체감했던 시간대별 교통 체증을 구역별로, 시간별로 나누어서 데이터로 확인하니 **어느 시간대에 어느 구간을 피해서 지나가야할지 판단할 근거가 생긴 것 같다**. 프로젝트를 진행하면서 느꼈던 것은 **실제 데이터를 얻는 것이 어렵다는 것**이었고, 이를 통합하고 **전처리하는 과정에서 많은 시간이 소요**된다는 것을 느낄 수 있었다. 또한 교통 데이터와 실제 교통간의 차이가 있기 때문에 더 실제에 가까운 분석을 하기 위해서는 **더 많은 고려사항이 필요**했다. 추가적으로 다른 고려사항도 추가하여 분석을 디벨롭해보고 싶다.


### 노션으로 더 자세히보기 : https://iodized-chartreuse-9cf.notion.site/hufs-shuttle-bus
