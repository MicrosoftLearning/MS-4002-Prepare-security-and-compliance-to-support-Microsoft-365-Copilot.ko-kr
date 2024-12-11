# 학습 경로 1 - 랩 4 -연습 1 - Microsoft Entra ID Protection을 사용하여 민감도 레이블 구현

Adatum의 새로운 Microsoft 365 관리자인 Holly Dickson의 역할로 가상화된 랩 환경에서 Microsoft 365를 배포했습니다. Microsoft 365 파일럿 프로젝트를 계속 진행하면서 다음에 수행할 단계는 Adatum에서 Microsoft Purview Information Protection을 사용하여 민감도 레이블을 구현하는 것입니다. 이 랩에서는 레이블을 만들고 게시하며 게시된 레이블을 테스트합니다. 하지만 이때 이 랩에서 만든 레이블을 테스트하지는 않습니다. 다른 레이블을 테스트합니다.

**중요:** 새로 게시한 민감도 레이블 및 레이블 정책을 Microsoft 365 전체에 전파하는 데에는 최대 24시간이 걸릴 수 있습니다. 따라서 이 랩에서 만든 레이블을 테스트할 수는 없습니다. 대신 **Project - Falcon**이라는 기존 민감도 레이블을 테스트합니다. 이 기존 레이블은 만들 레이블과 거의 동일하므로, 만든 레이블을 테스트할 수 있다면 기본적으로 동일한 결과를 확인할 수 있습니다. 


### 작업 1 - Microsoft Purview Information Protection 클라이언트 설치

Adatum 파일럿 프로젝트의 일부로 민감도 레이블을 구현하려면 먼저 Microsoft 다운로드 센터에서 **Microsoft Purview Information Protection 클라이언트**를 설치해야 합니다.

1. 이전 랩이 끝날 무렵에는 LON-CL2에서 작업했습니다. **LON-CL1**으로 전환합니다.  <br/>

    LON-CL1에는 계속 로컬 **adatum\administrator** 계정으로 로그인되어 있고, Edge 브라우저에서는 Microsoft 365에 계속 **Holly Dickson**으로 로그인되어 있어야 합니다. 

2. **Microsoft Edge**에서 새 탭을 열고 주소 표시줄에 다음 URL을 입력합니다. **https://www.microsoft.com/en-us/download/confirmation.aspx?id=53018**  <br/>

    그러면 **Microsoft Purview Information Protection 클라이언트** 다운로드가 시작됩니다.

3. 페이지 오른쪽 위에 표시되는 **다운로드** 창에서 **PurviewInfoProtection.exe** 파일이 다운로드 중인 것을 확인할 수 있습니다. 파일 다운로드가 완료되면 파일 이름 아래에 표시되는 **파일 열기** 링크를 선택합니다.

4. **Microsoft Azure Information Protection** 마법사가 열립니다. 마법사가 바탕 화면에 표시되지 않으면 작업 표시줄에서 마법사 아이콘을 선택하여 마법사를 표시합니다.

5. 마법사를 열면 나타나는 **Microsoft Purview Information Protection 클라이언트 설치** 창에서 **Office용 AIP 추가 기능 제거 승인(필수)** 확인란을 선택하고 **Microsoft Purview Information Protection 개선을 돕기 위해 사용 통계를 Microsoft에 전송** 확인란의 선택을 해제(취소)합니다. 그런 다음 **동의** 버튼을 선택합니다.

6. 설치가 완료된 후 **닫기**를 선택합니다.

LON-CL1 VM에 **Microsoft Purview Information Protection 클라이언트**를 성공적으로 설치했습니다.


### 작업 2 - SharePoint 및 OneDrive에서 파일에 민감도 레이블 사용

이 연습에서는 SharePoint 및 OneDrive에서 지원되는 Office 파일 및 PDF 파일에 민감도 레이블을 사용하도록 설정합니다. 이 기능을 사용하도록 설정하면 리본에 **민감도** 버튼이 표시되어 사용자가 레이블을 적용할 수 있습니다. 또한 상태 표시줄에 적용된 레이블 이름이 표시됩니다. SharePoint의 경우 세부 정보 창에서도 사용자가 민감도 레이블을 보고 적용할 수 있습니다.

또한 이 기능을 사용하도록 설정하면 SharePoint 및 OneDrive에서 민감도 레이블을 사용하여 암호화된 Office 파일과, 선택에 따라서는 PDF 문서의 콘텐츠도 처리할 수 있게 됩니다. 레이블은 웹용 Office 또는 Office 데스크톱 앱에서 적용하고, SharePoint 및 OneDrive에 업로드하거나 저장할 수 있습니다. 이 기능을 사용하도록 설정하기 전에는 해당 서비스에서 암호화된 파일을 처리할 수 없습니다. 즉 암호화된 파일에는 공동 작성, eDiscovery, 데이터 손실 방지, 검색, 기타 공동 작업 기능이 작동하지 않습니다.

먼저 SharePoint 및 OneDrive에 저장된 Office 온라인 파일에서 민감도 레이블을 사용할 수 있도록 설정해 보겠습니다. 그 다음에는 PDF에 대한 지원을 사용하도록 설정합니다.

**참고:** SharePoint 및 OneDrive의 모든 테넌트 수준 구성 변경과 마찬가지로, 이번에도 변경 내용을 적용하는 데 약 15분이 걸립니다.

1. LON-CL1의 Edge 브라우저에서 여전히 Microsoft 365에 **Holly Dickson**으로 로그인한 상태여야 합니다.

2. Edge 브라우저에 **Microsoft 365 관리 센터** 탭이 계속 열려 있어야 합니다. 그렇지 않은 경우 새 탭을 열고 다음 URL을 입력합니다. **https://admin.microsoft.com** 

3. **Microsoft 365 관리 센터**에서 필요한 경우 **... 모두 표시**를 선택합니다. **관리 센터** 그룹에서 **규정 준수**를 선택합니다. 그러면 새 탭에서 Microsoft Purview 포털이 열립니다.

