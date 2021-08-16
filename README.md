# cloudStudy
Cloud Study AWS GCP AZURE

---

## Cloud Front
[Cloud Front란?-1](https://galid1.tistory.com/355)  
[Cloud Front란?-2](https://happyer16.tistory.com/entry/CloudFront%EB%9E%80)

### Cloud Front?
html, css, js 및 이미지 파일과 같은 정적 및 동적 웹 컨텐츠를 사용자에게 더 빨리 배포하도록 지원하는 웹서비스라고 한다.

정의에 나온대로 가장 대표적인 사용사례가 정적 웹 사이트 콘텐츠 전송 속도 향상이다.

CloundFront에서 콘텐츠를 어떻게 제공하길래 빨라지는 걸까?

![image](https://user-images.githubusercontent.com/37138188/129475085-55a3857b-3f48-4e15-83b1-8c5f43d48447.png)
![image](https://user-images.githubusercontent.com/37138188/129475094-7bea809d-a779-4ea0-bcd4-d1bb0f891205.png)

---

### CloudFront에서 콘텐츠를 제공하는 방법
1. 사용자가 웹사이트에 접속하면 이미지 파일 및 HTML 파일을 요청합니다.
2. DNS가 요청을 최적으로 서비스 할 수 있는 CloudFront 엣지 로케이션으로 요청을 라우팅합니다. (지연시간과 관련해 가장 가까운 CloudFront 엣지 로케이션임)
3. 엣지 로케이션에서 CloudFront는 해당 캐시에 요청된 파일이 있는지 확인한다. 파일이 캐시에 없으면 다음을 수행한다.
   a. CloudFront는 파일 형식에 적절한 오리진 서버( 이미지파일- S3 버킷 / HTML파일 - HTTP 서버)로 전달한다.
   b. 오리진 서버는 다시 CloudFront로 파일을 보낸다.
   c. 오리진에서 첫번째 바이트가 도착하면 CloudFront가 파일을 사용자에게 전달하기 시작한다. 이때 해당 파일을 캐시에 파일을 추가한다.

---

### Cloud Front 보안 취약점

1. 접근 관리
- CloudFront 배포에 지리적 제한이 설정되어 있는지 확인하시오.
- CloudFront 배포에 AWS WAF 웹 ACL이 설정되어 있는지 확인하시오.
- CloudFront 배포의 S3 오리진에 버킷 액세스 제한이 설정되어 있는지 확인하시오.
2. 서비스 관리
- CloudFront 배포의 S3가 아닌 오리진에 최소 오리진 프로토콜이 "TLSv1.1" 또는 "TLSv1.2" 이상으로 설정되어 있는지 확인하시오.
- CloudFront 배포의 S3가 아닌 오리진에 오리진 프로토콜 정책이 "HTTPS 만" 또는 "매치 뷰어"로 설정되어 있는지 확인하시오.
- CloudFront 배포의 Behaviors의 뷰어 프로토콜 정책이 "HTTP를 HTTPS로 리다이렉션" 또는 "HTTPS 만"으로 설정되어 있는지 확인하시오.
3. 로그 관리
- CloudFront 배포에 실시간 로그 활성화되어 있는지 확인하시오.
4. 암호화
- CloudFront 웹 배포의 각 Behavior에 필드 수준 암호화를 실행하는지 확인하시오.

---

#### CloudFront 배포에 지리적 제한이 설정되어 있는지 확인하시오.
지리적 차단이라고도 하는 지리적 제한을 사용하여 특정 지리적 위치에 있는 사용자가 CloudFront 배포를 통해 배포한 콘텐츠에 액세스하는 것을 차단할 수 있습니다. CloudFront 지리적 제한 기능을 사용하여 화이트리스트에 있는 국가의 액세스를 허용하거나, 블랙리스트에 있는 국가의 액세스를 금지할 수 있습니다.
특정한 국가의 액세스를 금지해야 하는 경우, 이러한 지리적 제한이 설정되어야 합니다.

확인 방법 : Distribution Settings - Restrictions

---

#### CloudFront 배포에 AWS WAF 웹 ACL이 설정되어 있는지 확인하시오.
AWS WAF는 가용성에 영향을 주거나, 보안을 위협하거나, 리소스를 과도하게 사용하는 일반적인 웹 공격으로부터 웹 애플리케이션이나 API를 보호하는 데 도움이 되는 웹 애플리케이션 방화벽입니다. 
웹 액세스 제어 목록 (웹 ACL)을 사용하면 보호 된 리소스가 응답하는 웹 요청을 세밀하게 제어 할 수 있습니다. 출처 IP 주소, 출처 국가, 일부로 포함된 문자열 일치 또는 정규 표현식(정규식) 일치, 특정 부분의 크기, 악성 SQL 코드 또는 스크립팅 감지와 같은 기준으로 요청을 허용 또는 차단할 수 있습니다.
따라서, CloudFront 배포에 웹 ACL이 설정되어야 합니다.

확인 방법 : Distribution Settings - General

---

#### CloudFront 배포의 S3 오리진에 버킷 액세스 제한이 설정되어 있는지 확인하시오.
배포를 만들 때 CloudFront가 파일에 대한 요청을 보내는 위치를 지정합니다. CloudFront는 오리진으로 여러 AWS 리소스를 사용하는 것을 지원합니다. Amazon S3를 배포의 오리진으로 사용하는 경우 CloudFront에서 제공하려는 모든 객체를 Amazon S3 버킷에 배치합니다.
버킷 액세스 권한을 통해 버킷의 객체에 액세스할 수 있는 사용자와 그러한 사용자가 갖는 액세스 유형을 지정할 수 있습니다. 객체 액세스 권한을 통해 객체에 액세스할 수 있는 사용자와 그러한 사용자가 갖는 액세스 유형을 지정할 수 있습니다.
따라서, S3 오리진에 버킷 액세스 제한을 설정해야 합니다.

**Origin이 S3일 경우**
사용자가 CloudFront를 통해 액세스 할 수 있지만 Amazon S3 URL을 사용하여 직접 액세스 할 수 없도록 Amazon S3 버킷의 콘텐츠를 선택적으로 보호 할 수 있습니다. 이렇게하면 CloudFront를 우회하고 Amazon S3 URL을 사용하여 액세스를 제한하려는 콘텐츠를 가져올 수 없습니다. 이 기능은 서명 된 URL을 사용하는 데 필요하지 않지만 권장됩니다.

확인 방법 : Distribution Settings - Behaviors - Restrict Bucket Access

---

#### CloudFront 배포의 S3가 아닌 오리진에 최소 오리진 프로토콜이 "TLSv1.1" 또는 "TLSv1.2" 이상으로 설정되어 있는지 확인하시오.
최종 사용자가 CloudFront에 보낸 요청의 프로토콜, CloudFront 콘솔에서 오리진 프로토콜 정책 필드의 값 또는 CloudFront API를 사용 중인 경우 DistributionConfig 복합 형식의 OriginProtocolPolicy 요소를 바탕으로 HTTP 또는 HTTPS 요청을 오리진 서버에 전달합니다. CloudFront 콘솔에는 HTTP만 해당, HTTPS만 해당 및 최종 사용자 일치 옵션이 있습니다.
CloudFront에서는 SSLv3, TLSv1.0, TLSv1.1 및 TLSv1.2 프로토콜을 사용하여 HTTPS 요청을 오리진 서버에 전달합니다. 사용자 지정 오리진의 경우, 오리진과 통신할 때 CloudFront에서 사용하려는 SSL 프로토콜을 선택할 수 있습니다.
이때, SSL 및 TLS 1.0은 안전하지 못하므로 프로토콜이 최소 1.1 또는 1.2 버전으로 설정해야 합니다.

**Origin이 ELB일 경우**
사용자가 TLSv1.1 이상을 지원하지 않는 브라우저 또는 디바이스를 사용하지 않는 한 CloudFront 배포 보안 정책의 최소 프로토콜 버전으로 TLSv1.1을 사용할 것을 권장합니다. Cloudfront 배포가 CloudFront 엣지 로케이션과 사용자 지정 오리진 간의 HTTPS 통신에 안전하지 않은 SSL 프로토콜을 사용하지 않도록 설정해야 합니다. SSLv3 프로토콜은 덜 안전하므로 오리진이 TLSv1 이상을 지원하지 않는 경우에만 SSLv3를 선택하는 것이 좋습니다.

확인 방법 : Distribution Settings - Behaviors - Minimum Origin SSL Protocol

---

#### CloudFront 배포의 S3가 아닌 오리진에 오리진 프로토콜 정책이 "HTTPS 만" 또는 "매치 뷰어"로 설정되어 있는지 확인하시오.
최종 사용자가 CloudFront에 보낸 요청의 프로토콜, CloudFront 콘솔에서 오리진 프로토콜 정책 필드의 값 또는 CloudFront API를 사용 중인 경우 DistributionConfig 복합 형식의 OriginProtocolPolicy 요소를 바탕으로 HTTP 또는 HTTPS 요청을 오리진 서버에 전달합니다. CloudFront 콘솔에는 HTTP만 해당, HTTPS만 해당 및 최종 사용자 일치 옵션이 있습니다.
최종 사용자 일치를 지정하는 경우, CloudFront에서는 최종 사용자 요청의 프로토콜을 사용하여 오리진 서버에 요청을 전달합니다. 최종 사용자가 HTTP 및 HTTPS 프로토콜 모두를 사용하여 요청하더라도 CloudFront에서는 한 번만 객체를 캐싱합니다.
따라서, 안전한 프로토콜 정책을 설정하기 위해 HTTPS만 또는, 사용자가 선택할 수 있도록 매치 뷰어로 설정해야 합니다.

**Origin이 ELB일 경우**
사용자가 TLSv1.1 이상을 지원하지 않는 브라우저 또는 디바이스를 사용하지 않는 한 CloudFront 배포 보안 정책의 최소 프로토콜 버전으로 TLSv1.1을 사용할 것을 권장합니다. Cloudfront 배포가 CloudFront 엣지 로케이션과 사용자 지정 오리진 간의 HTTPS 통신에 안전하지 않은 SSL 프로토콜을 사용하지 않도록 설정해야 합니다. SSLv3 프로토콜은 덜 안전하므로 오리진이 TLSv1 이상을 지원하지 않는 경우에만 SSLv3를 선택하는 것이 좋습니다.

확인 방법 : Distribution Settings - Behaviors - Origin Protocol Policy

---

#### CloudFront 배포의 Behaviors의 뷰어 프로토콜 정책이 "HTTP를 HTTPS로 리다이렉션" 또는 "HTTPS 만"으로 설정되어 있는지 확인하시오.
최종 사용자가 CloudFront에 보낸 요청의 프로토콜, CloudFront 콘솔에서 오리진 프로토콜 정책 필드의 값 또는 CloudFront API를 사용 중인 경우 DistributionConfig 복합 형식의 OriginProtocolPolicy 요소를 바탕으로 HTTP 또는 HTTPS 요청을 오리진 서버에 전달합니다. CloudFront 콘솔에는 HTTP만 해당, HTTPS만 해당 및 최종 사용자 일치 옵션이 있습니다.
따라서, 안전한 프로토콜 정책을 설정하기 위해 HTTP를 HTTPS로 리다이렉션 또는, HTTPS만으로 설정해야 합니다.

확인 방법 : Distribution Settings - Behaviors - Viewer Protocol Policy

---

#### CloudFront 배포에 실시간 로그 활성화되어 있는지 확인하시오.
CloudFront 실시간 로그를 사용하여 배포에 대해 이루어진 요청에 대한 정보를 실시간으로 제공합니다(로그는 요청을 받은 후 몇 초 내에 전달됨). 실시간 로그를 사용하여 콘텐츠 전송 성능을 모니터링 및 분석하고 이에 기초해 조치를 취할 수 있습니다. 실시간 로그 구성에는 이름, 샘플링 비율, 필드, 엔드포인트, IAM 역할이 포함되어 있습니다.
따라서, 이러한 실시간 정보를 사용하기 위해서는 실시간 로그가 활성화되어 있어야 합니다.

확인 방법 : Distribution Settings - Behaviors - Enable Real-time Logs

---

#### CloudFront 웹 배포의 각 Behavior에 필드 수준 암호화를 실행하는지 확인하시오.
Amazon CloudFront를 사용하면 HTTPS를 통해 오리진 서버에 대한 종단 간 보안 연결을 적용할 수 있습니다. 필드 레벨 암호화는 추가 보안 레이어를 추가하여 시스템 처리 전체에서 특정 데이터를 보호하고 특정 애플리케이션만 이를 볼 수 있도록 하여 사용자가 민감한 정보를 웹 서버에 안전하게 업로드할 수 있습니다. 
따라서, 안전한 업로드를 위해 필드 수준 암호화가 실행되어야 합니다.

확인 방법 : 왼쪽 사이드바 Security - Field-level encryption - Field-level encryption

---
