## 접근 제한 시나리오
콘솔에 접근하여 클라우드 제품 관련 조작을 할 때 아래 그림과 같이 "관련 리소스를 조작할 권한이 없습니다"라는 알림이 나타날 수 있습니다.
![](https://main.qcloudimg.com/raw/d0b3aa8555810a16c0117f72bfc3e355.png)
이는 로그인한 서브 사용자 또는 협력자가 관련 권한을 부여받지 않았기 때문입니다. 기본 계정이 해당 권한을 부여한 후에야 정보를 확인하거나 관련 조작을 수행할 수 있습니다.

## 권한 부여 절차
1. 부여 받을 권한을 확인합니다.
  실패 정보 설명에서 보유하지 않은 권한을 나타냅니다. 위 그림과 같이 CloudAudit의 LookupEvents API에 대한 권한이 없으므로 페이지의 내용을 확인할 수 없습니다.

2. 기본 계정 또는 관리 권한이 있는 서브 계정을 사용하여 해당 서브 사용자 또는 협력자에게 관련 권한을 부여합니다.

   1. [CAM 관리 콘솔](https://console.cloud.tencent.com/cam)에 로그인하고 **전략 관리** 페이지로 이동하여 [사용자 지정 전략 생성] > [전략 생성기로 생성]을 클릭하여 특정 API 권한(예: LookupEvents)을 부여합니다.
   2. 전략 관리 페이지에서 해당 제품의 시스템 전략을 조회하고 서브 사용자에게 사전 설정된 시스템 전략(예: 이 예제에서 CloudAudit와 관련된 사전 설정 전략)을 부여합니다.
![](https://main.qcloudimg.com/raw/72438c9189b88bcd0833ccd039abcdb0.png)
