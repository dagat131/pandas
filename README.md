# 판다스(pandas)

### 1. 판다스 시작하기
```
import pandas as pd
import numpy as np
pd.__version__
```
- 먼저 pandas를 import 해줍니다.
- 먼저 numpy를 import 해줍니다.
- pandas의 버전을 확인해 줍니다.

```
'1.0.5'
```

```
from pandas import Series, DataFrame
```
- pandas의 Series와 DataFrame을 import 해줍니다.
- Series는 일련의 객체를 담을 수 있는 1차원 배월 같은 자료 구조
```
obj = pd.Series([4, 7, -5, 3])
obj
```
```
0    4
1    7
2   -5
3    3
dtype: int64
```
- index 번호대로 차례대로 출력이 됩니다.

```
obj.values
```
```
array([ 4,  7, -5,  3], dtype=int64)
```
- obj의 vlaue를 확인해 봅니다. dtype과 함께 확인할 수 있습니다.

#### 숫자로 된 index를 문자열로 바꿀 수 있습니다.
```
obj2 = pd.Series([4, 7, -5, 3], index=['d', 'b', 'a', 'c'])
obj2
obj2.index
```
```
d    4
b    7
a   -5
c    3
dtype: int64

Index(['d', 'b', 'a', 'c'], dtype='object')
```

#### 인덱스 번호를 출력하지 않고 바뀐 문자열로 value값을 불러올 수 있습니다.
```
obj2['a']
```
```
-5
```

### 2. 데이터 적용하기

```
sdata = {'Ohio': 35000, 'Texas': 71000, 'Oregon': 16000, 'Utah': 5000}
obj3 = pd.Series(sdata)
obj3
```
- key, value 값으로 sdate 변수에 넣어줍니다.
- Series 형태로 sdate를 만들어줍니다.
```
Ohio      35000
Texas     71000
Oregon    16000
Utah       5000
dtype: int64
```

- California 데이터는 기존에 저장된 데이터엔 없는 데이터 입니다.
```
states = ['California', 'Ohio', 'Oregon', 'Texas']
obj4 = pd.Series(sdata, index=states)
obj4
```
- California는 None값이 출력됩니다.
- California는 value값이 존재하지 않기 때문에 None으로 출력됩니다.
- obj4 변수에는 기존의 sdate에 index를 states로 맞추어 줍니다.
```
California        NaN
Ohio          35000.0
Oregon        16000.0
Texas         71000.0
dtype: float64
```
- isnull 함수로 obj4의 null값을 확인해봅니다.
```
California     True
Ohio          False
Oregon        False
Texas         False
dtype: bool
```
- notnull 함수로 obj4의 null값을 확인해봅니다.
```
California    False
Ohio           True
Oregon         True
Texas          True
dtype: bool
```

- isnull의 경우 value값에 none 값이 있으면 True로 출력하게 됩니다.
- notnull의 경우 value값에 none 값이 없으면 False 로 출력하게 됩니다.

#### 값을 더할때 값이 없으면 Nan이 출력
```
obj3 + obj4
```
```
California         NaN
Ohio           70000.0
Oregon         32000.0
Texas         142000.0
Utah               NaN
dtype: float64
```
- obj3에는 California가 없기 때문에,
- obj4에는 Utah가 없기 때문에, 
- 값을 더하게 되면 두 값은 Nan값으로 출력이 됩니다.

### 요소추가 / 요소삭제
- 요소추가
```
obj
obj.index = ['Bob', 'Steve', 'Jeff', 'Ryan']
obj = obj.append(pd.Series([5],index=['Tom']))
obj
```
```
Bob      4
Steve    7
Jeff    -5
Ryan     3
Tom      5
dtype: int64
```
- 요소삭제
```
obj = obj.drop('Tom')
obj
```
```
Bob      4
Steve    7
Jeff    -5
Ryan     3
dtype: int64
```

