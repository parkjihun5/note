### 이번엔 파티 구성 시스템을 만들어보자

```
import game_characters

def make_party():
    # 1. 도감(CharacterList) 객체를 만듦
    # 변수 이름을 manager로 해서 클래스 이름과 확실히 구분
    manager = game_characters.CharacterList()
    party = []

    print("=== 파티 편성을 시작합니다 (최대 4명) ===")
    # 도감 안의 리스트에서 이름들만 뽑아서 보여줌
    print("사용 가능 캐릭터:", [c[0] for c in manager.all_characters])

    while len(party) < 4:
        choice = input(f"\n[{len(party)+1}/4] 파티원을 입력하세요 (완료하려면 'q' 입력): ")

        if choice.lower() == 'q':
            if len(party) >= 1: break
            else:
                print("최소 1명의 파티원이 필요합니다!")
                continue

        found = False
        # 🚩 중요: 검색은 manager(객체) 안의 all_characters(리스트)에서
        for data in manager.all_characters:
            if data[0] == choice:
                if any(p.name == choice for p in party):
                    print("이미 파티에 포함된 캐릭터입니다.")
                else:
                    # 🚩 중요: 생성은 game_characters 파일의 Character 클래스로
                    new_member = game_characters.Character(*data)
                    party.append(new_member)
                    print(f"-> {new_member.name} 합류!")
                found = True
                break
        
        if not found:
            print("목록에 없는 캐릭터 이름입니다. 다시 입력해주세요.")

    print("\n=== 파티 편성 완료! ===")
    for i, member in enumerate(party):
        print(f"{i+1}번 대원: {member.name} ({member.role})")

    return party
```