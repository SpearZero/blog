## 카카오 좌표로 주소 변환하기(스프링)

다시 블로그를 시작할겸 간단한 주제로 올리는 글..

현재 진행중인 프로젝트에서 외부 API를 이용해 다양한 정보를 사용자에게 보여줘야 합니다. 이 API에서는 좌표정보는 제공해주지만, 좌표에 대한 주소정보를 제공해주지 않아 직접 좌표를 주소로 변환해야 합니다. 주소변환에 카카오 API를 사용하기로 했습니다.

-------------------------------------------------------------------------

> https://developers.kakao.com/console/app

애플리케이션을 추가해서 앱 키 중 REST API키를 사용해야 합니다.

> https://developers.kakao.com/docs/latest/ko/local/dev-guide#coord-to-address

가이드가 자세히 나와있습니다.

```
// 가이드를 보고 만든 스프링 코드
@Override
public String coordToAddr(String lon, String lat) {
    /*
        String url = https://dapi.kakao.com/v2/local/geo/coord2address.json?input_coord=WGS84&x=12.12345&y=12.12345
    */
    String url = APIURL.COORD_ADDR_URL + lon + "&y=" + lat;
    HttpEntity<?> entity = kakaoHeader();
    
    ResponseEntity<?> result = 
        restTemplate.exchange(url, HttpMethod.GET, entity, Map.class);
    
    String address = "";
    if (result.getStatusCode() == HttpStatus.OK && result.getBody() != null) {
        try {
            String response = objectMapper.writeValueAsString(result.getBody());
            address = getAddress(response);
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
    
    return address;
}

// header 설정
private HttpEntity<?> kakaoHeader() {
    HttpHeaders header = new HttpHeaders();
    // header.add("Authorization", "KaKaoAK RESTAPIKEY")
    header.add("Authorization", APIKEY.COORD_ADDR_KEY);
    return new HttpEntity<>(header);
}

// gson을 이용해 도로명 주소 or 주소 파싱
private String getAddress(String json) throws Exception {
    String value = "";
    
    JsonObject addrResult = JsonParser.parseString(json).getAsJsonObject();
    JsonObject meta = addrResult.getAsJsonObject("meta");
    
    long totalCount = meta.get("total_count").getAsLong();
    
    if (totalCount <= 0) {
        return value;
    }
    
    JsonArray documents = addrResult.getAsJsonArray("documents");
    JsonObject addrObject = documents.get(0).getAsJsonObject();
    
    if (addrObject.get("road_address").isJsonNull()) {
        JsonObject address = addrObject.get("address").getAsJsonObject();
        value = address.get("address_name").getAsString();
        return value;
    } 
    
    JsonObject roadAddress = addrObject.get("road_address").getAsJsonObject();
    value = roadAddress.get("address_name").getAsString();
    return value;
}
```
자바스크립트(jquery AJAX)로 API 호출

------------------------------------------------------------------------
```

$.ajax({
    type : 'get',
    url : 'https://dapi.kakao.com/v2/local/geo/coord2address.json?input_coord=WGS84&x=12.12345&y=12.12345',
    dataType : 'json',
    async: false,
    beforeSend : function(xhr) {
        xhr.setRequestHeader("Authorization", "KakaoAK RESTAPI키")
    },
    success : function(result) {
        let addressName = '';
        let totatlCount = result.meta.total_count;
        if (totatlCount > 0) {
            if (result.documents[0].road_address === null) {
                addressName = result.documents[0].address.address_name;
            } else {
                addressName = result.documents[0].road_address.address_name;
            }
        }
    },
    error:function(request, status, error) {
        alert(error);
    }
})
```