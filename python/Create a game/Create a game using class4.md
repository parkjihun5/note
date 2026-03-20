### 이제 전투(메인)로직을 만들어보자

```
import game_characters
import game_party
import game_enemy
import random

def start_battle():
    # 1. 초기 세팅
    party = game_party.make_party()
    if not party: return # 파티가 없으면 종료
    
    enemies = game_enemy.EnemyList()
    boss = game_enemy.Enemy(*enemies.all_enemies[0])

    print(f"\n" + "🔥" * 20)
    print(f" 보스 [{boss.name}]이 나타났다! ")
    print("🔥" * 20)

    round_cnt = 1
    
    # 2. 메인 전투 루프
    while boss.hp > 0 and any(p.hp > 0 for p in party):
        print(f"\n\n[ ROUND {round_cnt} ]")
        
        # 속도 기준 정렬 (턴 순서 결정)
        combatants = [p for p in party if p.hp > 0] + [boss]
        combatants.sort(key=lambda x: x.spd, reverse=True)

        for unit in combatants:
            # 보스가 죽었거나 아군이 전멸했는지 실시간 체크
            if boss.hp <= 0 or not any(p.hp > 0 for p in party): break
            if unit.hp <= 0: continue

            # --- [아군 유닛의 턴] ---
            if isinstance(unit, game_characters.Character):
                stack_bar = "●" * unit.ult_charge + "○" * (3 - unit.ult_charge)
                print(f"\n-- {unit.name}의 차례 (HP: {unit.hp:.0f} | 쉴드: {unit.shield:.0f} | 기력: {stack_bar}) --")
                print(f"1) [일반] {unit.n_name} (+1 스택)")
                print(f"2) [전투] {unit.b_name} (+1 스택)")
                ult_ready = " [사용 가능!]" if unit.ult_charge >= 3 else " [차징 중...]"
                print(f"3) [궁극] {unit.u_name} {ult_ready}")

                while True:
                    choice = input("명령을 내리세요 (1-3): ")
                    if choice == '1':
                        s_name, s_scope, s_effect, s_mult = unit.n_name, unit.n_scope, unit.n_effect, unit.n_mult
                        unit.ult_charge = min(3, unit.ult_charge + 1)
                        break
                    elif choice == '2':
                        s_name, s_scope, s_effect, s_mult = unit.b_name, unit.b_scope, unit.b_effect, unit.b_mult
                        unit.ult_charge = min(3, unit.ult_charge + 1)
                        break
                    elif choice == '3':
                        if unit.ult_charge >= 3:
                            s_name, s_scope, s_effect, s_mult = unit.u_name, unit.u_scope, unit.u_effect, unit.u_mult
                            unit.ult_charge = 0
                            break
                        else:
                            print("❌ 기력이 부족합니다!")
                    else:
                        print("❌ 올바른 번호를 입력하세요.")

                # 스킬 효과 처리
                print(f"\n✨ {unit.name}: {s_name}")
                
                if s_effect == "damage":
                    final_dmg = unit.atk * s_mult
                    boss.hp -= final_dmg
                    print(f"💥 {boss.name}에게 {final_dmg:.0f}의 피해! (남은 HP: {max(0, boss.hp):.0f})")
                
                elif s_effect == "shield":
                    # 수호형 서포터 특수 로직: 공격력의 2배
                    shield_val = unit.atk * 2
                    for p in party:
                        if p.hp > 0: p.shield += shield_val
                    print(f"🛡️ 모든 아군에게 {shield_val:.0f}의 방어막 부여!")

                elif s_effect == "buff":
                    # 버퍼 특수 로직: 공격력 1.1배 (상시 중첩 가능하도록 설정)
                    for p in party:
                        if p.hp > 0: p.atk *= 1.1
                    print(f"🔺 모든 아군의 공격력이 1.1배 상승했습니다!")

                elif s_effect == "debuff":
                    # 디버퍼 특수 로직: 적 속도 0.1배 감소 (현재 속도의 90%로)
                    boss.spd *= 0.9
                    print(f"🌀 {boss.name}의 속도 감소! (현재 속도: {boss.spd:.1f})")

            # --- [보스의 턴] ---
            else:
                target = random.choice([p for p in party if p.hp > 0])
                
                # 보스 패턴: action_count가 2이면 궁극기, 아니면 일반기
                if boss.action_count < 2:
                    s_name, s_mult = boss.s1_name, boss.s1_mult
                    boss.action_count += 1
                    print(f"\n👿 {boss.name}의 일반 공격: '{s_name}'!")
                else:
                    s_name, s_mult = boss.s2_name, boss.s2_mult
                    boss.action_count = 0
                    print(f"\n🔥 {boss.name}의 필살기: '{s_name}'!!!")

                # 데미지 계산 및 쉴드 로직
                raw_dmg = boss.atk * s_mult
                if target.shield >= raw_dmg:
                    target.shield -= raw_dmg
                    actual_dmg = 0
                else:
                    actual_dmg = raw_dmg - target.shield
                    target.shield = 0
                    target.hp -= actual_dmg
                
                print(f"💥 {target.name}에게 {raw_dmg:.0f} 피해! (쉴드 흡수 후 실 피해: {actual_dmg:.0f})")
                if target.hp <= 0:
                    print(f"💀 {target.name}이(가) 쓰러졌습니다...")

        round_cnt += 1

    # 3. 전투 결과
    if boss.hp <= 0:
        print(f"\n🏆 축하합니다! {boss.name}을(를) 쓰러뜨렸습니다")
    else:
        print(f"\n💀 {boss.name}의 힘 앞에 파티가 전멸했습니다... 다시 도전하세요.")

if __name__ == "__main__":
    start_battle()
```