### 필터링
- obj가 0 이상, 5이하인 값을 필터링 해줍니다.
```
obj[obj >= 0][obj <=  5]
```
```
Bob     4
Ryan    3
dtype: int64
```
### 정렬
- ascending = True : 생략가능 / ascen
- 내림차순
```
obj.sort_index(ascending = False) # 내림차순
```
```
Steve    7
Ryan     3
Jeff    -5
Bob      4
dtype: int64
```
- 오름차순
```
obj.sort_index(ascending = True)
```
```
Bob      4
Jeff    -5
Ryan     3
Steve    7
dtype: int64
```

### DataFrame
- 데이터프레임 형식으로 date를 넣어줍니다.
- key, value 형식으로 열에 키 값을 바탕으로 일렬로 값이 들어갑니다.
```
data = {'state': ['Ohio', 'Ohio', 'Ohio', 'Nevada', 'Nevada', 'Nevada'],
        'year': [2000, 2001, 2002, 2001, 2002, 2003],
        'pop': [1.5, 1.7, 3.6, 2.4, 2.9, 3.2]}
frame = pd.DataFrame(data)
frame
```
```
	state	year	pop
0	Ohio	2000	1.5
1	Ohio	2001	1.7
2	Ohio	2002	3.6
3	Nevada	2001	2.4
4	Nevada	2002	2.9
5	Nevada	2003	3.2
```

### Dataframe 치환하기(Transpose)
- Dataframe의 치환읜 굉장히 간단합니다.
- .T 를 사용하게 되면 치환시켜 줄 수 있습니다.
```
frame3 = pd.DataFrame(pop)
frame3
```
```
	Nevada	Ohio
2001	2.4	1.7
2002	2.9	3.6
2000	NaN	1.5
```
```
frame3.T
```
```
2001	2002	2000
Nevada	2.4	2.9	NaN
Ohio	1.7	3.6	1.5
```
- 치환이 완료되었습니다.

### 데이터 프레임 pop기능 / pandas.DataFrame.pop 
```
DataFrame.pop( self : ~ FrameOrSeries , item ) → ~ FrameOrSeries
항목을 반환하고 프레임에서 떨어 뜨립니다. 찾을 수 없으면 KeyError를 발생시킵니다.

매개 변수
아이템 str
팝업되는 열의 레이블입니다.
```
- pop 은 데이터를 따로 빼는 역할을 해줍니다.
```
pop2=pd.DataFrame(pop, index=[2001, 2002, 2003])
pop2
```
```
Nevada	Ohio
2001	2.4	1.7
2002	2.9	3.6
2003	NaN	NaN
```
### 데이터프레임 다루기
```
data = pd.DataFrame(np.arange(16).reshape((4, 4)),
                    index=['Ohio', 'Colorado', 'Utah', 'New York'],
                    columns=['one', 'two', 'three', 'four'])
data
```
```
	      one  two	three four
Ohio	    0	1	2	3
Colorado	4	5	6	7
Utah	    8	9	10	11
New York	12	13	14	15
```

- colorado, ohio 드랍
```
data.drop(['Colorado', 'Ohio'])
```
```
	    one	two	three	four
Utah	    8	9	10	11
New York	12	13	14	15
```
- index를 나라로 정하기
```
data = pd.DataFrame(np.arange(16).reshape((4, 4)),
                    index=['Ohio', 'Colorado', 'Utah', 'New York'],
                    columns=['one', 'two', 'three', 'four'])
data
```
```
            one	two	three	four
Ohio	    0	1	2	3
Colorado	4	5	6	7
Utah	    8	9	10	11
New York	12	13	14	15
```
- 5 초과인 값 출력하기
```
data[data['three'] > 5]
```
```
	        one	two	three	four
Colorado	4	5	6	7
Utah	    8	9	10	11
New York	12	13	14	15
```