4. 먼저 SharePoint 및 OneDrive에 저장된 Office 온라인 파일에서 민감도 레이블을 사용하도록 설정해 보겠습니다. <br/>

    **Microsoft Purview** 포털의 탐색 창에 있는 **솔루션** 섹션에서 **Information Protection**을 선택한 다음 **민감도 레이블**을 선택합니다.

5. **민감도 레이블** 페이지 중앙에 다음 메시지가 표시되어야 합니다. **조직에서 암호화된 민감도 레이블이 적용된 상태로 OneDrive 및 SharePoint에 저장된 Office 온라인 파일의 콘텐츠를 처리하는 기능을 활성화하지 않았습니다. 여기에서 활성화할 수 있지만, Multi-Geo 환경에 대한 추가 구성이 필요합니다.** <br/>

    이 메시지 아래에는 **지금 켜기** 버튼이 있습니다. 이 단추를 선택합니다.  <br/>

    **참고:** 명령은 즉시 실행되며 페이지를 다시 새로 고치면 메시지 또는 버튼이 더 이상 표시되지 않습니다.

6. 이제 SharePoint 및 OneDrive의 파일에 대해 PDF 보호를 사용하도록 설정해 보겠습니다. <br/>

    **Microsoft Purview** 포털의 탐색 창에 있는 **Information Protection**에서 **자동 레이블 지정**을 선택합니다.

7. **자동 레이블 지정** 페이지 중앙에 **자동 레이블 지정으로 PDF 보호** 배너가 표시되어야 합니다. **자동 레이블 지정으로 PDF 보호** 제목을 선택하여 SharePoint 및 OneDrive의 파일에 대한 PDF 보호를 활성화합니다. 

8. 표시되는 **자동 레이블 지정 정책** 대화 상자에서 **확인**을 선택하여 SharePoint 및 OneDrive의 파일에 대해 PDF 보호 활성화를 확인합니다. 

    **참고:** 명령은 즉시 실행되며 페이지를 다시 새로 고치면 **자동 레이블 지정으로 PDF 보호** 배너가 더 이상 표시되지 않습니다.

9. Edge 브라우저를 모든 탭과 함께 열어 둡니다. 


### 작업 3 - 민감도 레이블 만들기

이 연습에서는 민감도 레이블을 만들고 기본 정책에 추가하여 Adatum 테넌트의 모든 사용자에게 유효하도록 합니다.

1. LON-CL1의 Edge 브라우저에서 여전히 Microsoft 365에 **Holly Dickson**으로 로그인한 상태여야 합니다.

2. Edge 브라우저에는 이전 작업에서 연 **Microsoft Purview** 포털 탭이 계속 열려 있어야 합니다. **Microsoft Purview** 포털의 탐색 창에 있는 **Information protection**에서 **민감도 레이블**을 선택합니다. 

3. **민감도 레이블** 페이지에서 화면 가운데 메뉴 모음에 표시되는 **+레이블 만들기** 옵션을 선택합니다. 그러면 **새 민감도 레이블** 마법사가 시작됩니다.

4. **새 민감도 레이블** 마법사의 **이 레이블에 대한 기본 정보 입력** 페이지에서 다음 정보를 입력합니다.

    - 이름: **PII**
    
    - 표시 이름: **PII**

    - 사용자를 위한 설명: **Documents, files, and emails with PII**

    - 관리자를 위한 설명: **Documents, files, and emails with PII**

    - 레이블 색: 민감도 레이블에 할당할 색을 하나 선택합니다.

5. **다음**을 선택합니다.

6. **이 레이블의 범위 정의** 페이지에서 **항목** 확인란과 그 아래의 **파일**, **이메일**, **모임** 확인란이 선택되어 있는지 확인합니다(필요한 경우 이 네 가지 확인란 모두 선택). 이러한 확인란을 사용하면 해당 보호 설정을 구성할 수 있도록 이 민감도 레이블을 사용할 위치를 정의할 수 있습니다. **다음**을 선택합니다.

7. **선택한 항목 유형에 대한 보호 설정 선택** 페이지에서 암호화 설정을 사용하여 레이블이 적용될 콘텐츠에 대한 액세스를 제한하는 프로세스를 시작할 수 있습니다. 문서, 이메일 또는 모임 초대가 암호화되어 있으면 콘텐츠에 대한 액세스가 제한되어 다음과 같이 됩니다.  <br/>

    - 레이블의 암호화 설정에 따라 권한이 부여된 사용자만 암호를 해독할 수 있습니다.
    - 파일의 이름이 변경된 경우에도 조직 내부 또는 외부의 어디에 있든 관계 없이 암호화된 상태로 유지됩니다.
    - 미사용(예: OneDrive 계정에서)과 전송 중(예: 인터넷을 통해 전송되는 전자 메일) 두 상태 모두에서 암호화됩니다.

   Holly는 암호화 설정을 구성하려고 하므로 **선택한 항목 유형에 대한 보호 설정 선택** 페이지에서 **액세스 제어** 확인란을 선택합니다. 또한 나중에 머리글, 바닥글 및 워터마크 설정을 구성할 수 있도록 **콘텐츠 표시 적용** 확인란을 선택합니다. **다음**을 선택합니다.

