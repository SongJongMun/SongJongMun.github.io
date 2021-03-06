---
layout: post
title:  "InfluxDB + Grafana #1"
categories: [InfluxDB, Grafana]
tags: [InfluxDB, Grafana, time-series, db, dashboard]
description: influxDB와 잘 쓰이는 Grafana까지 같이 스터디
---


# InfluxDB


## 정의
InfluxDB는 시계열(Time-series) 데이터를 저장하는 데이터저장소이다.

시계열 데이터란 시간의 흐름에 따라 저장하는 데이터로서 서버, DB, 네트웍, 스토리지와 같은 IT인프라 모니터링을 위한 각종 데이터들, 서비스 반응을 확인하기위한 각종 지표들(동시접속자, PV 등), 요즘 뜨고 있는 IOT기기들의 각종 수집 데이터들, 다이어트를 위해 매일 기록하는 내 몸무게 등등 활용 목적에 따라 무척 다양할 수 있다.

시계열DB는 이러한 시계열 데이터들을 `효율적`으로 `저장`할 목적으로 사용되며 Grafana 같은 대쉬보드 툴과 연계하여 모니터링 용도로 주로 사용된다.

대체로 다음과 같은 구조로 모니터링 코드를 작성한다
InfluxDB(저장), Telegraf(수집), Grafana(대시보드)

## Data Structure

database : set of measurements

measurement : table

tag : indexed column

field : no indexed column

InfluxDB는 Schemaless 구조 tag, field를 생성하는 DDL를 수행하지 않고 바로 Insert를 수행할 수 있다.

insert 문은 다음과 같이 쓰여진다.
insert schemaName,tagKey=tagValue,tagKey2=tagValue fieldKey=fieldValue,fieldKey2=fieldValue2...
(Tag와 Field 사이에는 띄어쓰기를 넣어야 한다.)

하지만 출력 결과가 Tag와 Field가 구분되어 나오지 않고 단순히 알파벳 순서로 나열되어 나오게 된다.

## InfluxDB의 장점?

1. Continuous Queries를 지원한다.<br>
정해준 시간마다, 해당 query를 실행해서 그 결과 값을 지정하는 테이블의 특정 칼럼으로 저장하게 해준다.<br>
이걸 이용하면 분석, 통계 데이터를 쌓을 때, 스케쥴러를 통해 많이 하는 작업을 DB에서 한방에 해결할 수 있다.

2. SQL-like query language을 지원한다. 

## InfluxDB의 단점?

1. 데이타를 compression하기 때문에 읽기와 삭제에 느릴 수 있다.



# Grafana

InfluxDB와 자주, 함께 쓰이는 대시보드이다.
Graphite / Elasticsearch / InfluxDB 등을 이용하여  타임시리즈를 그래프 / 대쉬보드로 가시화해주는 툴이다.

DataSource를 선택하고, Query 설정(GroupBy, From, Select-field/mean/math 등)을 설정해주면 다음과 같이 그래프를 바로 확인할 수 있다.

![Grafana](http://docs.grafana.org/assets/img/blog/v2.6/influxdb_editor_v3.gif)

![Grafana](http://file.okky.kr/images/1460959016054.PNG)

# 사용하는 방법?
