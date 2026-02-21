# Hunter's Arena : Revolution
**게임 소개 영상 : https://www.youtube.com/watch?v=DAcVITWIVzc**

Megalink(前 Mantisco)의 이전 작 Hunter's Arena : Legends IP를 활용한 익스트랙션 장르의 게임입니다.


최대 8명의 플레이어가 한 세션에 입장하여 전투나 오브젝트와의 상호작용을 통해 아이템을 파밍하고 탈출하는 방식으로 진행됩니다.

<a name="table-contents"></a>
## 대표 작업 내역
> 1. [Skill](#skill)
> 2. [의뢰(퀘스트)](#quest)
> 3. [채굴기(BM)](#mining-machine)
> 4. [몬스터 / 아이템 Spawner](#spawner)
> 5. [Character Stat](#stat)
> 6. [재접속 기능](#reconnect)
> 7. [드래그 팝업 베이스](#drag-popup)
> 8. [시스템 메시지](#system-message)
> 9. [Blueprint -> C++ 포팅](#bp-to-cpp)
> 10. [얕은 플러그인 및 엔진 수정](#plugin)

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
- 공격, 회피, 기술의 ID 정보를 기반으로 객체를 관리하고 권한과 트리 형태의 연계 구조를 관리하는 시스템 설계 및 구현
  > 실제 스킬의 실행 로직이 아닌, 사용 가능 여부와 연계 가능한지 등 스킬 객체의 현재 상태들을 관리하는 시스템
- 다수의 자식 스킬을 가지고 하나의 부모를 가질 수 있도록 트리 구조로 설계하여 커스터마이징 기능 제공
- 재입력 방식(Q / E)과 분기 입력 기반(LMB / RMB) 연계 상태를 구분하여 적절한 연계 스킬로 이어지도록 처리
- 클라이언트 예측 방식으로 서버에서 자원을 검증하고 일치하지 않다면 서버의 값으로 보정하도록 동기화 처리
- 실시간 기능 테스트를 위해 현재 보유 스킬 상태를 확인하는 디버깅 UI와 스킬 트리를 변경할 수 있는 치트 개발


<a name="quest"></a>
## 2. 의뢰(퀘스트)
### 2.1. 소개
<details>
<summary> 접기 / 펼치기 </summary>
<img src="https://github.com/BUOMACC/ProjectHistory/blob/main/Images/HA_Quest_01.png" width="75%" height="75%"/> 

  
로비에서 다수의 퀘스트를 수락받고 인게임에서 특정 행동(납품 / 처치 / 탈출)을 통해 하위 목표를 달성하여 퀘스트를 완료하는 시스템입니다.
</details>

### 2.2. 업무 내용
- Game Server / Dedicated Server 환경을 모두 고려하여 다수의 목표 달성 후 완료하는 방식의 퀘스트 시스템 설계 및 구현
- 하나의 퀘스트가 다수의 목표를 가질 수 있는 구조로 설계하고, 처치/수집/납품 등 목표 행동을 세분화
- Dedicated Server에서 플레이 중 발생하는 행동 진행도를 실시간으로 처리하여 동기화
- 일일/주간 등 시간제한 퀘스트의 경우 서버와의 시간 차이를 기준으로 남은 시간을 계산하도록 하여  
  반복적인 시간 동기화를 위한 핑퐁 비용을 최소화하고 로컬 시간 변경에 대응함
- UI 갱신 시 전체를 재구성하지 않고 변경된 일부만 업데이트하도록 최적화 처리
- 퀘스트 진행도 및 목표 텍스트를 포맷 기반으로 처리하여 다국어 환경에서도 문맥이 자연스럽게 유지되도록 Localization 대응


<a name="mining-machine"></a>
## 3. 채굴기(BM)
### 3.1. 소개
<details>
<summary> 접기 / 펼치기 </summary>
<img src="https://github.com/BUOMACC/ProjectHistory/blob/main/Images/HA_Extractor_01.png" width="75%" height="75%"/>

게임에서 얻은 아이템을 기계(추출기)를 이용해 재화로 추출하는 시스템입니다.
</details>

### 3.2 업무 내용
- 재화를 생산하는 채굴기 시스템 설계 및 구현
- 실제 배치되는 시각적 Actor와 로직을 담당하는 Object로 분리하여 목적에 맞는 구조로 설계
- 임대(사용 권한), 연료(공급 에너지), 추출 시간 총 3가지 시간 조건으로 구성하고 아이템 투입 후 추출 완료 시 보상을 획득하는 흐름
- 정확한 시간 계산을 위해 UTC와 서버와의 딜레이 시간을 활용해 서버와의 지속적인 핑퐁 과정 없이도 정확한 상태를 유지하도록 구현
  > 이때 추출이 완료되거나 연료가 소진되는 등 상태 변화가 발생하는 시점이 되면 클라이언트에서 판단해 서버에 갱신을 요청하는 방식
- 강화 시스템을 통한 연출과 강화 결과에 따라 추출기의 상태 변화 로직 구현

<a name="spawner"></a>
## 4. 몬스터 / 아이템 Spawner
- 몬스터/아이템 스폰 시점, 스폰 대상, 위치 등을 제어하는 스포너 시스템 설계 및 개발
- 스포너가 생성한 오브젝트를 그룹 단위로 관리하여 일괄적으로 활성/비활성 처리하는 등 상태 추적이 가능하도록 구현
- 스포너의 공통 기능을 Base 클래스로 분리하고, 이를 상속받아 단일/다중/반복 스폰 등 다양한 규칙으로 확장할 수 있도록 구조화
- 잘못된 데이터 기입으로 인한 장시간 테스트 문제 해결을 위해 스포너를 일괄적으로 검증하고 관리할 수 있는 Editor Utility Widget을 개발    
  이를 통해 휴먼에러로 인한 디버깅 시간을 기존 30분~1시간 수준에서 10분 이내로 단축

<a name="stat"></a>
## 5. Character Stat
- 체력, 스테미너, 공격력 등 플레이어 상태를 관리하는 캐릭터 스텟 시스템 설계 및 구현
- 기존 스텟 계산식과 계수 적용 로직이 여러 컴포넌트에 분산되어 있어 갱신 시 테이블 기반 재계산이 반복되고  
  결과를 예측하기 어려운 구조적 문제가 있었으며 이를 개선하기 위해 공통 스텟 구조체 중심의 처리 흐름으로 재설계함
- Base / Add / Percent 형태로 값을 분리하고 최종 계산을 한 곳에서 수행하도록 통합하여 결괏값의 예측이 쉽도록 함
- 스텟을 개별 타입으로 관리되는 구조체로 정의하여 테이블에 타입을 추가하는 방식만으로 신규 스텟을 등록할 수 있도록 구조화
- 최소 / 최댓값 제한, 특정 스텟 간 상호 의존 관계(ex: 체력은 최대 체력보다 높으면 X) 등을 타입과 테이블의 조합으로 설정할 수 있도록 구현
- 스텟 효율 감쇠 처리 및 상위 스텟 변경시 하위 파생 스텟에 영향을 주고 갱신되는 구조를 설계 
  
<a name="reconnect"></a>
## 6. 재접속 기능
- 플레이어가 이탈했을 경우, 세션이 유지되는 동안 재접속 시 기존 상태로 복구할 수 있도록 기능 설계
- GameMode에서 InactivePlayerState로 전환된 PlayerState와 플레이어 고유 Idx를 매칭하여 재접속 시 기존 Character에 재연결되도록 구현
- 플레이어 이탈 시 발생하던 이슈에 대한 처리
  > Character가 자동으로 Destroy 되던 현상 -> APlayerController::Destroyed() 오버라이딩 하는 방식으로 해결
  > CharacterMovementComponent의 로직이 동작하지 않던 현상 -> bRunPhysicsWithNoController Flag를 true로 설정하여 해결

<a name="drag-popup"></a>
## 7. 드래그 팝업 베이스
### 7.1 소개
<details>
<summary> 접기 / 펼치기 </summary>
<img src="https://github.com/BUOMACC/ProjectHistory/blob/main/Images/HA_DragPopup_01.png" width="100%" height="100%"/>  
게임 내 드래그 가능한 형태의 팝업 베이스로 Inner Content 위젯을 등록하는 것으로 드래그 가능한 위젯이 됩니다.
</details>

### 7.2 업무 내용
- UMG를 활용해 드래그 가능한 팝업 형태의 위젯 개발
- 반복적으로 사용되는 드래그 로직의 중복을 줄이기 위해 공통 로직을 베이스 클래스로 분리하여 설계
- 외부에서 호출 가능한 인터페이스를 통해 다양한 UI에서 드래그 기능을 별도로 구현 없이 사용 가능하도록 구성

<a name="system-message"></a>
## 8. 시스템 메시지
- 게임 중 표시되는 시스템적 메시지(Top, Middle, Chat)를 공통 구조로 관리할 수 있도록 설계
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




