### loc와 iloc의 차이
```
iloc : integer position을 통해 값을 찾는다.

loc : label을 통해서 값을 찾는다.

문법 공통점:

df1.loc[[행],[열]]

단순 row만 필터할 땐 둘다 행열에는 숫자를 입력받으므로 동일하게 사용할 수 있다.

```
```
data.loc['Colorado', ['two', 'three']]
```
```
two      5
three    6
Name: Colorado, dtype: int32
```
```
data.iloc[2, [3, 0, 1]]
```
```
four    11
one      8
two      9
Name: Utah, dtype: int32
```
- iloc는 index값으로 값을 찾는다.
- 2번째 행(0부터 시작, Utah)의 3번, 0번, 1번 순으로 출력한다.

### 순서열뽑기
```
data.loc[:'Utah', 'two']
```
```
Ohio        1
Colorado    5
Utah        9
Name: two, dtype: int32
```
- 유타까지의 두번째 인덱스값을 불러온다.

#### 이름을 선택할때는 loc를 정수색인을 선택할때는 iloc : 색인이 1인 곳까지
```
ser.loc[:1]
```
```
0    0.0
1    1.0
dtype: float64
```

#### 이름을 선택할때는 loc를 정수색인을 선택할때는 iloc : 인덱스가 1보다 작은 값(0)까지
```
ser.iloc[:1]
```
```
0    0.0
dtype: float64
```

### Arithmetic methods with fill values(값 채우기)
```
df1 = pd.DataFrame(np.arange(12.).reshape((3, 4)),
                   columns=list('abcd'))
df1
```
```
    a	b	c	d
0	0.0	1.0	2.0	3.0
1	4.0	5.0	6.0	7.0
2	8.0	9.0	10.0 11.0
```
```
df2 = pd.DataFrame(np.arange(20.).reshape((4, 5)),
                   columns=list('abcde'))
df2.loc[1, 'b'] = np.nan
df2
```
```
    a	b	c	d	  e
0	0.0	1.0	2.0	3.0	4.0
1	5.0	NaN	7.0	8.0	9.0
2	10.0 11.0 12.0 13.0 14.0
3	15.0 16.0 17.0 18.0	19.0
```
```
df1 + df2
```
```
    a	b	c	d	e
0	0.0	2.0	4.0	6.0	NaN
1	9.0	NaN	13.0	15.0	NaN
2	18.0	20.0	22.0	24.0	NaN
3	NaN	NaN	NaN	NaN	NaN
```
- add를 사용
```
df1.add(df2, fill_value=0)
```
```
    a	    b	    c	    d	    e
0	0.0	    2.0	    4.0	    6.0	    4.0
1	9.0	    5.0	    13.0	15.0	9.0
2	18.0	20.0	22.0	24.0	14.0
3	15.0	16.0	17.0	18.0	19.0
```
- df1은 4x4 곱이고 df2는 4x5 행렬이다.
- df1에 add를 하여도 e열은 덧셈이 되지 않는다.

#### Function Application and Mapping
- np.random.rand() : 0~1사이의 랜덤 값
```
np.random.rand()
```
```
0.3542538701243291
```
- np.random.randn() : 평균0 표준편차1 가우시안 표준정규분포 난수 랜덤

```
np.random.randn()
```
```
-1.2822799819062438
```

```
frame = pd.DataFrame(np.random.randn(4, 3), columns=list('bde'),
                     index=['Utah', 'Ohio', 'Texas', 'Oregon'])
frame
```
```
	            b	        d	        e
Utah	-0.688162	0.150911	-0.450867
Ohio	0.379141	-0.665375	1.386786
Texas	-0.781644	-1.198891	0.701206
Oregon	-1.800643	-0.855044	1.430706
```
### Sorting and Ranking
```
obj = pd.Series(range(4), index=['d', 'a', 'b', 'c'])
obj
```
```
d    0
a    1
b    2
c    3
dtype: int64
```
```
obj.sort_index()
```
```
a    1
b    2
c    3
d    0
dtype: int64
```
### Axis Indexes with Duplicate Labels

```
obj = pd.Series(range(5), index=['a', 'a', 'b', 'b', 'c'])
obj
```
```
a    0
a    1
b    2
b    3
c    4
dtype: int64
```
