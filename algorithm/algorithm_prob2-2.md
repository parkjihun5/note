### 균형잡힌 세상

코드(유지보수가 더 간편하고 코드가 깔끔하게 짜여진 좋은 사례, 어떤 괄호가 늘어나도 한줄로 해결 가능)

from collections import deque

while True:
    data = input()
    braket_dic = {
        ')' : '(',
        '}' : '{',
        ']' : '[',
        '>' : '<'
    }
    open_braket = set(braket_dic.values())
    close_braket = braket_dic.keys()

    if data == '.':
        break

    stack = deque()
    is_valid = True

    for s in data:
        if s in open_braket:
            stack.append(s)

        elif s in close_braket:
            if stack and stack[-1] == braket_dic[s]:
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