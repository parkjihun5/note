## 행열 연습하기

mat = [
    [1, 2, 3, 4],
    [5, 6, 7, 8],
    [9, 10, 11, 12],
    [13, 14, 15, 16]
]

---
### mat[i][j] 형태 사용해보기
n = len(mat)
m = len(mat[0])
i = 0
j = 0

for i in range(n):
    for j in range(m):
        print(mat[i][j])

---
### 세로로 출력해보기
n = len(mat)
i = 0
j = 0

for j in range(n):
    for i in range(n):
        print(mat[i][j])

---
### 세로값 더해서 출력해보기
n = len(mat)
i = 0
j = 0

for j in range(n):
    total = 0
    for i in range(n):
        total += mat[i][j]
    print(total)

---
### 세로가 n, 가로가 m인 2차원배열 만들기
mat = []
num = 0
n = 0
m = 0

for _ in range(n):
    lst = []
    for _ in range(m):
        num += 1
        lst.append()
    mat.append()
print(mat)

---
## 상속 연습하기

### 강아지, 고양이 울음소리 내기
class animal:
    def __init__(self, name):
        self.name = name
    
    def move(self):
        print(f"{self.name}이 이동합니다")

class dog(animal):
    def bark(self):
        print(f"{self.name}가 멍멍 짖습니다")

class cat(animal):
    def mew(self):
        print(f"{self.name}가 냥냥 웁니다")

d = dog("강아지")
c = cat("고양이")

d.move()
d.bark()
c.move()
c.mew()

---
### 핸드폰 부팅 시키기

class Electronics:
    def __init__(self, brand):
        self.brand = brand

    def power_on(self):
        print(f"{self.brand}의 전원이 켜집니다")

class Smartphone(Electronics):
    def __init__(self, brand, model):
        super().__init__(brand)
        self.model = model
    
    def power_on(self):
        super().power_on()
        print(f"모델 {self.model}이 부팅을 시작합니다")

class Fan(Electronics):
    def power_on(self):
        super().power_on()
        print(f"바람이 붑니다")

phone = Smartphone("삼성", "폴드7")
fan = Fan("삼성팬")
    
phone.power_on()
fan.power_on()