8. 이전 페이지에서 **액세스 제어** 옵션을 선택했으므로 이제 **액세스 제어** 페이지가 나타납니다. 이 페이지에서는 레이블이 암호화를 적용하는 방법을 구성할 수 있습니다. 다음 중 하나를 선택할 수 있습니다.  <br/>

    - **항목에 이미 적용된 경우 액세스 제어 설정 제거:** 이 옵션을 선택하고 레이블을 적용하면 민감도 레이블과 독립적으로 적용된 경우에도 기존 암호화가 제거됩니다. 이 설정으로 인해 사용자에게 기존 암호화를 제거할 수 있는 충분한 권한이 없는 경우 민감도 레이블이 적용되지 않을 수 있다는 점을 이해하는 것이 중요합니다.  
    - **액세스 제어 설정 구성:** 이 옵션은 권한 관리를 사용하여 암호화를 설정합니다. 또한 다음 설정을 표시합니다. <br/>
        - 지금 권한을 할당하시겠습니까 아니면 사용자가 결정하도록 하시겠습니까?
        - 콘텐츠에 대한 사용자 액세스 만료
        - 오프라인 액세스 허용

    **참고:** 처음에는 암호화를 적용하지 않도록 민감도 레이블을 구성한 다음, 나중에 암호화를 적용하기 위해 일부 기존 레이블을 편집하는 것이 일반적인 배포 전략입니다. 이것이 Holly가 취하고 싶어하는 접근 방식입니다. <br/>

    따라서 **항목에 이미 적용된 경우 액세스 제어 설정 제거** 옵션을 선택한 다음, **다음**을 선택합니다.

9. 이전 페이지에서 **콘텐츠 표시 적용** 옵션을 선택했으므로 이제 **콘텐츠 표시** 페이지가 표시됩니다. **콘텐츠 표시** 토글 스위치를 **켜기**로 설정합니다. 이렇게 하면 파일 및 이메일을 표시하는 방법을 사용자 지정할 수 있는 세 가지 옵션이 표시됩니다. <br/>

    세 확인란을 모두 선택합니다. 각 설정에서 **텍스트 사용자 지정**을 선택합니다. 그러면 해당 특정 설정을 사용자 지정하는 창이 열립니다. 각 옵션에 대한 **사용자 지정** 창마다 다음 정보를 입력합니다(각 옵션에 대한 설정을 입력한 후 **저장** 선택). <br/>

    - **워터마크 추가** 
        - 워터마크 텍스트: **Sensitive - Do Not Share**(힌트: 이 값을 입력한 후 복사(Ctrl+C)하여 다른 두 텍스트 설정에 붙여넣기(Ctrl+V))
        - 글꼴 크기: **25**
        - 글꼴 색: **Blue**
        - 텍스트 레이아웃: **Diagonal**
            
    - **헤더 추가** 
        - 헤더 텍스트: **Sensitive - Do Not Share**
        - 글꼴 크기: **25**
        - 글꼴 색: **Red**
        - 텍스트 맞춤: **Center**
            
    - **바닥글 추가**
        - 바닥글 텍스트: **Sensitive - Do Not Share**
        - 글꼴 크기: **25**
        - 글꼴 색: **Red**
        - 텍스트 맞춤: **Center**

10. **콘텐츠 표시** 페이지에서 **다음**을 선택합니다. 

11. **파일 및 이메일에 자동 레이블 지정** 페이지에서 **파일 및 이메일에 자동 레이블 지정** 토글을 **켜기**로 전환합니다. 이렇게 하면 다음 단계에서 업데이트할 일련의 옵션을 사용할 수 있습니다.

12. **해당 조건과 일치하는 콘텐츠 검색**에서 **+조건 추가**를 선택하고 **다음이 포함된 콘텐츠**를 선택합니다.

13. **다음이 포함된 콘텐츠** 창에서 **추가** 드롭다운 화살표를 선택하고 **중요한 정보 유형**을 선택합니다.

14. **중요한 정보 유형** 창에서 **이름** 열 머리글의 왼쪽에 마우스를 놓습니다. 표시되는 확인란을 선택하면 모든 중요한 정보 유형이 자동으로 선택됩니다. **추가**를 선택하면 레이블에 이 중요한 정보 유형이 모두 추가됩니다.

15. **파일 및 이메일에 자동 레이블 지정** 페이지에 선택한 중요한 정보 유형이 모두 표시됩니다. 창의 아래쪽으로 스크롤하여 다음 설정을 업데이트합니다.

    - 콘텐츠가 이 조건과 일치하는 경우: **레이블 자동 적용** 선택 

    - 레이블이 적용될 때 사용자에게 이 메시지 표시: **중요한 콘텐츠가 검색되어 암호화됩니다**.
        
16. **다음**을 선택합니다.

17. **그룹 및 사이트의 보호 설정 정의** 페이지에서 확인란 두 개 모두 선택하지 않은 상태로 두고 **다음**을 선택합니다.

18. **스키마화된 데이터 자산에 자동 레이블 지정(미리 보기)** 페이지에서는 스키마화된 데이터 자산에 자동 레이블 지정(미리 보기)을 사용하도록 설정하지 않습니다. **다음**을 선택합니다. 

19. **설정 검토 및 완료** 페이지에서 입력한 정보를 검토합니다. 설정을 수정해야 하는 경우 해당 **편집** 옵션을 선택하고 필요한 내용을 변경합니다. 모든 정보가 올바르게 표시되면 **레이블 만들기**를 선택합니다.

20. 만들려는 레이블에 대해 생성된 규칙 Blob이 너무 길다는 **클라이언트 오류** 대화 상자가 표시됩니다. 규칙당 한 번에 선택할 수 있는 중요한 정보 유형의 최대 크기는 **49152**입니다. 몇 단계 전 **중요한 정보 유형 창**에서 수행한 것처럼 모든 중요한 정보 유형을 선택하면 이 제한을 초과하게 됩니다. <br/>

    **참고: 학습자가 이 오류를 확인할 수 있도록 일부러 중요한 정보 유형을 모두 선택하도록 한 것입니다.** 사용자의 프로덕션 환경에서 이 오류가 발생하면 오류가 나타난 이유와 이를 수정하는 방법을 알 수 있도록 사용자에게 이 오류를 미리 경험시키고자 했습니다.  <br/>

    이 문제를 해결하려면 **클라이언트 오류** 대화 상자에서 **확인**을 선택한 다음 **설정 검토 및 완료** 페이지에서 아래로 스크롤하여 **파일 및 이메일에 자동 레이블 지정** 섹션이 나오면 **편집**을 선택합니다.
    
