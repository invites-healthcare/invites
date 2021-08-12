# Care8 DNA, GsShop API 연동 예제
기본적으로 RestFul 방식으로 json을 데이터로 통신합니다. <br>
저희는 RestFul 라이브러리로 자체 통신 오픈소스 JsonWeb을 사용합니다.
다른 라이브러리도 방식이 비슷하니 참고용으로 사용하시기 바랍니다.

## 정보조회
- 호스트 : http://49.50.162.235:8081 (개발서버)
- URI : /v1/careplus/gsshop/dna
- Method : GET
- Parameter : userIdx(int)

### Vo 객체 
```java
class DnaResult{
    private int status;          //호출 결과(성공 200)
    private String path;         //uri
    private String timestamp;    //호출시간
    private String message;      //결과/에러 메세지
    private List<DnaDto> contents;

    //...getter, setter 생략
}

class DnaDto {
    private String trait;   //유전자 특성 코드
    private String traitKo; //유전자 특성 한글명
    private String grade;   //등급(주의)
    private int score;      //점수

    //...getter, setter 생략
}
```

### API 호출
```java
   String int = "145";
   String host = "http://49.50.162.235:8081/v1/careplus/gsshop/dna?userIdx="+userIdx;
   
   JsonWeb web = new JsonWeb();
   web.host(host);
   
   HttpResult httpResult = web.GET(); //get방식 호출
   
   if(httpResult.code == 200){        //통신결과가 성공일때(웹서버)
     DnaResult dnaResult = httpResult.toModel(); // DnaResult를 가져온다.
      if(dnaResult.getStatus() == 200){              // 조회 결과가 성공일때(로직,시스템)
        List<DnaDto> contents = dnaResult.getContents();
        System.out.println("DnaDto List 데이터 저장"); //결과 처리
      }else{
        System.out.println(dnaResult.message()); //에러메세지 출력
      }
   }  
```
```json
{
  "contents": [
    {
      "trait": "omega_6_fatty_acid_levels",
      "traitKo": "지방산 농도 (오메가-6)",
      "score": 10,
      "grade": "주의"
    },
    {
      "trait": "vitamin_d_levels",
      "traitKo": "비타민 D 농도",
      "score": 23,
      "grade": "주의"
    },
    {
      "trait": "vitamin_d_levels",
      "traitKo": "비타민 D 농도",
      "score": 23,
      "grade": "주의"
    },
    {
      "trait": "vitamin_d_levels",
      "traitKo": "비타민 D 농도",
      "score": 23,
      "grade": "주의"
    }
  ],
  "message": "정상처리 되었습니다.",
  "path": "/v1/careplus/gsshop/dna",
  "timestamp": "2021-08-12 13:57:19",
  "status": 200,
  "error": null,
  "trace": null
}
```

<br>

## 문진결과 입력


