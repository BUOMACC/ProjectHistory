# Hunter's Arena : Revolution
**게임 소개 영상 : https://www.youtube.com/watch?v=DAcVITWIVzc**

Megalink(前 Mantisco)의 이전 작 Hunter's Arena : Legends IP를 활용한 익스트랙션 장르의 게임입니다.


최대 8명의 플레이어가 한 세션에 입장하여 전투나 오브젝트와의 상호작용을 통해 아이템을 파밍하고 탈출하는 장르입니다.

<a name="table-contents"></a>
## 대표 작업 내역
> 1. [Skill](#skill)
> 2. [의뢰(퀘스트)](#quest)
> 3. [채굴기(BM)](#mining-machine)
> 4. [몬스터 / 아이템 Spawner](#spawner)
> 5. [Character Stat](#stat)
> 6. [재접속 기능](#reconnect)
> 8. [드래그 팝업 베이스](#drag-popup)
> 9. [시스템 메시지](#system-message)
> 10. [Blueprint -> C++ 포팅](#bp-to-cpp)
> 12. [얕은 플러그인 및 엔진 수정](#plugin)

<a name="skill"></a>
## 1. Skill
### 1.1 기능 소개
<img src="https://github.com/BUOMACC/ProjectHistory/blob/main/Images/HA_Skill_01.png" width="100%" height="100%"/>
전투를 진행하면서 사용되는 일반 공격이나 구르기, 그리고 Q / E를 눌러 발동시키는 연계 기술들의 사용을 관리하는 시스템입니다.

플레이어가 스킬을 사용할 수 있도록 스킬 객체를 관리하고 서버와의 일관성을 유지하며 Cost / Cooldown에 대한 처리를 담당합니다.
<br/>
<br/>

<img src="https://github.com/BUOMACC/ProjectHistory/blob/main/Images/HA_Skill_02.png" width="100%" height="100%"/>
Root 스킬(Q / E)을 사용하는 동안 좌 / 우 마우스 버튼을 통해 트리에 연결된 스킬이 이어서 실행됩니다.  

또한 노란색 테두리로 표시되는 시간동안 키(E)를 다시 입력하면 두 번째 Root 스킬로 연계됩니다.  

### 1.2 업무
- 트리 구조를 활용하여 커스터마이징 가능한 범용 스킬 시스템 설계  
  테이블 내 스킬 ID를 추가하는 것으로 빠르게 스킬 추가가 가능함
- 지속적인 기능 테스트를 위한 디버깅 UI 및 치트 개발
- 클라이언트 예측 방식으로 서버에서 이를 검증하고 보상하도록 처리


<a name="quest"></a>
## 2. 의뢰(퀘스트)
- 


<a name="mining-machine"></a>
## 3. 채굴기(BM)
채굴기 시스템에 대한 설명입니다.

<a name="spawner"></a>
## 4. 몬스터 / 아이템 Spawner
- 

<a name="stat"></a>
## 5. Character Stat
캐릭터 스텟에 대한 설명입니다.

<a name="reconnect"></a>
## 6. 재접속 기능
- 플레이어가 이탈했을 경우, 세션이 유지되는 동안 재접속 시 기존 상태로 복구할 수 있도록 기능 설계
- GameMode에서 InactivePlayerState로 전환된 PlayerState와 플레이어 고유 Idx를 매칭하여 재접속 시 기존 Character에 재연결되도록 구현
- 플레이어 이탈 시 발생하던 이슈에 대한 처리
  > - Character가 자동으로 Destroy되던 현상 -> APlayerController::Destroyed() 오버라이딩 하는 방식으로 해결
  > - CharacterMovementComponent의 로직이 동작하지 않던 현상 -> bRunPhysicsWithNoController Flag를 true로 설정하여 해결

<a name="drag-popup"></a>
## 7. 드래그 팝업 베이스
- UMG를 활용해 드래그 가능한 팝업 형태의 위젯 개발
- 반복적으로 사용되는 드래그 로직의 중복을 줄이기 위해 공통 로직을 베이스 클래스로 분리하여 설계
- 외부에서 호출 가능한 인터페이스를 통해 다양한 UI에서 드래그 기능을 별도로 구현 없이 사용 가능하도록 구성

<a name="system-message"></a>
## 8. 시스템 메시지
- 게임중 표시되는 시스템적 메시지(Top, Middle, Chat)를 공통 구조로 관리할 수 있도록 설계
- 유사한 방식으로 동작하는 메시지들을 Container 중심 구조로 통합
- Container 확장을 통해 메시지 유지시간, 최대 개수, 표시 개수, 밀려남 처리 등 노출 규칙을 설정 가능하도록 구현

<a name="bp-to-cpp"></a>
## 9. Blueprint -> C++ 포팅
- Blueprint 수정/컴파일 시 분 단위로 지연되는 문제를 분석
- Blueprint 간 순환 참조로 인한 의존성이 원인임을 파악
- Blueprint Interface를 활용하여 순환 참조를 끊어내고자 시도했지만 작업 효율이 좋지 않았음
- 핵심 로직의 80% 이상을 C++로 이전하여 의존성을 줄였고, Blueprint 컴파일 시간을 10초 이내로 단축

<a name="plugin"></a>
## 10. 얕은 플러그인 및 엔진 수정
- Able Ability System - 제공되지 않는 Task 우선순위 기능을 Priority 값을 기반으로 실행 순서가 제어되도록 확장
- Able Ability System - 프로젝트 요구사항에 맞는 커스텀 Task를 추가 구현
- UMG Drag & Drop 기능에서 DragVisual이 천천히 따라오는 원인을 확인하고 엔진 코드를 수정해 마우스 위치에 즉각 반응하도록 개선




















