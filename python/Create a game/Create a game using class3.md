### 이번엔 적대적 몹에 대한 데이터를 만들자

```
class EnemyList:
    def __init__(self):
        self.all_enemies = [
            ("아이언툼", 12000, 800, 100, 
             ("피와 상처를 쏟아내며", "single", "damage", 1.0), 
             ("신국을 불태우고, 세계를 버려라", "aoe", "damage", 1.875))
        ]

class Enemy:
    def __init__(self, name, hp, atk, spd, s1_data, s2_data):
        self.name = name
        self.hp = hp
        self.max_hp = hp
        self.atk = atk
        self.spd = spd
        
        # 🚩 추가: 보스의 현재 행동 순서를 기억하는 변수 (0, 1, 2 반복)
        self.action_count = 0
        
        # 일반 스킬 (skill1)
        self.s1_name, self.s1_scope, self.s1_effect, self.s1_mult = s1_data
        # 궁극기 (skill2)
        self.s2_name, self.s2_scope, self.s2_effect, self.s2_mult = s2_data
```