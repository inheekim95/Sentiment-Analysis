# Sentiment Analysis (감성 분석)
감성 분석 프로젝트를 위한 데이터 및 python code가 있습니다.

해당 데이터를 활용하여 진행한 프로젝트는 Velog에서 확인할 수 있습니다.

**프로젝트 목표:** 네이버 쇼핑 데이터, 남성 의류 데이터, 여성 의류 데이터를 활용하여 별점이 없는 제품에 대한 리뷰 긍/부정 판별

## 1. 데이터 목록
### 1) naver_shopping.txt
- 출처: https://github.com/bab2min/corpus/tree/master/sentiment
- 수집기간: 2020.06 ~ 2020.07
- 데이터 건수: 20만 건
- 설명: 네이버 쇼핑에서 제품별 후기를 별점과 함께 수집한 데이터입니다. 탭으로 분리되어 있으며, 첫 번째 필드에는 별점(1 ~ 5점), 두 번째 필드에는 리뷰가 있습니다. 리뷰 3점에 해당하는 데이터는 긍정이나 부정으로 분류하기 애매하기 때문에 제외되었습니다. 긍정과 부정의 비율은 거의 50:50으로 동일하게 샘플링 되었습니다.
#### 데이터 미리보기
```
5	배공빠르고 굿
2	택배가 엉망이네용 저희집 밑에층에 말도없이 놔두고가고
5	아주좋아요 바지 정말 좋아서2개 더 구매했어요 이가격에 대박입니다. 바느질이 조금 엉성하긴 하지만 편하고 가성비 최고예요.
2	선물용으로 빨리 받아서 전달했어야 하는 상품이었는데 머그컵만 와서 당황했습니다. 전화했더니 바로주신다했지만 배송도 누락되어있었네요.. 확인안하고 바로 선물했으면 큰일날뻔했네요..이렇게 배송이 오래걸렸으면 사는거 다시 생각했을거같아요 아쉽네요..
5	민트색상 예뻐요. 옆 손잡이는 거는 용도로도 사용되네요 ㅎㅎ
```

### 2) Score_n_Review.txt
- 설명: naver_shopping.txt 데이터를 별점에 따라 분리한 데이터입니다. 1, 2, 4, 5점에 대한 리뷰 데이터가 있습니다.

### 3) men_review_musinsa.csv
- 설명: 무신사에서 크롤링을 통해 수집한 남성 의류 리뷰 데이터로, 일반 후기에 대한 데이터입니다. 무신사 후기는 스타일 / 상품 / 일반 후기로 나뉘어져있으며, 일반 후기의 경우 20자 이상 필수이기 때문에 데이터의 질을 좋게 하기 위해 일반 후기를 선택하여 수집했습니다.
#### 데이터 미리보기
```
	0
0	조아요조아요조아요조아요조아요조아요조아요
1	사이즈 선택에 있어서 교환도 하고 많이 힘들었지만 옷은 가격 대비 좋아요
2	원단이 잘 늘어나서 좋습니다 활동하는데 거슬리는게 없습니드
3	무탠다드 정말 인정!! 앞으로 자주 입을꺼 같앙!! 최고
4	할인해서 좋은상품 잘구매한것 같습니다~'' 이쁘고 깔금하고 사이즈도 딱 좋습니다~^^
```

### 4) women_review_auction.csv
- 설명: 옥션에서 크롤링을 통해 수집한 여성 의류 리뷰 데이터입니다. 한 사람이 여러 제품을 동시에 구매한 후 동일한 후기를 남긴 경우에 한해 일부 중복되는 리뷰가 존재합니다. 각 제품마다 다른 리뷰를 작성하는 경우도 있기 때문에 중복을 제거하지 않고 수집하였습니다.
#### 데이터 미리보기
```
	0
0	디자인 기장 문안하고 깔끔해요
1	단추나 지퍼가 없지만 가볍게 걸치긴 좋았어요 실내에서 자주 걸치고 있었는데 보시는 ...
2	너무 스키니함..;
3	예상보다 괜찮음!
4	그냥그래요
```

