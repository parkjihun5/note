### 그리고 앞의 두 파일을 import해서 자판기를 만들어

import products2
import vending_sys  # 계산 엔진 임포트
import time

class VendingMachine:
    def __init__(self):
        # 자판기가 직접 상품 객체 리스트를 소유
        raw_data = [
            ("코카콜라", 1500, 10), ("펩시콜라", 1300, 7),
            ("칠성사이다", 1400, 0), ("스프라이트", 1200, 6),
            ("환타 파인", 1400, 9), ("환타 포도", 1400, 4),
            ("이프로", 1500, 11), ("제티", 1000, 3)
        ]
        self.inventory = [products2.Product(*data) for data in raw_data]
        self.money = 0
        self.is_card = False

    def show_menu(self):
        print("\n" + "="*50)
        print(f"{'🥤 자판기 메뉴판 🥤':^44}")
        print("="*50)
        for i, prod in enumerate(self.inventory, 1):
            # 엔진에게 상태 물어보기
            status = vending_sys.get_item_status(prod, self.money, self.is_card)
            print(f"{i}. {prod.name:<10} | {prod.price:>5}원 | {status}")
        print("0. 결제 취소 및 반환")
        print("="*50)

    def process_return(self):
        if self.is_card:
            print("\n💳 카드를 뽑아주세요. 감사합니다.")
        else:
            # 엔진에게 동전 개수 물어보기
            coins = vending_sys.calculate_coins(self.money)
            print(f"\n💵 {self.money}원 반환 -> 500원:{coins['500']}개, 100원:{coins['100']}개")
            self.money = 0

    def run(self):
        self.show_menu()
        pay = input("\n결제 선택 (1:현금, 2:카드, q:종료): ")
        if pay == '1': self.money = int(input("💵 입금: "))
        elif pay == '2': 
            print("💳 인증 중..."); time.sleep(1); self.is_card = True
        else: return

        while True:
            self.show_menu()
            choice = input("번호 선택: ")
            if choice == '0': self.process_return(); break
            
            try:
                idx = int(choice) - 1
                if 0 <= idx < len(self.inventory):
                    target = self.inventory[idx]
                    status = vending_sys.get_item_status(target, self.money, self.is_card)
                    
                    if "✅" in status:
                        print(f"\n🥤 {target.name} 나왔습니다!")
                        # 엔진에게 결제 처리 맡기기
                        self.money = vending_sys.execute_payment(target, self.money, self.is_card)
                        self.process_return()
                        break
                    else: print(f"\n❌ {status}")
                else: print("\n❌ 번호 오류")
            except ValueError: print("\n❌ 숫자 입력")

if __name__ == "__main__":
    machine = VendingMachine()
    machine.run()