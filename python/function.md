함수란 일종의 블랙박스 같은 느깜
함수는
def 함수명(매개변수):
    함수에서 실행되는 동작1
    동작2
    ..
    return 반환값으로 구성된다

    ex.
    def get_grade_result(score):
    if score > 100 or score < 0:
        return '잘못된 점수'
    elif score >= 60:
        return '합격'
    else:
        return '불합격'

print(get_grade_result(85))  
print(get_grade_result(40))  
print(get_grade_result(120)) 

*조건문의 순서는 조건의 범위가 작은 조건부터 작성하기
*조건의 범위가 큰 조건부터 들어가면 아래 값을 확인하지 못함

함수를 사용하는 이유
1. 코드의 재사용성: 동일한 동작을 하는 코드를 중복으로 작성하지 않고 재사용한다.
2. 가독성: 전체적인 코드가 깔끔해지고 기능 파악이 유용하다
3. 유지보수: 동작 변경 시 함수 내부 코드만 수정하면 okay

함수의 선언과 호출
선언(define): def 키워드로 함수를 작성
호출(call): 함수명() 형식으로 함수를 실행

매개변수와 인자
매개변수(parameter): 입력값을 받는 변수
인자(argument): 함수를 호출할 시 넘기는 실제 값

자주 사용하는 내장 함수
abs: 절댓값
sum: 합계
min, max: 최소/최대값
sorted: 오름차순 정렬 (새 리스트 반환)
map: 각 원소에 함수 적용