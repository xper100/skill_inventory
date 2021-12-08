# 법정구와 자치구 매칭할 수 있는 Pickle 파일 생성
## 목적

법정구와 자치구를 매칭할 수 있는 딕셔너리형태의 Pickle파일 생성을 위한 코드

## 개요 및 문제정의
- 서비스에 지속적으로 추가되는 고객의 법정구와 자치구를 딕셔너리에 수작업으로 추가하는 형태로 작업하고 있었음
- 그로인한 에러발생과 업무로드가 겹쳐져 비효율적이였음

## 해결과정
1. 서울특별시의 법정동과 자치구를 구할 수 있는 데이터 수집
 
   출처: https://www.code.go.kr/stdcode/regCodeL.do
   
2. 정규표현식으로 치구/법정동을 나누어 리스트 형식으로 저장

3. 위의 리스트를 딕셔너리형태로 묶음

4. pickle파일 형태로 저장
```
## 우편번호 - 행정동 딕셔너리 형태로 만들기
def load_pickle(name_pkl):
    with open(name_pkl, 'rb') as f:
        obj = pickle.load(f)
    return obj
# 예시
data = load_pickle('경로/data.pickle')
## pickle 저장 코드 (dict1 객체 저장)
def save_pickle(name_pkl, obj):
    with open(name_pkl, 'wb') as f:
        pickle.dump(obj, f, pickle.HIGHEST_PROTOCOL)
# 예시
save_pickle('경로.pickle', data)
```
