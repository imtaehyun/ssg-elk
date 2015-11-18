# Docker에 ELK구성


## Elasticsearch에 Mapping(data type)을 정의하는 방법
Kibana에 IP를 기반으로 한 위치정보를 지도에 표현하기 위해서 logstash에서 ip => geoip로 변환과정을 거쳤다. 그러나 Kibana에서는 geo_point형식이 아니라고 자꾸 에러가 나왔다. 엄청 찾아보니 결국 내용은 Template을 선언해서 미리 타입을 정의해 놓고 그 이후에 데이터를 넣어야 하는 것이었다.

방법1.
Elasticsearch 실행 -> curl호출하여 template 생성 -> 데이터 PUT
이때 Logstash 설정을 반드시 `manage_template => false`로 설정해야한다.
```
output {
  elasticsearch {
    ...생략...
    manage_template => false    
  }
}
```

방법2.
Elasticsearch 실행 -> Logstash 실행
이때 Logstash 설정이 필요하다
```
output {
  elasticsearch {
    ...생략...
    manage_template => true
    template => "/etc/logstash/conf.d/login-template.json"
    template_name => "login-template"
  }
}
```

- [Template 예제](https://github.com/logstash-plugins/logstash-output-elasticsearch/blob/master/lib/logstash/outputs/elasticsearch/elasticsearch-template.json)
- [Logstash output 설정 ](https://www.elastic.co/guide/en/logstash/current/plugins-outputs-elasticsearch.html#plugins-outputs-elasticsearch-manage_template)
- [Put Mapping](https://www.elastic.co/guide/en/elasticsearch/reference/current/indices-put-mapping.html)
- [How To Map User Location with GeoIP and ELK](https://www.digitalocean.com/community/tutorials/how-to-map-user-location-with-geoip-and-elk-elasticsearch-logstash-and-kibana)
- [Dynamic Template이란](http://jjeong.tistory.com/1007)

## 참고사이트
- [CoreOS Windows Vagrant File Sharing](http://micahasmith.github.io/2015/01/22/coreos-vagrant-windows-file-share/)
- [Logstash Doc](https://www.elastic.co/guide/en/logstash/current/index.html)
- [Storm과 Elasticsearch를 활용한 로깅 플랫폼의 실시간 알람 시스템 구현](http://www.slideshare.net/deview/241-storm-elasticsearch)
- [Elasticsearch 튜토리얼 따라해보기](http://hyeonjae.github.io/elasticsearch/2015/06/29/elasticsearch.html)
- [How To Install Elasticsearch, Logstash, and Kibana 4 on CentOS 7](https://www.digitalocean.com/community/tutorials/how-to-install-elasticsearch-logstash-and-kibana-4-on-centos-7)
- [elasticsearch로 로그 검색 시스템 만들기](http://d2.naver.com/helloworld/273788)
- [시작하세요! 엘라스틱서치](https://github.com/wikibook/elasticsearch)
- [Elasticsearch Nodejs Library](https://github.com/elastic/elasticsearch-js)
- [How to Configure the Logstash Date filter](http://blog.eagerelk.com/how-to-configure-the-date-logstash-filter/)
- http://www.slideshare.net/AmazeeAG/2014-0422-loggingwithlogstashbastianwidmercampusbern
