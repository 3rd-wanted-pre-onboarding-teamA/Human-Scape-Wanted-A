# labQ wanted teamA mission

## 👥 Team


- [황선영](https://github.com/syoungee)
- [이승연](https://github.com/dltmddus1998)
- [허정연](https://github.com/golgol22)
- [장덕수](https://github.com/dapsu)

`💻 프로젝트 진행기간 2022.06.29 ~ 2022.07.01 18:00`

### [📑 LabQ 기업과제](https://misty-lungfish-f16.notion.site/LabQ-OpenAPI-cc0e492facda4abfadabd844f843004d)

## 🔗 배포링크
[⚙️ 방법 1.](https://lab-qqq.herokuapp.com/api/seoul-rainfall-drainpipe-data1?cityId=01)

[⚙️ 방법 2.](https://lab-qqq.herokuapp.com/api/seoul-rainfall-drainpipe-data2?cityId=01)

## ⚒️ 기술 스택

![image](https://img.shields.io/badge/LAN-JavaScript-%23F7DF1E?style=for-the-badge&logo=JavaScript)

![image](https://img.shields.io/badge/FRM-Node.js-%23339933?style=for-the-badge&logo=Node.js)

![image](https://img.shields.io/badge/FRM-Express-%23000000?style=for-the-badge&logo=Express)

## ✍️ 서비스 개요

- 서울시 하수관로 수위 현황과 강우량 정보 데이터를 수집 및 가공하여 전달하는 REST API와 이를 요청하는 클라이언트 개발

<br>
<br>

## 📑 요구사항 분석

### ✔︎ 프로젝트 요구사항
**Open API를 활용하여 공공데이터 수집 및 가공, 데이터 전달하는 REST API 개발**

- Node.js를 이용하여 REST API 개발
- 서울시 `하수관로 수위 현황`과 `강우량` 정보 json 방식으로 전달받아 데이터 결합
    - [https://data.seoul.go.kr/dataList/OA-2527/S/1/datasetView.do](https://data.seoul.go.kr/dataList/OA-2527/S/1/datasetView.do)
    - [http://data.seoul.go.kr/dataList/OA-1168/S/1/datasetView.do](http://data.seoul.go.kr/dataList/OA-1168/S/1/datasetView.do)
- 결합된 데이터 출력

### ✔︎ 방향성 정리

> 프로젝트 요구사항을 보고 두 가지 방향을 생각했다.
> 
1. `방법1` 공통된 시간대의 하수관로 수위와 강우량을 동시에 확인 할 수 있게 데이터 결합하는 방법
2. `방법2` 받아온 모든 데이터를 누락없이 제공할 수 있도록 병렬 처리 하는 방법

       → 이에 따라 두 가지 방향성 모두 구현하기로 하였다.

### ✔︎ 요구사항 분석

- 서울시 하수관로 수위 현황과 강우량 정보 데이터 수집
    - 서울시 하수관로 수위 현황
        - [x]  측정일자
        - [x]  구분명(지역명)
        - [x]  측정수위
    - 강우량 정보 데이터
        - [x]  자료수집 시각
        - [x]  구청명
        - [x]  10분 우량
- 출력값 중 GUBN_NAM과 GU_NAME 기준으로 데이터 결합

    > **하수관로 수위 현황의 측정일자와 강우량 정보 데이터의 자료수집 시각이 일치할 때 데이터를 결합하도록 한다.**
    > 
    - [x]  날짜
    - [x]  구분명(지역명)
    - [x]  측정수위
    - [x]  10분 우량
- ✍️ 데이터는 `JSON`으로 전달 - **두 가지 방식으로 전달해보려 한다.**

    <aside>
    💡 먼저 두 가지 방법 모두 동일하게, 측정 시간은 현재 시간으로 고정했다.
    </aside>
    <br>
    <br>

    - 첫 번째
        - 프로그램의 확장성을 고려했다.
        - 클라이언트가 어떤 형식으로 받아야 할지 명확하게 알 수 없어서, 데이터를 모두 내려주는 게 맞다고 판단했다.
        - 명세가 하나도 없을 때, 데이터가 누락되는 경우 클라이언트 측에서 혼란을 느낄 수도 있다고 가정했다.
        - 위 모든 상황들을 고려했을 때, join하지 않고 병렬적으로 결합하고자 한다.
            ```json
            {
              "CODE": 200,
              "GUBN": "종로",
              "GUBN_NUM": "01",
              "DRAINPIPE": [
                {
                  "IDN": "01-0002",
                  "GUBN": "01",
                  "GUBN_NAM": "종로",
                  "MEA_YMD": "2022-07-01 13:51:03.0",
                  "MEA_WAL": 0,
                  "SIG_STA": "통신양호"
                },
                {
                  "IDN": "01-0001",
                  "GUBN": "01",
                  "GUBN_NAM": "종로",
                  "MEA_YMD": "2022-07-01 13:51:03.0",
                  "MEA_WAL": 0.17,
                  "SIG_STA": "통신양호"
                },
                {
                  "IDN": "01-0004",
                  "GUBN": "01",
                  "GUBN_NAM": "종로",
                  "MEA_YMD": "2022-07-01 13:52:03.0",
                  "MEA_WAL": 0.17,
                  "SIG_STA": "통신양호"
                },
                {
                  "IDN": "01-0003",
                  "GUBN": "01",
                  "GUBN_NAM": "종로",
                  "MEA_YMD": "2022-07-01 13:52:03.0",
                  "MEA_WAL": 0.14,
                  "SIG_STA": "통신양호"
                },
                {
                  "IDN": "01-0002",
                  "GUBN": "01",
                  "GUBN_NAM": "종로",
                  "MEA_YMD": "2022-07-01 13:52:03.0",
                  "MEA_WAL": 0,
                  "SIG_STA": "통신양호"
                },
                {
                  "IDN": "01-0001",
                  "GUBN": "01",
                  "GUBN_NAM": "종로",
                  "MEA_YMD": "2022-07-01 13:52:03.0",
                  "MEA_WAL": 0.17,
                  "SIG_STA": "통신양호"
                },
                {
                  "IDN": "01-0003",
                  "GUBN": "01",
                  "GUBN_NAM": "종로",
                  "MEA_YMD": "2022-07-01 13:53:03.0",
                  "MEA_WAL": 0.14,
                  "SIG_STA": "통신양호"
                },
                {
                  "IDN": "01-0002",
                  "GUBN": "01",
                  "GUBN_NAM": "종로",
                  "MEA_YMD": "2022-07-01 13:53:03.0",
                  "MEA_WAL": 0,
                  "SIG_STA": "통신양호"
                },
                {
                  "IDN": "01-0001",
                  "GUBN": "01",
                  "GUBN_NAM": "종로",
                  "MEA_YMD": "2022-07-01 13:53:03.0",
                  "MEA_WAL": 0.17,
                  "SIG_STA": "통신양호"
                }
              ],
              "RAINFALL": [
                {
                  "RAINGAUGE_CODE": 1002,
                  "RAINGAUGE_NAME": "부암동",
                  "GU_CODE": 110,
                  "GU_NAME": "종로구",
                  "RAINFALL10": "0",
                  "RECEIVE_TIME": "2022-06-28 02:49"
                },
                {
                  "RAINGAUGE_CODE": 1001,
                  "RAINGAUGE_NAME": "종로구청",
                  "GU_CODE": 110,
                  "GU_NAME": "종로구",
                  "RAINFALL10": "0",
                  "RECEIVE_TIME": "2022-06-28 02:49"
                },
                {
                  "RAINGAUGE_CODE": 1002,
                  "RAINGAUGE_NAME": "부암동",
                  "GU_CODE": 110,
                  "GU_NAME": "종로구",
                  "RAINFALL10": "0",
                  "RECEIVE_TIME": "2022-06-28 02:39"
                },
                {
                  "RAINGAUGE_CODE": 1001,
                  "RAINGAUGE_NAME": "종로구청",
                  "GU_CODE": 110,
                  "GU_NAME": "종로구",
                  "RAINFALL10": "0",
                  "RECEIVE_TIME": "2022-06-28 02:39"
                },
                {
                  "RAINGAUGE_CODE": 1001,
                  "RAINGAUGE_NAME": "종로구청",
                  "GU_CODE": 110,
                  "GU_NAME": "종로구",
                  "RAINFALL10": "0",
                  "RECEIVE_TIME": "2022-06-28 02:29"
                },
                {
                  "RAINGAUGE_CODE": 1002,
                  "RAINGAUGE_NAME": "부암동",
                  "GU_CODE": 110,
                  "GU_NAME": "종로구",
                  "RAINFALL10": "0",
                  "RECEIVE_TIME": "2022-06-28 02:29"
                },
                {
                  "RAINGAUGE_CODE": 1002,
                  "RAINGAUGE_NAME": "부암동",
                  "GU_CODE": 110,
                  "GU_NAME": "종로구",
                  "RAINFALL10": "0",
                  "RECEIVE_TIME": "2022-06-28 02:19"
                },
                {
                  "RAINGAUGE_CODE": 1001,
                  "RAINGAUGE_NAME": "종로구청",
                  "GU_CODE": 110,
                  "GU_NAME": "종로구",
                  "RAINFALL10": "0",
                  "RECEIVE_TIME": "2022-06-28 02:19"
                },
                {
                  "RAINGAUGE_CODE": 1002,
                  "RAINGAUGE_NAME": "부암동",
                  "GU_CODE": 110,
                  "GU_NAME": "종로구",
                  "RAINFALL10": "0",
                  "RECEIVE_TIME": "2022-06-28 02:09"
                }
              ]
            }
            ```

        - 두 번째
            - 서울시 하수관로 수위 현황에서 한 지역 내에서 (예) 종로구) 동일한 시간대에 고유번호에 따른 다른 위치에서의 데이터가 존재하는 것을 발견했다.
            - 이를 해결하기 위한 방안으로, 평균값을 구한 후, `innerJoin`방식으로 두 테이블을 합쳐 출력할 예정이다. → Client

                > 서울시 하수관로 수위 현황은 1분, 강우량 정보 데이터는 10분 주기로 업데이트 되므로 이 두 데이터를 합쳐서 출력하기 위해 10분 단위로 측정한 데이터 값을 출력하려 한다.

            ```json
            {
              "CODE": 200,
              "GUBN": "종로",
              "GUBN_NUM": "01",
              "RESULT": [
                {
                  "localname": "종로",
                  "date": "2022-07-01 13:09",
                  "waterLevel": "0.125",
                  "rainFall": 0
                },
                {
                  "localname": "종로",
                  "date": "2022-07-01 13:19",
                  "waterLevel": "0.122",
                  "rainFall": 0
                },
                {
                  "localname": "종로",
                  "date": "2022-07-01 13:29",
                  "waterLevel": "0.120",
                  "rainFall": 0
                },
                {
                  "localname": "종로",
                  "date": "2022-07-01 13:39",
                  "waterLevel": "0.120",
                  "rainFall": 0
                },
                {
                  "localname": "종로",
                  "date": "2022-07-01 13:49",
                  "waterLevel": "0.120",
                  "rainFall": 0
                }
              ]
            }
            ```

        
        
- Client
    - 구분코드를 명시해서 REST API 호출하기
    - 서버에서 전송받은 결과 출력
        - 콘솔에 로그로 출력

## 📜 REST API

| Method | Request | Url  |
| --- | --- | --- |
| GET | inner join 방식으로 결합 | https://lab-qqq.herokuapp.com/api/seoul-rainfall-drainpipe-data1?cityId=01 |
| GET | 병렬 방식으로 출력 | https://lab-qqq.herokuapp.com/api/seoul-rainfall-drainpipe-data2?cityId=01 |

## 📝 More Information
[Team A Notion Page](https://www.notion.so/Team-A-7434a7bbf7ef4a8ebf2f458f44cccd2f)
