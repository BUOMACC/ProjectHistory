# Hunter's Arena : Revolution
- Extraction 장르 Dedicated 기반 멀티플레이 (최대 8인)
- Steam / Epic CBT 및 Early Access 출시
- 게임 소개 영상 : https://www.youtube.com/watch?v=DAcVITWIVzc


<a name="table-contents"></a>
## 주 작업 내역
> 1. [Skill](#skill)
> 3. [Character Stat](#stat)
> 3. [의뢰(퀘스트)](#quest)
> 4. [스포너 및 검증 툴](#spawner)
> 5. [Blueprint -> C++ 포팅](#bp-to-cpp)
> 6. [재접속](#reconnect)
> 7. [엔진 및 플러그인 개선 경험](#plugin)

<a name="skill"></a>
## 1. Skill
### 1.1 소개
<details>
<summary> 접기 / 펼치기 </summary>
<img src="https://github.com/BUOMACC/ProjectHistory/blob/main/Images/HA_Skill_01.png" width="100%" height="100%"/>

  
전투를 진행하면서 사용되는 일반 공격이나 구르기, 그리고 Q / E를 눌러 발동시키는 연계 기술들의 사용을 관리하는 시스템입니다.  
플레이어가 스킬을 사용할 수 있도록 스킬 객체를 관리하고 서버와의 일관성을 유지하며 Cost / Cooldown에 대한 처리를 담당합니다.
<br/>
<br/>

<img src="https://github.com/BUOMACC/ProjectHistory/blob/main/Images/HA_Skill_02.png" width="100%" height="100%"/>


Root 스킬(Q / E)을 사용하는 동안 좌 / 우 마우스 버튼을 통해 트리에 연결된 스킬이 이어서 실행됩니다.    
또한 노란색 테두리로 표시되는 시간 동안 키(E)를 다시 입력하면 두 번째 Root 스킬로 연계됩니다.  
</details>

### 1.2 업무 내용
- 다수의 자식을 가질 수 있는 스킬자원 관리 시스템 설계
- 스킬 간 연결 복잡도로 확장시 구조 변경 비용을 최소화하기 위해 트리 구조 기반으로 설계 및 구현
  → 코드 변경 최소화, 스킬 커스터마이징 / 테이블을 통한 기능확장 제공
- 클라이언트 에측 + 서버 검증 방식으로 자원 상태 동기화 처리
- 디버그 UI 및 치트 개발 → 스킬 데이터 튜닝 시간 단축


<a name="stat"></a>
## 2. Character Stat
- 기존 스텟 로직이 분산되어, 확장하거나 수정하는데 많은 코드 변경이 필요한 문제 → 공통 스텟 구조체 중심의 처리 흐름으로 재설계
- 스텟 계산 로직 통합으로 표준화하여 결과 예측이 용이해짐
- 코드 수정을 최소화하고 테이블 기반으로 확장 가능해짐

<a name="quest"></a>
## 3. 의뢰(퀘스트)
### 3.1. 소개
<details>
<summary> 접기 / 펼치기 </summary>
<img src="https://github.com/BUOMACC/ProjectHistory/blob/main/Images/HA_Quest_01.png" width="75%" height="75%"/> 

  
로비에서 다수의 퀘스트를 수락받고 인게임에서 특정 행동(납품 / 처치 / 탈출)을 통해 하위 목표를 달성하여 퀘스트를 완료하는 시스템입니다.
</details>

### 3.2. 업무 내용
- 목표 기반(처치/수집/납품) 퀘스트 시스템 설계 및 구현
- 목표를 세분화하여 코드 구조 변경을 최소화 → 테이블을 통해 다양한 퀘스트로 확장
- 서버와의 시간 차이를 기준으로 계산 및 처리 → 네트워크 비용 절감 및 클라이언트 시간 조작 방지
- 텍스트 포맷 기반의 Localization 대응


<a name="spawner"></a>
## 4. 스포너 및 검증 툴
- 몬스터/아이템 스폰 시점, 스폰 대상, 위치 등을 제어하는 스포너 시스템 설계 및 개발
- 대량의 테이블 데이터에서 휴먼에러로 인한 긴 테스트 시간 발생
- Editor Utility Widget 기반 데이터 검증 툴을 개발하여, 휴먼에러로 인한 디버깅 시간을 기존 30분~1시간 → 10분 이내로 단축


<a name="bp-to-cpp"></a>
## 5. Blueprint -> C++ 포팅
- Blueprint 수정 / 로드시 분 단위로 지연되고 불필요한 에셋 로드가 발생하여 작업효율 저하
- Blueprint 간 순환 참조로 인한 의존성이 원인임을 파악 (에셋로드 + 재컴파일)
- Interface 기반 분리를 적용했으나 관리 복잡 및 코드 연동 이슈로 유지보수 효율이 낮다고 판단
- 핵심 로직을 C++로 이전하고, 필요한 경우 Soft Reference로 관리하여 의존성 최소화 → 수정 / 로드시간 분 단위에서 10초 이내로 단축


<a name="reconnect"></a>
## 6. 재접속
- Inactive PlayerState 기반의 프레임워크 흐름에 맞도록 재연결 구조 설계
- 플레이어 연결 해제시 발생하는 Character Destroy 및 CMC 비정상 동작 문제 해결


<a name="plugin"></a>
## 7. 엔진 및 플러그인 개선 경험
- UMG Drag & Drop 시작시 지연되어 따라오던 원인 분석 및 엔진 코드 수정
- Able Ability System 기능 확장 (Task 확장 / Task 우선순위 기능 추가)





