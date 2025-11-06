# **Git Branch 및 Merge 핵심 정리 (Git 입문자 가이드)**

본 문서는 동영상 강의를 바탕으로 Git의 핵심 기능인 Branch(브랜치)와 Merge(병합)에 대한 개념, 주요 명령어, 그리고 실습 시나리오를 정리한 문서입니다.

## **1\. Git Branch (브랜치)**

브랜치는 나뭇가지처럼 여러 갈래로 작업 공간을 나누어 독립적으로 작업할 수 있도록 도와주는 Git의 도구입니다.

### **1.1. Branch 사용의 장점**

1. **안정성 확보:** 독립된 개발 환경을 형성하여 원본(master 또는 main)에 영향을 주지 않고 안전하게 작업할 수 있습니다.  
2. **체계적인 협업:** 하나의 작업을 브랜치로 나누어 진행함으로써 체계적으로 협업 및 개발이 가능합니다.  
3. **손쉬운 이동:** 브랜치를 손쉽게 생성하고 브랜치 사이를 이동할 수 있습니다.  
4. **에러 복구:** 상용 서비스에 발생한 에러를 브랜치에서 분리하여 해결한 후, 문제가 없는 내용만 원본에 반영할 수 있습니다.

### **1.2. Branch 명령어 요약**

| 명령어 | 기능 |
| :---- | :---- |
| git branch | 브랜치 목록 확인 |
| git branch \-r | 원격 저장소의 브랜치 목록 확인 |
| git branch \<브랜치 이름\> | 새로운 브랜치 생성 |
| git branch \-d \<브랜치 이름\> | 브랜치 삭제 (병합된 브랜치만 삭제 가능) |
| git branch \-D \<브랜치 이름\> | 브랜치 강제 삭제 (병합 여부와 무관하게 삭제) |
| git switch \<다른 브랜치 이름\> | 현재 HEAD를 다른 브랜치로 전환 |
| git switch \-c \<새 브랜치 이름\> | 새 브랜치 생성 후 즉시 전환 |
| git switch \-c \<새 브랜치 이름\> \<commit ID\> | 특정 커밋에서 새 브랜치 생성 후 전환 |

### **1.3. Branch 실습 시나리오: 사전 준비**

Git 저장소를 초기화하고 몇 개의 커밋을 생성하는 준비 과정입니다.

| 순서 | 명령어 | 설명 |
| :---- | :---- | :---- |
| 1 | mkdir git-branch-practice | 실습용 폴더 생성 |
| 2 | cd git-branch-practice | 폴더 이동 |
| 3 | git init | Git 저장소 초기화 |
| 4 | \# article.txt에 master-1 작성 후 commit |  |
| 5 | touch article.txt | 빈 파일 생성 |
| 6 | echo "master-1" \> article.txt | 파일 내용 작성 |
| 7 | git add . | 변경 사항 스테이징 |
| 8 | git commit \-m "c1" | 첫 번째 커밋 |
| 9 | echo "master-2" \>\> article.txt | 파일 내용 추가 작성 |
| 10 | git commit \-am "c2" | 두 번째 커밋 (add 생략) |
| 11 | echo "master-3" \>\> article.txt | 파일 내용 추가 작성 |
| 12 | git commit \-am "c3" | 세 번째 커밋 |
| **확인** | git log \--oneline | 커밋 목록 확인 (master 브랜치에 c1, c2, c3 커밋이 존재) |

### **1.4. Branch 이동 (git switch)**

현재 HEAD가 master를 가리키고 있는 상태에서 새 브랜치 login을 생성하고 이동하는 시나리오입니다.

