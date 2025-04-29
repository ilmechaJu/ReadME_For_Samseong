# JIRA → GitHub 마이그레이션 도구 README
이 도구는 JIRA 이슈, 마일스톤, 라벨, 프로젝트 보드를 GitHub(GHEC, GHES)로 자동 마이그레이션하는 스크립트입니다. 대량의 이슈 이전이 필요한 경우, JIRA의 프로젝트 구조와 상태를 최대한 보존하며 GitHub로 옮길 수 있습니다.




## **주요기능**
1. JIRA의 모든 이슈 데이터를 GitHub로 이전할 수 있습니다.
2. 이슈 간의 관계(예: duplicates, blocks 등)를 댓글로 변환하여 유지.
3. 마일스톤과 라벨도 함께 이전됩니다.
4. (기능추가) JIRA -> GHES(Enterprise Server)지원.
5. (기능추가) JIRA -> GHES 이전 시, 이슈, 마일스톤, 라벨뿐만 아니라 '프로젝트' 도 함께 이전이 가능해졌습니다.
  - GHES 프로젝트를 자동으로 만들어주고, JIRA의 상태(Todo, In Progress, Holding, Done)를 GHES의 프로젝트 보드 상태로 자동맵핑.
6. JIRA -> GHES(User), GHES(Organization) 모두 Migration 가능.