21. 그러면 마법사의 **레이블이 지정된 항목에 대한 보호 설정 선택** 페이지로 돌아갑니다. 이 페이지에서 **다음**을 선택하고 **암호화** 페이지에서 **다음**을 선택한 뒤 **콘텐츠 표시** 페이지에서 **다음**을 선택합니다. 그러면 **파일 및 이메일에 자동 레이블 지정** 페이지로 이동합니다 . 

22. **파일 및 이메일에 자동 레이블 지정** 페이지의 **다음이 포함된 콘텐츠** 조건 오른쪽에 있는 **휴지통 아이콘**을 선택합니다. 이렇게 하면 만든 **PII** 레이블에서 **다음이 포함된 콘텐츠** 조건이 제거됩니다. <br/>

    남은 단계에서는 앞에서 설정한 모든 민감도 정보 유형이 아닌 두 가지 민감도 정보 유형만 포함하는 새 조건을 추가해 보겠습니다.

23. **파일 및 이메일에 자동 레이블 지정** 페이지의 **해당 조건과 일치하는 콘텐츠 검색**에서 **+조건 추가**를 선택한 다음 **다음이 포함된 콘텐츠**를 선택합니다.

24. **다음이 포함된 콘텐츠** 창에서 **추가** 드롭다운 화살표를 선택하고 **중요한 정보 유형**을 선택합니다.

25. **중요한 정보 유형** 창의 중요한 정보 유형 목록에서 **ABA 라우팅 번호**와 **미국 SSN(사회 보장 번호)** 확인란만 선택한 다음 **추가**를 선택합니다. **파일 및 이메일에 자동 레이블 지정** 페이지로 돌아가면 이 두 가지 중요한 정보 유형이 모두 표시됩니다. **다음**을 선택합니다.

26. **그룹 및 사이트의 보호 설정 정의** 페이지에서 두 확인란을 선택하지 않은 상태로 두고 **다음**을 선택합니다.

27. **스키마화된 데이터 자산에 자동 레이블 지정(미리 보기)** 페이지에서는 데이터베이스 열에 자동 레이블 지정을 사용하도록 설정하지 않습니다. **다음**을 선택합니다.

28. **설정 검토 후 완료** 페이지에서 **레이블 만들기**를 선택합니다.

29. **민감도 레이블이 생성됨** 페이지에서 **아직 정책을 만들지 않기** 옵션을 선택한 다음 **완료**를 선택합니다. 그러면 **레이블** 페이지로 돌아갑니다.

30. 이제 **PII** 레이블을 게시할 차례입니다. **레이블** 페이지에서 **PII** 레이블이 레이블 목록에 표시되지 않으면 메뉴 모음에서 **새로 고침**을 선택합니다. **PII** 레이블이 나타나면 왼쪽에 표시되는 확인란을 선택합니다. 

31. 레이블 목록 위의 메뉴 모음에 표시되는 **레이블 게시** 옵션을 선택합니다. 그러면 **정책 만들기** 마법사가 시작됩니다.

32. **정책 만들기** 마법사에서 **게시할 민감도 레이블 선택** 페이지의 목록에 이미 **PII** 레이블이 있으므로 **다음**을 선택합니다. 

33. 이 PII 정책을 선택한 관리 단위 그룹이 아닌 Adatum의 전체 디렉터리에 할당할 예정이므로 **관리 단위 할당** 페이지에서는 **다음**을 선택합니다.

34. **사용자 및 그룹에 게시** 페이지에서 모든 사용자 및 그룹에서 정책을 사용할 수 있도록 할지, 선택한 사용자 및 그룹으로 정책을 제한할지 여부를 선택할 수 있습니다. 이 랩의 경우 모든 사용자가 정책을 사용할 수 있도록 만들려고 합니다. **사용자 및 그룹** 옵션은 기본적으로 이미 선택되어 있습니다(그렇지 않은 경우 지금 선택). 이대로 진행하면 모든 사용자 및 그룹에서 정책을 사용할 수 있습니다. **다음**을 선택합니다.  <br/>

    **참고:** 실제 배포에서 이 작업을 수행할 때 정책을 선택한 수의 사용자 또는 그룹으로 제한하려면 **편집**을 선택하고 원하는 대상을 선택합니다. 

35. **정책 설정** 페이지에서 **레이블을 제거하거나 분류 레이블을 낮추는 근거를 제공하도록 사용자에게 요구** 확인란을 선택하고 **다음**을 선택합니다. 

36. **문서에 기본 레이블 적용** 페이지에 표시되는 드롭다운 메뉴에서 **PII**를 선택한 뒤 **다음**을 선택합니다.

37. **이메일에 기본 레이블 적용** 페이지에 표시되는 드롭다운 메뉴에서 **PII**를 선택한 뒤 **다음**을 선택합니다.

38. **모임 및 일정 이벤트 페이지에 기본 레이블 적용** 페이지에 표시되는 드롭다운 메뉴에서 **PII**를 선택한 뒤 **다음**을 선택합니다.

39. **Fabric 및 Power BI 콘텐츠에 기본 레이블 적용** 페이지에 표시되는 드롭다운 메뉴에서 **PII**를 선택한 뒤 **다음**을 선택합니다.

40. **정책 이름 지정** 페이지에서 **이름** 필드에 **PII Policy**라고 입력한 뒤 이 민감도 레이블 정책에 대한 다음 설명을 입력(하거나 복사 후 붙여넣기)합니다. **The purpose of this policy is to detect sensitive information such as ABA bank routing numbers and US social security numbers in emails and documents, and to encrypt this information when it's discovered. The user must provide an explanation for removing the classification label.** **다음**을 선택합니다.

41. **검토 및 완료** 페이지에서 입력한 정보를 검토합니다. 수정해야 하는 사항이 있으면 해당 **편집** 옵션을 선택하고 필요한 수정을 합니다. 모든 정보가 올바르게 표시되면 **제출**을 선택합니다.

42. **새 정책이 생성됨** 페이지에서 **완료**를 선택합니다.