| 순서 | 명령어 | 설명 |
| :---- | :---- | :---- |
| 1 | git switch \-c login | login 브랜치 생성 후 즉시 전환 |
| **확인** | git log \--oneline | login 브랜치에서도 기존 커밋(c1, c2, c3)을 모두 볼 수 있음 |
| 2 | \# login 브랜치에서 article.txt 파일 수정 |  |
| 3 | echo "login-1" \>\> article.txt | login 브랜치에서 새 내용 추가 |
| 4 | git commit \-am "c4 (login branch)" | login 브랜치에서 커밋 생성 |
| **확인** | git log \--oneline \--graph \--all | master와 login 브랜치가 갈라진 것을 확인 |
| 5 | git switch master | 다시 master 브랜치로 전환 |
| **확인** | cat article.txt | master 브랜치에는 login-1 내용이 보이지 않음 (login 브랜치에서만 커밋했기 때문) |

## **2\. Git Merge (병합)**

Merge는 두 브랜치(Branch)의 변경 사항을 하나로 합치는 작업입니다.

### **2.1. Fast-Forward Merge (빨리 감기 병합)**

**정의:** 합치려는 브랜치(Hotfix)가 현재 브랜치(Master)의 커밋을 포함하고 있을 때, 단순히 현재 브랜치 포인터(HEAD)를 합치려는 브랜치의 최신 커밋으로 이동시키는 방식입니다. 이 방식은 별도의 **Merge 커밋을 생성하지 않습니다.**

| 순서 | 명령어 | 설명 |
| :---- | :---- | :---- |
| 1 | \# c1, c2 커밋 생성 후 master 브랜치 위치 가정 | (사전 준비 완료) |
| 2 | git branch hotfix | hotfix 브랜치 생성 |
| 3 | git switch hotfix | hotfix 브랜치로 전환 |
| 4 | \# hotfix 브랜치에서 c3 커밋 생성 |  |
| 5 | echo "hotfix-3" \>\> article.txt | 파일 수정 |
| 6 | git commit \-am "c3 (hotfix)" | hotfix 커밋 생성 |
| 7 | git switch master | master 브랜치로 전환 |
| 8 | git merge hotfix | hotfix 브랜치를 master로 병합 (Fast-Forward 발생) |
| **확인** | git log \--oneline | Merge 커밋 없이 master의 HEAD가 hotfix의 최신 커밋으로 이동 |
| 9 | git branch \-d hotfix | 병합 완료된 hotfix 브랜치 삭제 |

### **2.2. 3-Way Merge (3방향 병합)**

**정의:** 두 브랜치의 분기점(공통 조상 커밋)과 각 브랜치의 최신 커밋 2개를 사용하여 변경 내용을 병합하는 방식입니다. 이 방식은 **별도의 Merge 커밋을 생성합니다.**

| 순서 | 명령어 | 설명 |
| :---- | :---- | :---- |
| 1 | \# c1, c2 커밋 생성 후 master 브랜치 위치 가정 | (사전 준비 완료) |
| 2 | git switch \-c hotfix c2 | c2 커밋에서 hotfix 브랜치 생성 및 이동 |
| 3 | \# hotfix에서 c3 커밋 생성 | (hotfix 작업) |
| 4 | git switch master | master 브랜치로 이동 |
| 5 | \# master에서 c4, c5 커밋 생성 | (master 작업) |
| 6 | git merge hotfix | hotfix를 master로 병합 (3-Way Merge 발생 및 Merge 커밋 생성) |
| **확인** | git log \--oneline \--graph | 브랜치가 합쳐지고 Merge 커밋이 생성된 것을 확인 |
| 7 | git branch \-d hotfix | 병합 완료된 hotfix 브랜치 삭제 |

### **2.3. Merge Conflict (병합 충돌)**

**정의:** 두 브랜치에서 **동일한 파일의 동일한 부분**을 변경하여 병합 시 Git이 어떤 변경 사항을 선택해야 할지 자동으로 결정할 수 없을 때 발생합니다.

#### **2.3.1. 충돌 해결 과정**

1. **충돌 확인:** git merge \<브랜치 이름\> 명령 실행 후 CONFLICT 메시지 발생.  
2. **파일 수정:** 충돌이 발생한 파일(article.txt 등)을 열어 충돌 마커(\<\<\<\<\<\<\<, \=======, \>\>\>\>\>\>\>)를 보고 원하는 내용으로 직접 수정합니다.  
3. **스테이징:** 수정한 파일을 git add . 명령으로 스테이징 영역에 추가합니다.  
4. **커밋:** git commit 명령으로 Merge 커밋을 생성하여 충돌 해결을 완료합니다.

