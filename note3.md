## 알고리즘 문제 풀어보기

---
### 제거된 수 찾기

def solution(numbers):
    answer = 0
    
    new_number = range(1, numbers[-1]+1)
    if len(numbers) == numbers[-1]:
        answer = numbers[-1] + 1
    else:
        exclude_num = list(set(new_number) - set(numbers))
        answer = exclude_num[0]

    return answer

---
### 팰린드롬 수

def solution(n):
    answer = True
    max_num = 100000000
    while n <= max_num:
        n_list = list(map(int, str(n)))
        if n_list[0:] == n_list[::-1]:
            answer = True

        if n_list[0:] != n_list[::-1]:
            answer = False
        return answer

---
### 콜라츠 규칙

def solution(n):
    answer = 0
    while n != 1:
        answer += 1
        if n % 2 == 0:
            n = n // 2
        else:
            n = n * 3 + 1
    return answer

---
### 최소 동전 거스름돈

1. first build

def solution(change):
    answer = 0
    total = 0

    total += change // 500
    total += change % 500 // 100
    total += change % 500 % 100 // 50 
    total += change % 500 % 100 % 50 // 10
    total += change % 500 % 100 % 50 % 10 // 5
    total += change % 500 % 100 % 50 % 10 % 5 // 1

    answer = total

    return answer



2. second build

def solution(change):
    answer = 0
    coins = [500, 100 , 50, 10, 5, 1]

    for coin in coins:
        if change >= coin:
            answer += (change // coin)
            change -= (coin * (change // coin))
    return answer