43. Edge 브라우저를 모든 탭과 함께 열어 둡니다. 


### 작업 4 - 문서에 기존 민감도 레이블 할당

이 랩의 시작 부분에서 설명한 바와 같이, 이전 작업에서 만든 민감도 레이블 및 레이블 정책을 즉시 테스트할 수는 없습니다. 새 레이블 정책이 Microsoft 365 전체에 전파되어 해당 레이블이 Microsoft Word 및 Outlook과 같은 응용 프로그램에 표시될 때까지 최대 24시간이 걸리기 때문입니다.

대신 Microsoft 365의 기존 민감도 레이블 중 하나를 테스트해 보겠습니다. 이 랩에서는 극비 레이블인 **Project - Falcon** 민감도 레이블을 사용합니다. 이 레이블은 이전 작업에서 만든 레이블과 유사합니다. 한 가지 예외는 헤더나 바닥글이 포함되지 않는다는 것입니다. 이 기존 레이블을 사용해 보면 앞에서 만든 레이블이 Adatum에서 어떻게 작동할지 이해하는 데 도움이 됩니다.

1. LON-CL1의 Edge 브라우저에서 여전히 Microsoft 365에 **Holly Dickson**으로 로그인한 상태여야 합니다.

2. 먼저 이 작업에서 문서에 적용할 **Project-Falcon** 민감도 레이블을 검토합니다.  Edge 브라우저에는 이전 작업에서 연 **Microsoft Purview** 포털 탭이 계속 열려 있어야 합니다. **Microsoft Purview** 포털의 탐색 창에 있는 **Information protection**에서 **레이블**을 선택합니다. 

3. **레이블** 페이지의 레이블 목록에서 **극비** 옆에 있는 오른쪽 화살표(**>**)를 선택하여 이 레이블의 하위 레이블을 표시합니다. 이렇게 하면 기존의 **Project - Falcon** 레이블이 표시됩니다.

4. **Project - Falcon** 레이블을 선택합니다(확인란이 아닌 레이블 이름 선택). 이렇게 하면 **Project - Falcon** 세부 정보 창이 열립니다. 이 레이블에 대해 정의된 정보를 검토한 다음 완료되면 창을 닫습니다.  

5. 이제 **Project-Falcon** 민감도 레이블을 문서에 할당해 보겠습니다. 브라우저의 **홈 | Microsoft 365** 탭을 선택하여 Microsoft 365 홈페이지로 돌아갑니다. 화면 왼쪽의 **앱** 아이콘을 선택합니다. 그러면 나타나는 **앱** 페이지에서 **Word** 타일을 마우스 오른쪽 버튼으로 클릭하고 **새 탭에서 열기**를 선택합니다. 

6. **Word | Microsoft 365** 탭의 페이지 맨 위에 있는 **새로 만들기** 섹션에서 **빈 문서**를 선택합니다.

7. **개인 정보 옵션** 창이 나타나는 경우 **닫기**를 선택합니다.

8. Word 리본에 각 기능에 대한 아이콘이 표시되지만 아이콘이 그룹별로 나뉘지는 않는 경우 리본의 맨 오른쪽에 있는 아래쪽 화살표를 선택한 다음 **리본 레이아웃**에서 **클래식 리본**을 선택합니다. 그러면 리본이 기능 그룹(예: 실행 취소, 클립보드, 글꼴, 단락, 스타일 등) 단위로 구분된 기존 리본 스타일로 전환됩니다.

9. **Word** 문서에 다음 텍스트를 입력합니다. **Testing a sensitivity label on a document with personally identifiable information (PII); in this case, a U.S Social Security Number: 111-11-1111.**

10. 이 연습을 시작할 때 민감도 레이블을 사용하도록 설정했기 때문에 **Word** 페이지 맨 위에 있는 리본에 **민감도** 그룹이 표시됩니다. **민감도** 그룹에서 아래쪽 화살표를 선택합니다. 나타나는 드롭다운 메뉴에 민감도 레이블 유형 목록이 표시되어야 합니다. **극비**를 선택한 뒤 나타난 하위 메뉴에서 **Project - Falcon**을 선택합니다. <br/>

    **참고:** 24시간이 지나면 이전 작업에서 만든 레이블이 Project-Falcon 레이블 옆에 있는 극비 하위 메뉴에 표시됩니다. 그러나 지금은 **Project - Falcon** 레이블을 대신 사용합니다.

11. 문서에서 레이블이 문서 위쪽을 가로지르는 **CONFIDENTIAL - ProjectFalcon** 워터마크를 적용한 모습을 확인합니다. Project - Falcon 레이블은 사용자가 만든, 워터마크가 페이지 중간에 대각선으로 표시되도록 하는 레이블과 동일한 방식으로 구성되었습니다. 그렇다면 왜 페이지 맨 위에 표시되는 것일까요? 답은 **웹용 Word**를 사용하고 있기 때문입니다. 웹 버전에서는 기본적으로 여기에 보이는 것과 같이 표시됩니다. 문서를 읽는 사람에게 어떻게 표시되는지 확인하려면 **읽기용 보기**에서 문서를 확인해야 하며, 지금 바로 시작해 보겠습니다. <br/>

    **보기** 탭을 선택한 다음 Word 리본에서 **읽기용 보기**를 선택합니다. 워터마크가 문서 중간에 대각선으로 표시되는 것을 확인합니다. 이는 문서를 읽는 사람에게 워터마크가 표시되는 방식입니다. Word 데스크톱 앱을 사용하는 경우 레이블에 지정된 대로 워터마크가 표시되며, 이 경우 여기 읽기용 보기에 표시되는 것과 동일하게 표시됩니다. <br/>

    읽기용 보기를 종료하려면 페이지 상단의 메뉴 모음에서 **문서 편집**을 선택합니다. 표시되는 드롭다운 메뉴에서 **편집**을 선택합니다.

