# 멕시코풍 프랜차이즈 Chipotle의 주문 데이터 분석하기
## 데이터의 기초 정보 살펴보기


### Chipotle 데이터셋의 수치적 특징 파악
- describe 함수로 요약통계 파악가능
```
chipo['order_id'] = chipo['order_id'].astype(str) 
```
-  order_id는 숫자의 의미를 가지지 않기 때문에 str으로 변환합니다.
```
print(chipo.describe()) 
```
- chipo dataframe에서 수치형 피처들의 요약 통계량을 확인합니다.
```
quantity
count  4622.000000
mean      1.075725
std       0.410186
min       1.000000
25%       1.000000
50%       1.000000
75%       1.000000
max      15.000000
```
#### unique 함수로 범주형 피처의 개수 출력하기
- order_id의 개수를 출력합니다.
- item_name의 개수를 출력합니다.
```
print(len(chipo['order_id'].unique())) 
print(len(chipo['item_name'].unique())) 
```
```
1834
50
```
- orderid의 개수는 1834개
- item_name의 개수는 50개 로 확인됩니다.
- 중복되지 않은 값으로 각자 다른 id의 1개의 물량만의 갯수를 출력해줍니다.

### 탐색과 시각화
- 가장 많이 주문한 item은 무엇일까
- itme당 주문의 총량은 얼마일까
#### 가장 많이 주문한 item
- 가장 많이 주문한 item을 top10으로 출력합니다.
```
item_count = chipo['item_name'].value_counts()[:10]
item_count
```
```
Chicken Bowl                    726
Chicken Burrito                 553
Chips and Guacamole             479
Steak Burrito                   368
Canned Soft Drink               301
Steak Bowl                      211
Chips                           211
Bottled Water                   162
Chicken Soft Tacos              115
Chips and Fresh Tomato Salsa    110
Name: item_name, dtype: int64
```
```
item_count = chipo['item_name'].value_counts()[:10]
for idx, (val, cnt) in enumerate(item_count.iteritems(), 1):
    print("Top", idx, ":", val, cnt)
```
- enumerate 함수를 이용하여 for문을 출력한다.
```
Top 1 : Chicken Bowl 726
Top 2 : Chicken Burrito 553
Top 3 : Chips and Guacamole 479
Top 4 : Steak Burrito 368
Top 5 : Canned Soft Drink 301
Top 6 : Steak Bowl 211
Top 7 : Chips 211
Top 8 : Bottled Water 162
Top 9 : Chicken Soft Tacos 115
Top 10 : Chips and Fresh Tomato Salsa 110
```
### 리스트화
```
chipo['item_name'].value_counts().index.tolist()[0]
```
- item_name을 값을 count하여 리스트형식으로 담아낸다.
```
['Chicken Bowl',
 'Chicken Burrito',
 'Chips and Guacamole',
 'Steak Burrito',
 'Canned Soft Drink',
 'Steak Bowl',
 'Chips',
 'Bottled Water',
 'Chicken Soft Tacos',
 'Chips and Fresh Tomato Salsa',
 'Chicken Salad Bowl',
 'Canned Soda',
 'Side of Chips',
 'Veggie Burrito',
 'Barbacoa Burrito',
 'Veggie Bowl',
 'Carnitas Bowl',
 'Barbacoa Bowl',
 'Carnitas Burrito',
 'Steak Soft Tacos',
 '6 Pack Soft Drink',
 'Chips and Tomatillo Red Chili Salsa',
 'Chicken Crispy Tacos',
 'Chips and Tomatillo Green Chili Salsa',
 'Carnitas Soft Tacos',
 'Steak Crispy Tacos',
 'Chips and Tomatillo-Green Chili Salsa',
 'Steak Salad Bowl',
 'Nantucket Nectar',
 'Barbacoa Soft Tacos',
 'Chips and Roasted Chili Corn Salsa',
 'Izze',
 'Chips and Tomatillo-Red Chili Salsa',
 'Chips and Roasted Chili-Corn Salsa',
 'Veggie Salad Bowl',
 'Barbacoa Crispy Tacos',
 'Barbacoa Salad Bowl',
 'Chicken Salad',
 'Veggie Soft Tacos',
 'Carnitas Crispy Tacos',
 'Carnitas Salad Bowl',
 'Burrito',
 'Veggie Salad',
 'Steak Salad',
 'Bowl',
 'Salad',
 'Crispy Tacos',
 'Veggie Crispy Tacos',
 'Chips and Mild Fresh Tomato Salsa',
 'Carnitas Salad']
 ```
 - index 안에 리스트가 들어있다.
