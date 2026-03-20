## 똑같은 조건이지만 조금 더 깔끔하게 만들어보고 싶어

### 우선 파일에 상품을 넣어

class Product:
    def __init__(self, name, price, stock):
        self.name = name
        self.price = price
        self.stock = stock

---
### 그리고 자판기 시스템을 구성해서 다른 파일에 넣어

def get_item_status(product, money, is_card):
    """현재 제품을 살 수 있는지 상태 문자열 반환"""
    if product.stock <= 0:
        return "🚩 [품절]"
    if not is_card and money > 0 and money < product.price:
        return "⚠️ [잔액부족]"
    return "✅ [구매가능]"

def calculate_coins(amount):
    """금액을 받아서 500원과 100원의 개수를 딕셔너리로 반환"""
    c_500 = amount // 500
    c_100 = (amount % 500) // 100
    return {"500": c_500, "100": c_100}

def execute_payment(product, money, is_card):
    """재고를 깎고 남은 돈을 계산해서 반환"""
    product.stock -= 1
    if is_card:
        return 0
    return money - product.price