12. 이 첫 번째 유효성 검사 테스트에서는 이 민감도 레이블이 이 문서에 적용되지 않도록 제거합니다. 레이블 정책 옵션 중 하나는 사용자가 레이블을 제거하거나 더 낮은 분류 레이블을 선택하기 위해 근거를 제공해야 한다는 것입니다. 이제 이 설정이 제대로 작동하는지 확인합니다. <br/>

    Word 리본의 **민감도** 그룹에서 아래쪽 화살표를 선택합니다. 표시되는 드롭다운 메뉴에서 **극비** 옆에 확인 표시가 나타나는지 확인합니다. **극비** 위에 마우스를 올려놓으면 하위 메뉴가 표시됩니다. **프로젝트 - Falcon** 옆에 확인 표시가 나타나는지 확인합니다. 확인 표시는 문서에 적용되는 현재 레이블을 식별합니다.  <br/>
 
    이 문서에서 레이블을 제거하려면 이 드롭다운 메뉴에 나타나는 **프로젝트 - Falcon** 레이블을 선택합니다.
    
13. 표시되는 **근거 필요** 창에서 **기타(설명)** 옵션을 선택합니다. **이 레이블을 변경하는 이유 설명** 필드에 **Testing what happens when a label is removed from a document**를 입력한 다음 **변경**을 선택합니다.

14. 문서의 워터마크가 사라졌는지 확인합니다. Word 리본의 **민감도** 그룹에서 아래쪽 화살표를 선택합니다. 표시되는 드롭다운 메뉴에서 **극비** > **프로젝트 - Falcon**이 표시되어 있지만 그 옆에는 확인 표시가 나타나지 않는다는 점에 유의합니다. 이는 민감도 레이블이 이 문서에 더 이상 적용되지 않음을 나타냅니다.  

15. 문서에 민감도 레이블을 다시 적용하려면 드롭다운 메뉴에서 **극비** > **프로젝트 - Falcon**을 선택합니다. 워터마크가 문서에 어떻게 다시 나타나는지 확인합니다.

16. 이제 다음 작업에서 공유할 수 있도록 문서를 저장합니다. 드롭다운 화살표가 있는 문서 이름 필드가 페이지 왼쪽 상단, Word 아이콘 오른쪽에 나타납니다(Word에서 임시 파일 이름으로 **Document** 또는 **Document1**이 표시될 수 있습니다). 드롭다운 화살표를 선택합니다. 표시되는 드롭다운 메뉴에서 파일 **위치**가 **Holly Dickson > 문서**로 되어 있는지 확인합니다. <br/>

    **파일 이름** 필드에서 파일 이름을 **ProtectedDocument1**로 변경한 다음 이 파일 이름 메뉴 외부를 선택합니다(문서 내부 선택). 파일에 할당된 새 이름이 제목 표시줄에 표시됩니다. 

17. 문서를 표시하는 **ProtectedDocument1** 탭을 열어둔 채로 둡니다. 다음 작업에서 이 문서로 돌아와서 Joni Sherman과 문서를 공유하게 됩니다.

**프로젝트 - Falcon**이라는 제목의 극비 레이블이 포함된 Word 문서를 성공적으로 만들었습니다. 


### 작업 5 – Microsoft Purview Information Protection을 사용하여 문서 보호

이전 작업에서 Word 문서를 만들고 **프로젝트 - Falcon** 민감도 레이블로 보호했습니다. 이 레이블은 문서에 워터마크를 삽입했습니다. 이 작업에서는 만든 문서를 Joni Sherman과 공유하고 Joni를 "보기 전용" 권한으로 제한합니다. 이렇게 하면 구성한 매개 변수를 기반으로 Microsoft Purview Information Protection가 문서를 보호하는 방법을 확인할 수 있습니다.

문서에 할당한 보호 기능이 작동하는지 확인하기 위해 먼저 문서를 두 사람, 즉 Joni Sherman과 본인의 개인 이메일 주소로 이메일을 보냅니다. 그런 다음 Joni가 문서를 볼 수만 있고 편집할 수 없는지 확인하고 문서가 공유되지 않은 상태에서 문서에 액세스할 수 없는지 확인합니다. 마지막으로 Joni가 문서를 편집할 수 있도록 문서에 대한 권한을 변경하고 테스트를 위해 이 업데이트된 문서를 Joni에게 이메일로 보냅니다. 읽기 전용 액세스 권한을 제공하는 문서 링크와 문서 편집 기능을 제공하는 문서 링크가 포함된 이메일 두 개를 Joni에게 보낸 목적은 Microsoft Entra ID Protection이 어떻게 다양한 수준의 문서 보호를 제공할 수 있는지 보여주기 위해서입니다. 

1. LON-CL1의 경우, Edge 브라우저에서 **Word** 탭이 열려 있는 상태에서 이전 작업의 **Holly Dickson**으로 Microsoft 365에 로그인되어 있어야 합니다.

2. Edge 브라우저에서 **앱 | Microsoft 365** 탭을 선택합니다. 

3. **앱** 페이지에서 **Outlook** 타일을 마우스 오른쪽 단추로 클릭하고 **새 탭에서 열기**를 선택합니다. 그러면 새 브라우저 탭에서 웹용 Outlook에 Holly의 사서함이 열립니다. 

4. **웹용 Outlook**에서 화면 상단의 **새 메일**을 선택합니다.

5. 이메일 양식에 다음 정보를 입력합니다.

    - 받는 사람: **Joni**를 입력한 다음 사용자 목록에서 **Joni Sherman**를 선택합니다. 

    - 참조: 본인의 개인 이메일 주소를 입력(Holly의 이메일 주소가 '아닌' 본인의 개인 이메일 주소 입력)한 다음 표시되는 메시지 **이 주소 사용: <your email address>** 을(를) 선택합니다.

    - 제목 추가: **보호된 문서 테스트 - 보기 전용 권한**

    - 메시지 본문: **이 이메일에 첨부된 보호된 문서를 열어 변경 시도**를 입력합니다.