#### **2.3.2. 충돌 마커의 의미**

\<\<\<\<\<\<\< HEAD (Current Change)  
master 브랜치에서 작성한 부분  
\=======  
hotfix 브랜치에서 작성한 부분  
\>\>\>\>\>\>\> hotfix (Incoming Change)

* \<\<\<\<\<\<\< HEAD 부터 \======= 까지: 현재 브랜치(master)의 내용  
* \======= 부터 \>\>\>\>\>\>\> hotfix 까지: 병합하려는 브랜치(hotfix)의 내용

## **3\. Git Workflow (협업 전략)**

원격 저장소를 활용하여 다른 사용자와 협업하는 방법을 다룹니다.

### **3.1. Feature Branch Workflow**

중앙 저장소(origin)를 중심으로 모든 팀원이 각자 기능별 브랜치(feature/login, feature/signup 등)를 생성하여 작업하는 전략입니다.

1. **브랜치 생성:** master (또는 main)에서 새 기능 브랜치를 생성하고 작업합니다.  
2. **Pull Request (PR):** 작업이 완료되면 중앙 저장소에 브랜치 내용을 반영해 달라고 Pull Request를 요청합니다.  
3. **코드 리뷰 및 Merge:** 팀원의 코드 리뷰를 거쳐 master 브랜치에 병합(merge)합니다.  
4. **브랜치 삭제:** 병합이 완료된 기능 브랜치는 삭제합니다.

### **3.2. Git Flow**

배포를 관리하기 위한 브랜치 전략으로, master, develop, release, hotfix, feature 등 여러 개의 전용 브랜치를 사용합니다.

* **master:** 배포(production) 코드를 보관하는 브랜치.  
* **develop:** 개발 중인 최신 코드를 보관하는 브랜치.  
* **feature:** 특정 기능을 개발하는 브랜치.  
* **release:** 배포를 준비하는 브랜치.

### **3.3. Forking Workflow**

각 사용자가 소유권이 없는 원격 저장소(원본)를 복제(fork)하여 독립적인 저장소(복제본)를 만든 후, 복제본에서 작업하고 Pull Request를 통해 원본에 기여하는 방식입니다. 주로 오픈 소스 프로젝트에서 사용됩니다.

| 순서 | 명령어 | 설명 |
| :---- | :---- | :---- |
| 1 | **원본 저장소 Fork** | 원본 저장소(upstream)를 개인 계정의 복제본(origin)으로 복제합니다. |
| 2 | git clone \[복제본 URL\] | 복제본을 로컬 환경에 복제합니다. |
| 3 | git remote add upstream \[원본 URL\] | 원본 저장소를 upstream이라는 이름으로 원격 추가하여 동기화할 수 있게 합니다. |
| 4 | **기능 브랜치 생성 및 작업** | feature/login 등 기능 브랜치에서 독립적으로 작업합니다. |
| 5 | **Pull Request** | 작업이 완료되면 복제본 브랜치에서 원본 브랜치(upstream/master)로 Pull Request를 보냅니다. |
| 6 | git pull upstream master | 새로운 기능 추가 전 upstream/master의 내용을 Pull하여 동기화합니다. |

## **4\. 기타 명령어 정리**

| 명령어 | 설명 |
| :---- | :---- |
| git log \--oneline | 커밋 기록을 한 줄로 간결하게 표시 |
| git log \--oneline \--graph \--all | 모든 브랜치의 커밋 기록을 그래프 형태로 표시 |
| git status | 현재 Git 저장소의 상태 확인 (스테이징 여부, 추적되지 않는 파일 등) |
| git add . | 현재 디렉토리의 모든 변경 사항을 스테이징 영역에 추가 |
| git commit \-m "메시지" | 스테이징 영역의 내용을 커밋 |