### 5) review_synonym.txt
- 설명: 리뷰 데이터 분석 시 활용한 동의어(유의어) 데이터입니다. 같거나 비슷한 용어를 정리해놓았습니다.
#### 데이터 미리보기
```
가깝다,가까이
색상,색감,색이,색깔,색,컬러,칼라
아이보리,크림
검정색,블랙,까맣다,검정
흰색,하얗다,허옇다,화이트,희다,백색,새하얗다
```

### 6) stopwords_self_improved.txt
- 설명: 감성 분석에 사용한 stopwords는 https://raw.githubusercontent.com/yoonkt200/FastCampusDataset/master/korean_stopwords.txt를 참고하였으나, 자체적으로 추가한 stopwors를 파일로 따로 정리했습니다. 분석 시 기존 stopwords와 추가한 stopwords를 합쳐 진행했습니다.
#### 데이터 미리보기
```
배송
구매
요거
요건
요것
```

### 7) naver_shopping_tokenized_review.csv
- 설명: 기존 stopwords를 활용하여 토큰화한 리뷰 데이터입니다.
#### 데이터 미리보기
```
Score	Review	y	tokenized_1	tokenized_2
5	배공빠르고 굿	1	배공 빠르고	배공 빠르고 굿
2	택배가 엉망이네용 저희집 밑에층에 말도없이 놔두고가고	0	택배 엉망 놔두고가고	택배 엉망 이네 용 집 밑 층 없이 놔두고가고
5	아주좋아요 바지 정말 좋아서2개 더 구매했어요 이가격에 대박입니다. 박음질이 조금 ...	1	아주 좋아요 바지 정말 좋아서 구매 가격 대박 입니다 박음질 어설프다하긴 편하고 가...	아주 좋아요 바지 정말 좋아서 2 구매 가격 대박 입니다 . 박음질 어설프다하긴 편...
2	선물용으로 빨리 받아서 전달했어야 하는 상품이었는데 머그컵만 와서 당황했습니다. 전...	0	선물 받아서 전달 했어야 하는 상품 이었는데 머그컵 와서 당황 했습니다 전화했더니 ...	선물 용 빨리 받아서 전달 했어야 하는 상품 이었는데 머그컵 와서 당황 했습니다 ....
5	민트색상상 예뻐요. 옆 손잡이는 거는 용도로도 사용되네요 ㅎㅎ	1	민트 색상 예뻐요 손잡이 도로 사용 되네요	민트 색상 예뻐요 . 손잡이 는 는 용 도로 사용 되네요 ㅎㅎ
```

### 8) naver_shopping_tokenized_review_improved.csv
- 설명: 기존 stopwords에 추가한 stopwords를 합쳐 토큰화한 리뷰 데이터입니다.
```
Score	Review	y	tokenized_1	tokenized_2
5	배공빠르고 굿	1	배공 빠르고	배공 빠르고 굿
2	택배가 엉망이네용 집 밑에층에 말도없이 놔두고가고	0	택배 엉망 놔두고가고	택배 엉망 이네 용 집 밑 층 없이 놔두고가고
5	아주좋아요 바지 정말 좋아서2개 더 이가격에 대박입니다. 박음질이 조금 어설프다하...	1	아주 좋아요요 바지 정말 좋아요요서 가격 대박 입니다 박음질 어설프다하긴 편하고 ...	아주 좋아요요 바지 정말 좋아요요서 2 가격 대박 입니다 . 박음질 어설프다하긴 ...
2	선물용 빨리 받아서 전달했어야 하는 상품이었는데 머그컵만 와서 당황했습니다. 전화했...	0	선물 받아서 전달 했어야 하는 상품 이었는데 머그컵 와서 당황 했습니다 전화했더니 ...	선물 용 빨리 받아서 전달 했어야 하는 상품 이었는데 머그컵 와서 당황 했습니다 ....
5	민트색상상 예뻐요. 옆 손잡이는 거는 용도로도 사용되네요 ㅎㅎ	1	민트 색상 예뻐요 손잡이 도로 사용 되네요	민트 색상 예뻐요 . 손잡이 는 는 용 도로 사용 되네요 ㅎㅎ
```
