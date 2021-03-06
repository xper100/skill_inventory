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

```
def get_location_name(name_location):
    split_list = re.split(r'[ ]', name_location) # 띄어쓰기를 기준으로 문자열 나누기
    자치구 = split_list[1]
    법정동 = split_list[2]
    return 법정동, 자치구
```

3. 위의 리스트를 딕셔너리형태로 묶음

```
location_column = raw_data['법정동명']
법정동_list = []
자치구_list = []

for idx in range(len(location_column)):
    if len(location_column[idx]) < 11:
        continue
    else:     
        법정동, 자치구 = get_location_name(location_column[idx])
        법정동_list.append(법정동)
        자치구_list.append(자치구)

dictionary_location = dict(zip(법정동_list,자치구_list))
dictionary_location
```

4. pickle파일 형태로 저장
```
## 딕셔너리 형태로 만들기
def load_pickle(name_pkl):
    with open(name_pkl, 'rb') as f:
        obj = pickle.load(f)
    return obj
# 예시
data = load_pickle('경로/data.pickle')


## pickle 저장방식
def save_pickle(name_pkl, obj):
    with open(name_pkl, 'wb') as f:
        pickle.dump(obj, f, pickle.HIGHEST_PROTOCOL)
# 예시
save_pickle('경로.pickle', data)
```
