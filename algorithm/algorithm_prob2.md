## 알고리즘 문제

### 카드 게임 만들기

---
코드

from collections import deque

num = int(input())
exclude_card = []
lst = deque(range(1, num+1))

while len(lst) != 1:
    exclude_num = lst.popleft()
    exclude_card.append(exclude_num)
    num_retrun = lst.popleft()
    lst.append(num_retrun)

# if len(lst) == 1:
exclude_card.append(lst[0])

# if int(num) == 1:
#     # print(num)
#     exclude_card.append(1)

print(*exclude_card)

---
### 요세푸스 문제

코드

from collections import deque


members, sequence_mem = map(int, input().split())
game_members = deque(range(1, members + 1))
exclude_mem = []

while len(game_members) != 0 :
    game_members.rotate(-(sequence_mem-1))
    mem = game_members.popleft()
    exclude_mem.append(mem)

print("<", ", ". join(map(str, exclude_mem)), ">", sep='')

---
### 괄호

from collections import deque
T = int(input())

for _ in range(T):
    data = input()
    stack = deque()
    is_valid = True
    for s in data:
        if s == '(':
            stack.append(s)
        else:
            if stack:
                stack.pop()
            else:
                is_valid = False
                break
    
    if stack:
        is_valid = False

    if is_valid:
        print('YES')
    else:
        print('NO')

---
### 트럭

코드

from collections import deque

n, w, l = map(int, input().split())

trucks = deque(map(int, input().split()))

bridge = deque([0] * w)
weight = 0
time = 0

while trucks:
    time += 1
    exit_truck = bridge.popleft()
    weight -= exit_truck

    if weight + trucks[0] <= l:
        new_truck = trucks.popleft()
        bridge.append(new_truck)
        weight += new_truck
    else:
        bridge.append(0)
    print(time + w)

---
### 균형잡힌 세상

코드

from collections import deque

while True:
    data = input()
    
    if data == '.':
        break
    stack = deque()
    is_valid = True
    
    for s in data:
        if s == '(' or s == '[':
            stack.append(s)
        
        elif s == ')':
            if stack and stack[-1] == '(':
                stack.pop()
            else:
                is_valid = False
                break
        
        elif s == ']':
            if stack and stack[-1] == '[':
                stack.pop()
            else:
                is_valid = False
                break
    
    if stack:
        is_valid = False

    if is_valid:
        print('yes')
    else:
        print('no')