6. 메시지 본문의 이전 단계에서 추가한 텍스트 아래에 이전 작업에서 만든 문서의 링크를 첨부합니다. 하지만 그렇게 하려면 먼저 Joni Sherman과 문서를 공유해야 하며, 공유할 때 제한된 **보기 전용** 권한이 적용됩니다. 이렇게 하려면 이 이메일을 그대로 두고 문서로 돌아가서 Joni와 공유해야 합니다. 공유 과정에서 생성된 링크를 복사한 후 이 이메일로 돌아와서 링크를 붙여넣습니다. <br/>

    Edge 브라우저에서 **ProtectedDocument1** 탭을 선택하면 이전 작업에서 만든 문서가 여전히 표시되어야 합니다. 페이지 오른쪽 상단의 Holly Dickson의 이름과 이니셜 아래에서 **공유** 버튼을 선택합니다. 표시되는 드롭다운 메뉴에서 **공유**를 선택합니다.

7. 표시되는 **"ProtectedDocument1" 공유** 창에서 **링크 복사** 버튼 옆에 표시되는 기어(**링크 설정**) 아이콘을 선택합니다. 

8. 표시되는 **링크 설정** 창에서 **선택한 사람** 옵션을 선택합니다.
    
9. **기타 설정**에서 현재 옵션은 **수정 가능**입니다. 이 문서를 Joni Sherman과 공유하려고 하지만 Joni에게 문서 보기 권한까지만 설정하려고 합니다. 이 권한을 변경하려면 **수정 가능**을 선택합니다. 표시되는 메뉴에서 사용 가능한 옵션을 검토합니다. **수정 가능** 옆에 확인 표시가 있는 것을 볼 수 있으며, 이는 현재 설정임을 보여줍니다. Joni를 읽기 전용 권한으로 제한하려면 **보기 가능**을 선택한 다음 **적용**을 선택합니다.

10. 그러면 **"ProtectedDocument1" 공유** 창으로 돌아가게 됩니다. **이름, 그룹 또는 이메일 추가** 필드에 **Joni**를 입력합니다. 이름이 **Joni**로 시작하는 사용자 목록이 표시되어야 합니다. **Joni Sherman**을 선택합니다.

11. **"ProtectedDocument1" 공유** 창에서 Joni의 이름 오른쪽에 표시되는 '눈' 아이콘 위로 마우스를 가져갑니다. 이렇게 하면 이 문서에 대해 할당한 현재 설정인 **보기 가능**이 표시되어야 합니다. "눈" 아이콘은 "보기 가능"을 니다. **링크 복사** 버튼을 선택합니다. 

12. **"ProtectedDocument1" 공유** 창 하단에 **링크 복사됨** 메시지가 나타나면 창 위쪽 모서리에 있는 X를 선택하여 닫습니다.

13. Edge 브라우저에서 **메일 - Holly Dickson - Outlook** 탭을 선택하여 이메일 메시지로 돌아갑니다. 메시지 본문에서 앞서 추가한 텍스트 아래에 방금 클립보드에 복사한 공유 문서의 링크를 붙여넣기(Ctrl+V)합니다. **ProtectedDocument1.docx**라는 파일 링크가 나타납니다. 

14. **보내기**를 선택합니다.

15. **수신자가 링크에 액세스할 수 없음**이라는 메시지가 표시되어야 합니다. 이 메시지는 문서에 액세스할 수 있는 권한이 없는 개인 이메일 주소를 이메일에 포함했다는 사실을 Microsoft Entra ID Protection에서 인식한 결과입니다. 이 랩 테스트의 목적에 따라 **무시하고 보내기**를 선택합니다.

16. **LON-CL2**로 전환합니다. 

17. **LON-CL2**에서 이전 랩 연습의 **Lynne Robbins**로 **웹용 Outlook**에 로그인해야 합니다. Lynne으로 로그아웃합니다.