### **[JIRA에 등록되어있는 상태(Todo, In Progress, Holding, Done)]**
![image](https://github.com/user-attachments/assets/8afa5a51-fb44-458c-a547-c1be7d69060a)


### **[GHES에 등록되어있는 상태(Todo, In Progress, Holding, Done)]**
![image](https://github.com/user-attachments/assets/7a1645de-e48d-4748-8e3b-c672dca25bf8)


**<필수 권한>**

1. GitHub Personal Access Token이 필요합니다
2. JIRA API 접근 권한이 필요합니다
3. GitHub Enterprise Server의 URL과 접근 정보가 필요합니다

이 도구는 JIRA에서 GitHub로의 대량 마이그레이션을 자동화하는데 유용합니다.


<br><br>

## GHES - 환경변수 설정

매번 번거롭게 run 할때마다 입력 해줘야하는 값들 때문에 고민이시라면, 환경변수로 셋팅 해주면 됩니다.

**<사용된 환경변수>**
```bash
export GITHUB_ENTERPRISE_DNS="https://<GHES 도메인>.com"
export GITHUB_ENTERPRISE_URL="https://<GHES 도메인과 맵핑된 실제 ip주소>.com"
export JIRA_MIGRATION_GITHUB_ACCESS_TOKEN="<GHES 액세스 토큰>"
export JIRA_MIGRATION_GITHUB_NAME="<github 유저 이름>"
export JIRA_MIGRATION_GITHUB_REPO="<repository 이름>"
export JIRA_MIGRATION_FILE_PATHS=<Jira에서 추출한 XML파일 이름.xml>
export JIRA_MIGRATION_JIRA_PROJECT_NAME=<Jira 프로젝트 키 이름>
```

*Jira 프로젝트 키 이름은 프로젝트 경로에서 확인할 수 있습니다.(아래 이미지 참조)

![image](https://github.com/user-attachments/assets/3f6150e6-473a-477a-83c8-a7b454390be4)


<br><br><br>

## 지원 버전 및 사용법
![image](https://github.com/user-attachments/assets/d2d7c44d-465d-4360-afd8-114b7a917b70)
- JIRA -> GHEC로 Migration을 원하시는 경우, **커밋 id = e646fb8** 을 clone 받아서 사용해주시고,
- JIRA -> GHES로 Migration을 원하시는 경우, **커밋 id = c002fa5** 를 clone 받아서 사용해주시고,
- **프로젝트 보드까지 포함한 기능** 사용을 원하시는 경우, **최신 커밋**을 사용해주세요.
  - 단순 Issues(제목, 설명, 라벨, 댓글), 마일스톤, 컴포넌트 이전 외에 추가로, 프로젝트 형태로 Migration을 원하시는 분은 **최신 커밋** 사용하시면 됩니다.


<br><br>

## 주의사항 - 작성자 이전 불가
- JIRA 사용자와 Github 사용자를 매핑하지 않습니다.

  - 가져오기를 수행하는 Github 사용자(사용하는 personal access token 기준)가 이슈 작성자로 표시되며, 원래 JIRA 이슈 작성자는 첫 번째 코멘트에 기록됩니다.
  - 가져오기를 수행하는 Github 사용자가 코멘트 작성자로도 표시되며, Github API가 이를 지원하지 않기 때문에 원래 JIRA 코멘트 작성자는 코멘트 본문에 기록됩니다.

<br><br>


## 사전준비 및 주의사항
- 본 스크립트는 사용자의 책임 하에 사용하며, 올바른 마이그레이션을 보장하지 않습니다.
- Github의 테스트 프로젝트로 먼저 마이그레이션을 시도해보는 것을 권장합니다.
- 입력 파일은 JIRA 프로젝트의 XML 내보내기 파일이어야 합니다(아래 참고).
- 2025년 4월 기준 JIRA Cloud에서 동작 확인됨.
- ~~대상 Github 프로젝트는 이미 존재해야 하며, 이슈 트래커가 활성화되어 있어야 합니다.~~ -> (기능추가)프로젝트가 자동으로 생성됩니다. 자세한 내용은 아래 **'프로젝트 셋팅법'**을 참고해주세요.
- 기존 이슈와 풀 리퀘스트가 없어야 합니다. 그렇지 않으면 이슈 ID 매핑이 잘못될 수 있습니다.


<br><br><br>

## 시작하기
**설치**
- 이 저장소를 클론합니다.
- pip install -r requirements.txt 명령어로 의존성 패키지를 설치합니다.
- 라벨 색상 지정 로직을 변경하려면 labelcolourselector.py를 수정합니다.
- Github Personal Access Token 생성 (토큰은 안전하게 보관하세요. 나중에 입력해야 합니다. 경고: 토큰은 비밀번호처럼 취급하고 외부에 노출하지 마세요.)

**도구 실행**
- 프로젝트의 원하는 JIRA 이슈를 내보냅니다(아래 참고).
- Github로 가져오기를 시작하려면 python main.py를 실행합니다.
- 시작 시 다음을 입력하라는 메시지가 표시됩니다.
  - JIRA XML 내보내기 파일명(여러 XML 경로는 세미콜론으로 구분)
  - JIRA 프로젝트명
  - 이슈를 완료로 간주하는 <statusCategoryId> 요소의 id 속성(정수)
  - 저장소 소유 Github 계정명(사용자 또는 조직)
  - 대상 Github 저장소명
  - 인증용 Github personal access token
  - 시작 인덱스(처음부터 시작하려면 0, 실패 지점부터 재시작하려면 해당 인덱스 번호 입력. 0보다 큰 값을 입력하면 라벨은 재가져오지 않고, 마일스톤은 기존과 재매칭됩니다.)

- 가져오기 과정은 다음과 같이 진행됩니다.
  - JIRA XML 내보내기 파일을 읽어 메모리 내 프로젝트 구조로 변환
  - Github Milestone API로 마일스톤 가져오기
  - Github Label API로 라벨 가져오기
  - Github Comment API로 모든 코멘트의 이슈 참조 플레이스홀더를 실제 Github 이슈 ID로 후처리
  - Github Import API로 이슈 및 코멘트 가져오기
    - 이 단계에서 코멘트 내 이슈 참조는 플레이스홀더로 대체됨
    - Import API는 일반 Github Issues API와 달리 abuse rate limit에 걸리지 않음

<br><br><br>


## JIRA 이슈 내보내기
1. 프로젝트의 이슈 검색 페이지로 이동(Issues → Search for Issues)
2. 원하는 프로젝트 선택
3. 쿼리 조건 및 정렬 지정, 1000개 초과 시 예시처럼 project = INFRA and issuekey <= INFRA-3000 AND issuekey > INFRA-2000 ORDER BY created DESC로 범위 지정 후 각 범위별로 개별 XML 파일로 내보내기
4. 결과 페이지 우측 상단의 내보내기 아이콘 클릭
5. XML 출력 선택 후 파일 저장

<br><br><br>


## 프로젝트 셋팅법

**1) 프로젝트 보드 상태 맵핑**

프로젝트는 GHES projects (classic) 에서 만들어지며, Jira <-> GHES 보드를 맵핑하기 위해서는 importer.py안에 **컬럼 설정하는 부분**을 수정해주어야 합니다.

    importer.py 에서 "# 프로젝트 보드 상태 매핑" 주석으로 되어있는 코드 아래쪽에 파라미터를 
본인의 Jira 컬럼과 맞게 변경합니다. - (JIRA의 기본값은 할일/진행중/완료/보류 이며, GHES의 기본값은 Todo/In Progress/Holding/Done 입니다. 이름이 각각 매칭이 되어야 합니다. *만약 매칭이 되지 않는다면 기본값인 Todo에 Issue들이 쌓이게 됩니다.)

예시:
```python
# 프로젝트 보드 상태 매핑
        self.status_mapping = {
            'To Do': 'Todo',
            'In Progress': 'In Progress',
            'Done': 'Done',
            'Holding': 'Holding',
            '할 일': 'Todo',
            '진행 중': 'In Progress',
            '완료': 'Done',
            '보류': 'Holding'
        }
```


**2) GHES 프로젝트 이름 지정**
```python
   def import_project_board(self):
        """
        JIRA의 보드를 GitHub 프로젝트 보드로 마이그레이션
        """
        print('Creating project board...')
        
        # 1. 프로젝트 생성 - 레포지토리 수준의 프로젝트
        project_url = f"{self.github_url}/projects"
        project_data = {
            'name': '이현중_GitHub 기술 스택 리스트 및 스킬업 커리큘럼',
            'body': 'Migrated from JIRA'
        }
```
GHES에서 생성될 프로젝트 이름 파라미터를 변경해줍니다.
'name' : '<원하는 프로젝트 이름>',


<br><br><br>

기술 설명
------ 
- 이 스크립트는 JIRA API를 통해 JIRA의 이슈들을 가져와서 GitHub Enterprise Server(GHES) API를 통해 GitHub로 마이그레이션하는 도구입니다.

작동 방식은 다음 단계와 같습니다.

### 1.JIRA 데이터 가져오기

- JIRA API를 사용하여 다음 정보들을 가져옵니다:
  - 이슈들 (제목, 설명, 라벨, 댓글 등)
  - 마일스톤
  - 컴포넌트
  - 이슈 간의 관계 (duplicates, blocks, relates to 등)

### 2. GitHub로 데이터 전송

- GitHub Enterprise Server API를 사용하여

  - 마일스톤 생성
  - 라벨 생성
  - 이슈 생성 (댓글 포함)
  - 이슈 간의 관계를 댓글로 변환

### 3. 매핑 정보 저장

- JIRA 이슈 ID와 GitHub 이슈 ID의 매핑 정보를 jira-keys-to-github-id.txt 파일에 저장


#### **주요 기능**

```python
# 마일스톤 가져오기
def import_milestones(self):
    # JIRA 마일스톤을 GitHub 마일스톤로 변환

# 라벨 가져오기
def import_labels(self, colour_selector):
    # JIRA 컴포넌트와 라벨을 GitHub 라벨로 변환

# 이슈 가져오기
def import_issues(self, start_from_count):
    # JIRA 이슈를 GitHub 이슈로 변환
    # 댓글도 함께 가져옴
    # 이슈 간의 관계도 댓글로 변환

```
