# **Azure Search**
- Azure 내 Saas 형태로 제공되는 검색엔진 stack
- CloudSearch in AWS
- DB등의 index를 이용하여 해당 데이터 검색
- portal, Rest api, .net SDK 제공

## Index & Indexer
- table 형태
- index import 가능 대상 : Azure 내 Sql DB, storage, Doc. DB
- 수정불가

## fields
- index 내 colum 형태
- 저장할 data 형태에 따라 type 지정

## Search
- simple query syntax : and(+) or(|) not(-)
- lucene query syntax

## filter
- OData expression

## scoring
- 검색어와 일치울 표현
- 동일 스코어 내 not sort

## custom score
- 검색시 특정 필드에 가중치를 주거나, 함수를 이용해 가중치 부여

## [Language analyzer](https://docs.microsoft.com/ko-kr/rest/api/searchservice/language-support)
- text를 tokenize
- ms 기본, [lucene](http://lucene.apache.org/core/4_9_0/analyzers-common/index.html) 선택 가능

## suggestor
- index당 1개 생성 가능

 # DEMO

## 1.  Create Index (+ Default sample)

header
~~~
POST https://[service name].search.windows.net/indexes?api-version=2016-09-01
Content-Type: application/json
api-key: [api-key]
Content: application/json
~~~
body
~~~
 {
    "name": "hotels",  
    "fields": [
        {"name": "hotelId", "type": "Edm.String", "key": true, "searchable": false, "sortable": false, "facetable": false},
        {"name": "baseRate", "type": "Edm.Double"},
        {"name": "description", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false},
        {"name": "description_fr", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false, "analyzer": "fr.lucene"},
        {"name": "description_kr", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false, "analyzer": "ko.lucene"},
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

## 2. Delete Index

header
~~~
DELETE https://[service name].search.windows.net/indexes/hotels?api-version=2016-09-01
api-key: [api-key]
~~~

## 3. Query
### - simple query syntax
- search operator(and+ or| not-)
- searchmode
### - Lucene syntax


### - filter(OData)
- gt ge lt le eq ne
~~~
$filter=geo.distance(location, geography'POINT(-122.131577 47.678581)') le 10    
$filter=tags/all(t: t ne 'apartment residence') 
~~~
### order by
~~~
$orderby=beds asc, baths
~~~

### facet
~~~
&facet=beds,interval:1
~~~

## 4. scoring

## 5. analyzer
- ko.microsoft, ko.lucene
~~~
  POST https://[search service name].search.windows.net/indexes/[index name]/analyze?api-version=[api-version]
  Content-Type: application/json
    api-key: [admin key]

  {
     "analyzer":"ko.lucene",
     "text": "분석하고 싶은 텍스트를 입력하세요"
  }
~~~

 # 참조
 >[Azure Search Documentation](https://docs.microsoft.com/en-us/azure/search/)
 >[Azure 샘플 리포지토리](https://github.com/Azure-Samples/search-dotnet-asp-net-mvc-jobs)
 >[Azure Rest API](https://docs.microsoft.com/en-us/rest/api/searchservice/search-documents)