```
chipo['item_name'].value_counts().index[0]
```
```
'Chicken Bowl'
```
- 0번 인덱스인 Chicken Bowl이 출력된다.

### item당 주문 수와 총량 구하기
- item당 주문수를 출력.  주문수를 보여줍니다. 
```
order_count = chipo.groupby('item_name')['order_id'].count()
order_count[:10] # item당 주문 개수를 출력합니다. 
```
```
item_name
6 Pack Soft Drink         54
Barbacoa Bowl             66
Barbacoa Burrito          91
Barbacoa Crispy Tacos     11
Barbacoa Salad Bowl       10
Barbacoa Soft Tacos       25
Bottled Water            162
Bowl                       2
Burrito                    6
Canned Soda              104
Name: order_id, dtype: int64
```
- item당 주문 총량을 출력. 주문된 양을 보여줍니다.
```
item_quantity = chipo.groupby('item_name')['quantity'].sum()
item_quantity[:10] # item당 주문 총량을 출력합니다.
```
```
item_name
6 Pack Soft Drink         55
Barbacoa Bowl             66
Barbacoa Burrito          91
Barbacoa Crispy Tacos     12
Barbacoa Salad Bowl       10
Barbacoa Soft Tacos       25
Bottled Water            211
Bowl                       4
Burrito                    6
Canned Soda              126
Name: quantity, dtype: int64
```
- 비교
```
item_name                         item_name
6 Pack Soft Drink         54      6 Pack Soft Drink         55
Barbacoa Bowl             66      Barbacoa Bowl             66
Barbacoa Burrito          91      Barbacoa Burrito          91
Barbacoa Crispy Tacos     11      Barbacoa Crispy Tacos     12
Barbacoa Salad Bowl       10      Barbacoa Salad Bowl       10
Barbacoa Soft Tacos       25      Barbacoa Soft Tacos       25
Bottled Water            162      Bottled Water            211
Bowl                       2      Bowl                       4
Burrito                    6      Burrito                    6
Canned Soda              104      Canned Soda              126
Name: order_id, dtype: int64      Name: quantity, dtype: int64
```

- 왼쪽이 주문수이고 오른쪽이 주문된 양입니다.
- Bottled Water로 비교해보자면 주문된 횟수는 162번이고 
- 주문된 아이템의 총량은 211 입니다.
- 이말인 즉슨 한번 주문할때 여러개의 주문량이 있었을 것입니다.

