### 간단하게 게임 만들기

#우선 나는 스타레일을 좋아하니까 스타레일과 최대한 비슷하게 만들어보고 싶어
#우선 캐릭터들과 적대적 몹, 파티 구성 시스템을 만들고 메인(전투)시스템 파일을 통해 구현하면 재밌을거 같아
#캐릭터들의 파티가 존재(최소: 1명, 최대: 4명)
#캐릭터는 체력, 공격력, 속도가 있으면 좋겠어
#여러 캐릭터들 중 4개의 캐릭터를 선택해서 파티를 구성했으면 좋겠어

#캐릭터의 속도에 따라 행동 순서 확정

#캐릭터의 행동이 일반 공격, 특수 스킬, 필살기로 나눠지면 좋겠어
#필살기는 필살기 게이지가 가득 차면 사용할 수 있게 하면 좋겠어

---
#우선 캐릭터의 데이터를 집어넣자

class CharacterList:
    def __init__(self):
        # 데이터 구조: (이름, 역할, HP, ATK, SPD, 일반공격셋, 전투스킬셋, 필살기셋)
        # 스킬셋 구성: (이름, 범위, 효과, 배율) 
        self.all_characters = [
            ("비소", "메인 딜러", 2000, 500, 135, 
             ("섬렬", "single", "damage", 1.0), 
             ("꿰뚫는 도끼", "single", "damage", 1.8), 
             ("천지 격쇄", "single", "damage", 4.0)),
            
            ("단항·등황", "수호형 서포터", 2500, 200, 120, 
             ("악을 진압하고 창생을 지키리", "single", "damage", 1.0), 
             ("팔황을 품은 변화무상한 대지", "single", "shield", 1.5), 
             ("후회 없이 세상을 변화시키는 승룡", "aoe", "shield", 2.0)),
            
            ("사이퍼", "디버퍼, 서브딜러", 2000, 350, 161, 
             ("앗! 그물을 빠져나간 물고기", "single", "damage", 1.0), 
             ("헷, 공짜 보물", "single", "debuff", 0.5), 
             ("고양이 괴도 드림!", "aoe", "damage", 2.5)), 
            
            ("트리비", "버퍼", 3000, 250, 95, 
             ("100층짜리 미니 로켓", "aoe", "damage", 1.0), 
             ("선물이 다 어디 갔지?", "single", "buff", 0.0), 
             ("여기에 누가 살고 있게?", "aoe", "buff", 0.0))
        ]

class Character:
    def __init__(self, name, role, hp, atk, spd, n_data, b_data, u_data):
        # 기본 정보 설정
        self.name = name
        self.role = role
        self.hp = hp
        self.max_hp = hp
        self.atk = atk
        self.spd = spd
        self.shield = 0
        
        # 🚩 추가: 궁극기 스택 관리 (전투 중 0~3 사이로 변화)
        self.ult_charge = 0
        
        # 스킬 데이터 언패킹 (각 튜플에서 이름, 범위, 효과, 배율 추출)
        self.n_name, self.n_scope, self.n_effect, self.n_mult = n_data
        self.b_name, self.b_scope, self.b_effect, self.b_mult = b_data
        self.u_name, self.u_scope, self.u_effect, self.u_mult = u_data