# 트위터API를 활용한 키워드 분석 웹 페이지

## 사용 언어
- Python3.6
- HTML/CSS/JavaScript with jQuery

## 프로젝트 간단 소개
트위터API를 사용하여 국내(대한민국) 트윗들을 크롤링합니다.<br/>
크롤링된 트윗들을 한글 전처리를 통해 단어(keyword)로 추출합니다.<br/>
각 키워드의 수를 count해서 많이 언급된 키워드 순으로 순위표를 나타내줍니다.<br/>
하루 기준으로 막대그래프와 워드클라우드(Word Cloud)로 시각화하여 보여줍니다.<br/>
특정 키워드를 클릭하면 해당 키워드의 일자별 언급 횟수 변화가 꺽은선 그래프로 표시됩니다.

## 프로젝트 구현 이미지
<img src="https://github.com/gibiee/Twitter_Crawling/blob/master/img/구동샘플.jpg" width="70%" height="70%"></img>

순위표의 특정 키워드를 클릭할 시 다음과 같은 꺽은선 그래프가 표시됨.
<img src="https://github.com/gibiee/Twitter_Crawling/blob/master/img/꺽은선그래프.jpg" width="70%" height="70%"></img>


## 구성 파일별 소개
1. __트위터 크롤링__
    - 트위터API를 사용하여 트윗들을 크롤링한 후 txt파일으로 저장. 저장 형식은 하단 부록 참고.
    - 한 번 크롤링 할 때 약 17,000개의 트윗이 크롤링이 되며 15분 동안 크롤링이 정지된다. 따라서 1시간 단위로 크롤링을 자동화시켜 진행한다. 자동화 방법은 하단 구동 방법 참고.
    - 업로드된 파일에는 보안상 API key 값이 삭제되어 있음.
    
2. __txt 하루 단위로 합치기__
    - 1시간 단위로 크롤링 한 txt파일들을 하루(24시간) 단위로 합친다.

3. __시각화 그래프 생성 및 업로드__
    - 하루 단위로 합쳐진 txt파일을 기준으로 막대그래프와 워드클라우드 이미지를 생성한다. 생성된 이미지들은 Google Storage 버킷에 저장된다.

4. __토픽 추출 HTML 생성 및 업로드__
    - 순위표는 HTML파일로 생성된 것이다. 관련 키워드 추출은 머신러닝 LDA 기법을 사용하였다.

5. __꺽은선그래프 생성 및 업로드__
    - 꺽은선 그래프는 하루가 지날 때마다 x축이 증가해야하므로 구동시간이 오래걸린다. 꺽은선 그래프 이미지를 생성한 후 Google Storage 버킷에 저장한다.

## 구동 방법
- 자동화 : GCE(Google Compute Engine)을 이용하여 crontab 명령어를 통해 1시간 단위로 자동화 구현하였다.
- 웹 페이지 배포 : Google App Engine을 이용하였다.

## 사용 라이브러리
- Tweepy : 트위터 API.
- KonLPy : 한글 전처리 패키지. 그 중 Okt 클래스를 사용함.
- Matplotlib : 막대그래프, 워드클라우드 시각화에 사용됨.
- Numpy : 막대그래프 시각화에 사용됨.
- WordCloud : 워드클라우드 시각화에 사용됨..

## 부록. 크롤링 형식
1. per_hour_before : 1시간 단위로 [[트윗, 시간], ...] 저장.
2. per_hour_after : 1시간 단위로 [[[단어들...], 시간], ...] 저장.
3. per_day_before : 하루 단위로 [트윗, ...] 저장
4. per_day_after : 하루 단위로 [[[단어들...], 시간], ...] 저장
5. per_day_keword : 하루 단위로 [단어들, ...] 저장

## 부록. 각 파일별 실행시간
1. 트위터 크롤링 : 약 3~5분
2. txt 하루 단위로 합치기 : 약 1분
3. 시각화 그래프 생성 및 업로드 : 약 1분
4. 토픽 추출 HTML 생성 및 업로드 : 약 30분
5. 꺽은선그래프 생성 및 업로드 : (일주일 분량 기준으로)약 2시간

crontab 작업이 병렬적으로 실행되므로 실행시간을 잘 고려하여 예약하는 것이 중요함.

<br/><br/><br/>
# 추후 수정 예정 : 없음.