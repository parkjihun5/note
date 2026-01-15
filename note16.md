## 자판기를 만들어볼거에요

#처음에 현금인지 카드인지를 선택하고
#만약 카드라면 먼저 카드를 단말기에 꼿게 하고 현금이라면 금액을 먼저 투입하게 해
#만약 돈을 넣었는데 갑자기 사기가 싫어졌을 수도 있으니 결제 취소 버튼도 있으면 좋겠어
    #만약 카드일 경우 "카드를 뽑아주세요"를 출력하고 현금일 경우 투입된 금액만큼 최소개수의 동전으로 돌려주면 좋겠어(ex. 투입금: 1,000원, 반환금: 500원 2개)
#음료를 선택할 때 남은 재고가 없는 음료는 품절 표시가 되어 있었으면 좋겠어
#뽑을 음료수를 선택해
    #음료를 선택할 때는 음료들의 리스트를 쫙 나열해주고 그 나열한 리스트에서 선택할 수 있는 것과 없는 것, 품절 등이 표시되면 좋겠어
#만약 현금일 때 자판기에 넣은 액수보다 선택한 음료의 가격이 비싸다면 고를 수 없게 해
#음료를 선택했다면 선택한 음료를 뽑아내
#만약 현금이라면 음료를 뽑아낸 후 자판기에 넣은 액수에서 음료의 가격만큼 제외하고 500원과 100원으로 최소한의 동전을 줘(ex. 잔액: 600이라면 500원짜리 1개, 100원짜리 1개)

---
### 우선 데이터를 넣어보자

class Product:
    def __init__(self, name, price, stock):
        self.name = name
        self.price = price
        self.stock = stock

class ProductList:
    def __init__(self):
        # 원본 데이터 (이름, 가격, 재고)
        raw_data = [
            ("코카콜라", 1500, 10),
            ("펩시콜라", 1300, 7),
            ("칠성사이다", 1400, 0),
            ("스프라이트", 1200, 6),
            ("환타 파인애플", 1400, 9),
            ("환타 포도", 1400, 4),
            ("이프로", 1500, 11),
            ("제티", 1000, 3)
        ]

        self.all_products = [Product(*data) for data in raw_data]

---
### 다음은 자판기 자체의 로직을 만들어보자

import products
import time

def start_vending_machine():
    plist = products.ProductList()
    all_items = plist.all_products
    
    # --- [STEP 1: 메뉴 먼저 보여주기] ---
    print("\n" + "="*50)
    print("          🥤 환영합니다! 자판기 메뉴판 🥤          ")
    print("="*50)
    for i, prod in enumerate(all_items, 1):
        # 돈을 넣기 전이므로 품절 여부만 체크
        status = "🚩 [품절]" if prod.stock <= 0 else "✅ [판매중]"
        print(f"{i}. {prod.name:<10} | {prod.price:>5}원 | {status}")
    print("="*50)

    # --- [STEP 2: 결제 수단 및 금액 투입] ---
    print("\n원하시는 음료가 있다면 결제 수단을 선택해 주세요.")
    pay_choice = input("1: 현금 결제 | 2: 카드 결제 | q: 종료\n선택: ")
    
    if pay_choice.lower() == 'q': return
    
    current_money = 0
    is_card = False
    
    if pay_choice == '1':
        try:
            current_money = int(input("\n💵 현금을 투입해 주세요: "))
        except ValueError:
            print("❌ 숫자만 입력 가능합니다.")
            return
    elif pay_choice == '2':
        print("\n💳 카드를 단말기에 꽂아주세요...")
        time.sleep(1)
        print("✅ 카드 인증 완료! (한도 무제한)")
        is_card = True
    else:
        return

    # --- [STEP 3: 실제 구매 단계] ---
    while True:
        print(f"\n" + "-"*50)
        print(f" [ 이용 중 ] " + (f"잔액: {current_money}원" if not is_card else "카드 결제 모드"))
        print("-"*50)
        
        for i, prod in enumerate(all_items, 1):
            # 이제는 내 잔액에 맞춘 실시간 상태 표시
            if prod.stock <= 0: status = "🚩 [품절]"
            elif not is_card and current_money < prod.price: status = "⚠️ [잔액부족]"
            else: status = "🛒 [구매가능]"
            print(f"{i}. {prod.name:<10} | {prod.price:>5}원 | {status}")
        
        print(f"0. 결제 취소 및 반환")
        print("-"*50)
        
        choice = input("구매하실 음료 번호를 선택하세요: ")
        
        if choice == '0':
            if is_card: print("\n💳 카드를 뽑아주세요. 이용해 주셔서 감사합니다.")
            else:
                print(f"\n💵 {current_money}원을 반환합니다.")
                c_500 = current_money // 500
                c_100 = (current_money % 500) // 100
                print(f"🪙 반환 동전: 500원 {c_500}개, 100원 {c_100}개")
            break

        try:
            idx = int(choice) - 1
            if 0 <= idx < len(all_items):
                selected = all_items[idx]
                if selected.stock <= 0:
                    print(f"\n❌ '{selected.name}'은(는) 품절입니다.")
                elif not is_card and current_money < selected.price:
                    print(f"\n❌ 잔액이 부족합니다.")
                else:
                    print(f"\n🥤 '{selected.name}' 배출 완료! 이용해 주셔서 감사합니다.")
                    selected.stock -= 1
                    if not is_card:
                        current_money -= selected.price
                        # 구매 후 남은 돈 반환
                        c_500 = current_money // 500
                        c_100 = (current_money % 500) // 100
                        print(f"🪙 잔돈 반환: 500원 {c_500}개, 100원 {c_100}개")
                    break
            else: print("\n❌ 잘못된 번호입니다.")
        except ValueError: print("\n❌ 숫자를 입력해 주세요.")

if __name__ == "__main__":
    start_vending_machine()