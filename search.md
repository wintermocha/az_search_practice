# **Azure Search?**
- Azure 내 Saas 형태로 제공되는 검색엔진
- CloudSearch in AWS
- DB등의 index를 이용하여 해당 데이터 검색
- portal, Rest api, .net SDK 제공
- 무료구독 가능


**특징**

단순 쿼리또는 lucene 쿼리 사용
1. 단순쿼리 simple query
- and (+)
- or (|)
- not (-)
- suffix(*)

2. lucene


언어 지원
- 56개 언어 지원
- 시제, 복수, 띄어쓰기 등

제안 검색(suggestion)

패싯 탐색

지리공간

필터
- OData expression

검색가능한 대상 : 
Azure Sql DB, storage, Doc. DB, 그 외 index를 생성하는 모든 서비스 연결 가능

검색 트래픽 분석

score profile
- 특정 검색 결과를 상위에 위치 시키거나, 특정 유저에 대해 다른 검색 결과 제공이 가능한 기능

 # DEMO

- 인덱스 생성

~~~
POST https://[service name].search.windows.net/indexes?api-version=2016-09-01
Content-Type: application/json
api-key: [api-key]
~~~
~~~
 {
    "name": "hotels",  
    "fields": [
        {"name": "hotelId", "type": "Edm.String", "key": true, "searchable": false, "sortable": false, "facetable": false},
        {"name": "baseRate", "type": "Edm.Double"},
        {"name": "description", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false},
        {"name": "description_fr", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false, "analyzer": "fr.lucene"},
        {"name": "hotelName", "type": "Edm.String", "facetable": false},
        {"name": "category", "type": "Edm.String"},
        {"name": "tags", "type": "Collection(Edm.String)"},
        {"name": "parkingIncluded", "type": "Edm.Boolean", "sortable": false},
        {"name": "smokingAllowed", "type": "Edm.Boolean", "sortable": false},
        {"name": "lastRenovationDate", "type": "Edm.DateTimeOffset"},
        {"name": "rating", "type": "Edm.Int32"},
        {"name": "location", "type": "Edm.GeographyPoint"}
    ]
}
~~~


 # 참조
 >[Azure Search Documentation](https://docs.microsoft.com/en-us/azure/search/)
