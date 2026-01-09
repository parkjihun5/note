## 모듈(Module)
모듈이란, 특정 기능을 하는 파이썬 파일(.py) 단위로 작성한 것
모듈의 장점: 코드를 새로 작성하지 않고, 다른 모듈에 있는 코드를 불러와서 재사용 가능, 기능 단위로 코드가 분리되어 있어 유지보수에 용이

### import
모듈을 불러올 때는 import 모듈이름(.py 파일의 이름)와 같은 형식으로 작성
모듈 내의 기능을 사용할 때는 모듈이름.함수명과 같은 방식으로 작성
모듈 이름 없이, 직접 함수의 이름만으로 사용하고 싶다면 from 모듈이름 import 함수명과 같은 형식으로 작성

자주 사용하는 모듈
### math모듈
math모듈은 수학 연산을 다루는 기능을 제공

### random 모듈
random 모듈은 난수(무작위 수)를 다루는 기능을 제공

### collection 모듈
collection 모듈은 특수한 용도의 컨테이너 자료형을 제공

### itertools 모듈
itertools 모듈은 효율적인 반복 작업을 위한 도구(순열, 조합 등)을 제공



## 팀 뽑기 프로그램
---
### 우선 랜덤으로 이름을 뽑아보기
import random

names = [
    '루미나리스', '제피로스', '벨라트릭스', '카이로스', '아이테르', 
    '엘라라', '노바', '실바누스', '오리온', '프리즘', 
    '아르카나', '세레스티아', '볼텍스', '리비아', '이그니스', 
    '네뷸라', '크로노', '솔라리스', '에코', '미스틱'
]

#팀을 5개로 나눠서 한 팀에 4명씩 랜덤으로 중복없이 들어가게 만들고 싶어
random.shuffle(names)
team1 = names[0:4]
team2 = names[4:8]
team3 = names[8:12]
team4 = names[12:16]
team5 = names[16:20]

print(team1)
print(team2)
print(team3)
print(team4)
print(team5)

---
### 코드를 깔끔하게 만들어보기
import random

names = [
    '루미나리스', '제피로스', '벨라트릭스', '카이로스', '아이테르', 
    '엘라라', '노바', '실바누스', '오리온', '프리즘', 
    '아르카나', '세레스티아', '볼텍스', '리비아', '이그니스', 
    '네뷸라', '크로노', '솔라리스', '에코', '미스틱'
]

random.shuffle(names) #우선 랜덤으로 막 섞자
team_size = 4 #몇명을 한 팀으로 묶을까?
for i in range(0, len(names), team_size):
    teams = names[i:i+team_size]
    print(teams)

---
### 옵션을 더해보기
import random

names = [
    '루미나리스', '제피로스', '벨라트릭스', '카이로스', '아이테르', 
    '엘라라', '노바', '실바누스', '오리온', '프리즘', 
    '아르카나', '세레스티아', '볼텍스', '리비아', '이그니스', 
    '네뷸라', '크로노', '솔라리스', '에코', '미스틱'
]
past_teams = [] #과거의 기록들

team_size = 4 #몇명을 한 팀으로 묶을까?
while True:
    random.shuffle(names) #우선 랜덤으로 막 섞자
    past_check = False
    today_teams = [] #오늘의 값들을 담아둘 임시 보관함
    for i in range(0, len(names), team_size):
        teams = names[i:i+team_size]

        #3명 이상 중복은 쳐내
        for old_team in past_teams:
            if len(set(teams) & set(old_team)) >= 3:
                past_check = True
                break
        
        if past_check:
            break

        #중복 없으면 들어가
        today_teams.append(teams)
        
    if not past_check: 
            for team in today_teams:
                print(team) 
                past_teams.append(team) #다음번 뽑기를 위해 과거의 데이터에 저장
            break

---
더 추가했으면 좋았을 것들
---
결석자나 제외해야 하는 사람들 리스트에서 제외
남자와 여자 혹은 코딩의 경험자와 무경험자를 적절히 섞기
아직 얼굴을 모를 수 있으니 조 이름(ex. 1조, 2조)을 추가
만약 마지막 팀에 1~2명 등의 소수의 인원이 남을 경우
특정 인원과 함께 팀이 되고 싶지 않다고 했을 경우