### 시각화로 분석결과 살펴보기
- 지금까지의 분석결과를 간단하게 시각화로 표현해보겠습니다.
```
%matplotlib inline
import numpy as np
import matplotlib.pyplot as plt

item_name_list = item_quantity.index.tolist()
x_pos = np.arange(len(item_name_list))
order_cnt = item_quantity.values.tolist()
 
plt.bar(x_pos, order_cnt, align='center')
plt.ylabel('ordered_item_count')
plt.title('Distribution of all orderd item')
 
plt.show()
```
![image](https://user-images.githubusercontent.com/60453691/88370451-04c17b00-cdcd-11ea-909f-39ead1f527bd.png)

- 표를 보게 되면 y축은 주문된 아이템의 카운트 수입니다.
- x축은 주문된 아이템의 횟수정도를 나타냅니다.

### pandas에서 유용하게 사용되는 함수 value_counts()와 unique()의 차이점

```
print(chipo['item_name'].value_counts()[:10])
```
```
Chicken Bowl                    726
Chicken Burrito                 553
Chips and Guacamole             479
Steak Burrito                   368
Canned Soft Drink               301
Steak Bowl                      211
Chips                           211
Bottled Water                   162
Chicken Soft Tacos              115
Chips and Fresh Tomato Salsa    110
Name: item_name, dtype: int64
```
- 타입을 확인해봅니다.
```
print(type(chipo['item_name'].value_counts()))
```
```
<class 'pandas.core.series.Series'>
```

## 데이터 전처리
### apply와 lambda 함수를 이용한 데이터 전처리
- read_csv 함수로 데이터를 Dataframe 형태로 불러옵니다.
- 이미 위에서 float으로 전환했기때문에 또 float으로 바꿀수 없어서 에러 뜨기 때문에 다시 파일을 불러와야함
```
import pandas as pd

file_path = 'data/chipotle.tsv'
chipo = pd.read_csv(file_path, sep = '\t')
chipo.tail()
```
```
order_id	quantity	item_name	choice_description	item_price
4617	1833	1	Steak Burrito	[Fresh Tomato Salsa, [Rice, Black Beans, Sour ...	$11.75
4618	1833	1	Steak Burrito	[Fresh Tomato Salsa, [Rice, Sour Cream, Cheese...	$11.75
4619	1834	1	Chicken Salad Bowl	[Fresh Tomato Salsa, [Fajita Vegetables, Pinto...	$11.25
4620	1834	1	Chicken Salad Bowl	[Fresh Tomato Salsa, [Fajita Vegetables, Lettu...	$8.75
4621	1834	1	Chicken Salad Bowl	[Fresh Tomato Salsa, [Fajita Vegetables, Pinto...	$8.75
```
- column 단위 데이터에 apply 함수로 전처리를 적용합니다.
- %precision 3 : 이미 위에서 float으로 전환했기때문에 또 float으로 바꿀수 없어서 에러 뜨기 때문에 다시 파일을 불러와야함
```
%precision 3 
pd.set_option('precision',3)
chipo['item_price'] = chipo['item_price'].apply(lambda x: float(x[1:]))
chipo.describe()
```
```
order_id	quantity	item_price
count	4622.000	4622.000	4622.000
mean	927.255	1.076	7.464
std	528.891	0.410	4.246
min	1.000	1.000	1.090
25%	477.250	1.000	3.390
50%	926.000	1.000	8.750
75%	1393.000	1.000	9.250
max	1834.000	15.000	44.250
```
```
chipo['item_price'].head()
```
```
0     2.39
1     3.39
2     3.39
3     2.39
4    16.98
Name: item_price, dtype: float64
```

## 탐색적분석
- 데이터를 이해하기 위한 조금 더 복잡한 질문들로 탐색적 데이터 분석 연습하기
- 주문당 평균 계산금액 출력하기
- 한 주문에 10달러 이상 사용한 주문의 id들 출력하기
- 각 아이템의 가격 구하기
- 가장 비싼 주문에서 item이 몇개 팔렸는지 구하기
- “Veggie Salad Bowl”이 몇 번 주문되었는지 구하기
- “Chicken Bowl”을 2개 이상 주문한 주문 횟수 구하기

### 주문당 평균 계산금액 출력하기
- 주문당 평균 계산금액을 출력한다.
```
chipo.groupby('order_id')['item_price'].sum().mean()
```
```
18.811428571428717
```

### 가장 비싼 주문에서 item이 총 몇개 팔렸는지 구하기

```
        quantity	item_price
order_id		
    926	    23	205.25
    1443	35	160.74
    1483	14	139.00
    691	    11	118.25
    1786	20	114.30
```