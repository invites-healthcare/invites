# Care8 DNA, GsShop API 연동 예제
기본적으로 RestFul 방식으로 json을 데이터로 통신합니다. <br>
저희는 RestFul 라이브러리로 자체 통신 오픈소스 JsonWeb을 사용합니다. <br>
다른 라이브러리도 방식이 비슷하니 참고용으로 사용하시기 바랍니다.

<br>

## [API 문서 이동](http://49.50.162.235:8081/swagger-ui.html#/17._Care+)

<br>

## [Test apk 다운로드](https://github.com/invites-healthcare/invites/raw/master/20211119152918-v41(1.3.8)-debug.apk)

<br>

## 정보조회
- 호스트 
  - 개발서버 : http://49.50.162.235:8081
  - 운영서버 : https://app.care8.co.kr
- URI : /v1/careplus/gsshop/dna
- Method : GET
- Parameter : userIdx(int)

### dto 객체 
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
    private String trait;   //유전자 특성 영문명
    private String traitKo; //유전자 특성 한글명
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
      "trait": "VitaminA",
      "traitKo": "비타민 A 농도",
    },
    {
      "trait": "VitaminC",
      "traitKo": "비타민 C 농도",
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
- 호스트 
  - 개발서버 : http://49.50.162.235:8081
  - 운영서버 : https://app.care8.co.kr
- URI : /v1/careplus/gsshop/inquiry
- Method : POST
- Parameter : InquiryDto

### dto 객체 
```java
class InquiryResult{
    private int status;          //호출 결과(성공 200)
    private String path;         //uri
    private String timestamp;    //호출시간
    private String message;      //결과/에러 메세지
    private InquiryDto contents;

    //...getter, setter 생략
}

class InquiryDto{
    private int int userIdx;        //care8 userIdx
    private int gsShopUserIdx;      //gsshop userIdx
    private int q1Height;           //키
    private int q1Weight;           //몸무게
    private List<Integer> q2;       //문진 2번(여러가지 선택)
    private List<Integer> q3_1;     //문진 3_1번(여러가지 선택)
    private List<Integer> q3_2;     //문진 3_2번(여러가지 선택)
    private List<Integer> q3_3;     //문진 3_3번(여러가지 선택)
    private List<Integer> q3_4;     //문진 3_4번(여러가지 선택)
    private List<Integer> q3_5;     //문진 3_5번(여러가지 선택)
    private List<Integer> q3_6;     //문진 3_6번(여러가지 선택)
    private List<Integer> q3_7;     //문진 3_7번(여러가지 선택)
    private List<Integer> q3_8;     //문진 3_8번(여러가지 선택)
    private int q4;                 //문진 4번
    private int q5;                 //문진 5번
    private int q6;                 //문진 6번
    private List<Integer> q7;       //문진 3_7번(여러가지 선택)
    
    //...getter, setter 생략
}
```

### API 호출
```java
   String host = "http://49.50.162.235:8081/v1/careplus/gsshop/inquiry";
   
   //Dto에 문진 값 입력
   InquiryDto inquiryDto = new InquiryDto();
   inquiryDto.setQ1Height(183); //키
   inquiryDto.setQ1Height(75);  //몸무게
   int[] q2 = {1,3};            
   inquiryDto.setQ2(q2);        //2번 문진
   
   JsonWeb web = new JsonWeb();
   web.host(host);
   
   HttpResult httpResult = web.Post(inquiryDto); //Post방식 호출
   
   if(httpResult.code == 200){        //통신결과가 성공일때(웹서버)
     DnaResult dnaResult = httpResult.toModel(); // DnaResult를 가져온다.
      if(dnaResult.getStatus() == 200){              // 조회 결과가 성공일때(로직,시스템)
        System.out.println("문진결과 입력 완료");
      }else{
        System.out.println(dnaResult.message()); //에러메세지 출력
      }
   }  
```