18. Edge 브라우저에서 **로그아웃** 탭을 제외한 모든 탭을 닫습니다. 이 탭의 주소 표시줄에 다음 URL(**https://outlook.office365.com**)을 입력합니다. 

19. **계정 선택** 창에서 **다른 계정 사용**을 선택합니다.

20. **로그인** 창에서 **JoniS@xxxxxZZZZZZ.onmicrosoft**(여기서 xxxxxZZZZZZ는 랩 호스팅 공급자가 제공한 테넌트 접두사)을(를) 입력한 후 **다음**을 선택합니다.

21. **암호 입력** 창에서 이전에 Joni의 계정에 할당했던 새 사용자 암호를 입력한 다음 **로그인**을 선택합니다. 

22. **시작** 창이 나타나면 X를 선택하여 창을 닫습니다.

23. **웹용 Outlook**에서 Joni의 **받은 편지함**에는 Holly가 방금 보낸 제목 줄에 문서에 보기 전용 권한이 있음을 나타내는 이메일이 표시됩니다. 이 이메일을 엽니다.

24. 이메일에서 첨부 파일을 선택하여 엽니다.

25. 표시되는 **개인정보 옵션** 창에서 **닫기**를 선택합니다. 문서는 새 브라우저 탭에서 **웹용 Word**로 열리며, 해당 탭의 제목은 **ProtectedDocument1.docx**입니다. 문서가 **웹용 Word**의 읽기용 보기로 표시되는 것을 확인합니다. 이는 Joni가 보기 전용 권한을 가지고 있으며 문서를 편집할 수 없다는 것을 나타냅니다. 이를 확인하려면 문서를 선택합니다. 표시되는 메시지(**읽기 전용. 이 문서는 읽기 전용입니다.**)에 유의합니다. **프로젝트 - Falcon** 정책에 명시된 워터마크에 유의합니다. <br/>

    문서 검토가 끝나면 **ProtectedDocument1.docx** 탭을 닫습니다. 

26. 이제 개인 이메일 주소로 전송된 문서를 열려고 할 때 어떤 일이 발생하는지 테스트합니다. 휴대폰 또는 교실 PC를 사용하여 개인 사서함에 액세스합니다. Holly가 방금 개인 이메일 주소로 보낸 이메일을 연 다음 첨부 파일을 열어봅니다. 

27. 문서에 액세스할 수 있는 권한이 없으므로 **계정 선택** 창이 니다. 실제 시나리오에서는 파일에 액세스할 수 있는 권한이 있는 계정으로 로그인하거나 **Holly@xxxxxZZZZZZ.onmicrosoft.com** 계정에서 액세스를 요청할 수 있습니다. <br/>

    이 테스트의 목적을 위해 공유되지 않았기 때문에 파일에 액세스할 수 없음을 방금 확인했습니다. 또한 Joni가 파일을 볼 수만 있고 편집할 수 없다는 것도 확인했습니다. 이제 Joni가 파일을 편집할 수 있도록 허용하여 파일에 대한 공유 권한을 변경합니다. 이렇게 하면 이 환경이 방금 완료한 환경과 어떻게 다른지 확인할 수 있습니다. 

28. **LON-CL1**으로 전환합니다. 

29. LON-CL1에서는 Edge 브라우저에서 여전히 **Holly Dickson**으로 Microsoft 365에 로그인되어 있어야 하며 **Word**와 **Outlook** 탭이 모두 열려 있어야 합니다. **메일 - Holly Dickson - Outlook** 탭을 선택합니다. 

30. Holly의 사서함에서 Joni Sherman에게 다른 이메일을 작성합니다. 참조 에 개인 이메일 주소를 포함하지 않습니다. 이메일 양식에 다음 정보를 입력합니다.

    - 받는 사람: **Joni**를 입력한 다음 사용자 목록에서 **Joni Sherman**를 선택합니다. 

    - 참조 비워두기

    - 제목 추가: **보호된 문서 테스트 - 편집 권한**

    - 메시지 본문: **이 이메일에 첨부된 보호된 문서를 열어 변경 시도**를 입력합니다.

31. 이전 이메일과 마찬가지로 이제 문서를 Joni와 공유해야 하지만 이번에는 편집 권한이 있어야 합니다. 이렇게 하려면 다음 단계를 수행하십시오. <br/>

    - 브라우저에서 **ProtectedDocument1** 탭을 선택한 다음 메뉴 모음 오른쪽에서 **공유** 버튼을 선택합니다. 표시되는 드롭다운 메뉴에서 **공유**를 선택합니다. 
    - **"ProtectedDocument1" 공유** 창에서 **이름, 그룹 또는 이메일 추가** 필드에 **Joni**를 입력한 다음 **Joni Sherman**을 선택합니다.
    - Joni의 이름 오른쪽에는 연필(**편집 가능**) 아이콘이 있습니다. 이는 문서를 공유할 때의 기본 권한입니다. **링크 복사** 버튼을 선택하면 어떤 일이 발생하는지 확인할 수 있습니다.
    - 표시되는 **링크 복사됨** 메시지에 유의합니다. 이 메시지는 Joni의 이름을 지정했음에도 불구하고 누구나 문서를 편집할 수 있음을 나타냅니다. 편집할 수 있는 사람을 Joni로만 제한하는 것을 의도한 것은 아닐 것입니다. 이 제한을 적용하려면 **링크 복사** 버튼 옆의 기어(**링크 설정**) 아이콘을 선택합니다. 
    - 표시되는 **링크 설정** 창에서 **선택한 사람** 옵션을 선택합니다. 이 옵션은 선택한 사용자에 대한 권한을 제한하는 핵심 옵션입니다. 
    - **더 많은 설정**에서 **편집 가능**이 표시되면 **적용**을 선택합니다. 그러나 **보기 가능**이 표시되면 **보기 가능**을 선택한 다음, 표시되는 메뉴에서 **편집 가능**을 선택하고 **적용**을 선택합니다. 
    - **"ProtectedDocument1" 공유** 창에서 **링크 복사** 버튼을 선택합니다.
    - 표시되는 **링크 복사됨** 메시지에 유의합니다. 이번에는 지정한 사람만 문서를 편집할 수 있다는 메시지가 표시됩니다. 이 경우 편집은 사용자가 지정한 유일한 사람이므로 Joni로만 제한됩니다. 
    - 브라우저에서 **메일 - Holly Dickson - Outlook** 탭을 선택한 다음 링크를 이메일 메시지 본문에 붙여넣습니다. 

32. **보내기**를 선택합니다.

33. **LON-CL2**로 전환합니다. 

34. **LON-CL2**에서는 여전히 **웹용 Outlook**에 **Joni Sherman**으로 로그인해야 합니다. Joni의 **받은 편지함**에 Holly가 방금 보낸 이메일의 제목 줄에 문서 편집 권한이 있음을 나타내는 이메일이 표시됩니다. 이 이메일을 엽니다.

35. 이메일에서 첨부 파일을 선택하여 엽니다.

36. Joni에게 보기 전용 권한이 있는 경우 문서가 읽기 보기 창에서 열립니다. 따라서 Joni는 문서를 편집할 수 없습니다. 이 버전의 문서는 Joni에게 편집 권한을 제공하므로 이번에는 일반 편집 모드로 Word에서 문서가 열립니다. 문서에 텍스트를 입력할 수 있는지 확인합니다. 

    **참고:** 이 작업에서는 구성한 PII 정책 매개 변수를 기반으로 Microsoft Purview Information Protection이 문서를 보호하는 방법을 방금 확인했습니다. Joni에게 보기 전용 권한이 할당되었을 때 문서가 읽기용 보기로 열렸고 문서를 변경할 수 없었습니다. Joni에게 편집 권한이 할당되자 Word에서 문서가 열렸고 문서를 변경할 수 있었습니다. 그리고 Holly가 문서를 공유하지 않았기 때문에 개인 사서함으로 문서를 이메일로 보냈을 때 해당 문서를 열 수 없었습니다. 

## 랩 4 종료


# 축하합니다! 이 과정에서 마지막 랩을 완료했습니다.