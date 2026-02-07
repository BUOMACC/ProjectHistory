# Hunter's Arena : Revolution
**게임 소개 영상 : https://www.youtube.com/watch?v=DAcVITWIVzc**

Megalink(前 Mantisco)의 이전 작 Hunter's Arena : Legends IP를 활용한 익스트랙션 장르의 게임입니다.


최대 8명의 플레이어가 한 세션에 입장하여 전투나 오브젝트와의 상호작용을 통해 아이템을 파밍하고 탈출하는 장르입니다.

<a name="table-contents"></a>
## 대표 작업 내역
> 1. [Skill](#skill)
>    * 하나의 스킬 슬롯에 여러개의 스킬을 트리 형태로 연결하여 원하는 다음 연계 스킬로 이어질 수 있게 커스터마이징 가능하도록 개발
> 3. [의뢰(퀘스트)](#quest)
> 4. [채굴기(BM)](#mining-machine)
> 5. [몬스터 / 아이템 Spawner](#spawner)
> 6. [Character Stat](#stat)
> 7. [재접속 기능](#reconnect)
> 8. [게임 결과(정산) 처리](#result)
> 9. [드래그 팝업 베이스](#drag-popup)
> 10. [시스템 메시지](#system-message)
> 11. [Blueprint -> C++ 포팅](#bp-to-cpp)
> 12. [Dedicated Server 게임 모드 처리](#gamemode)
> 13. [얕은 플러그인 및 엔진 수정](#plugin)

<a name="skill"></a>
## 1. Skill
### 1.1 기능 소개
<img src="https://github.com/BUOMACC/ProjectHistory/blob/main/Images/HA_Skill_01.png" width="100%" height="100%"/>
전투를 진행하면서 사용되는 일반 공격이나 구르기, Q / E를 눌러 발동시키는 기술들의 사용을 관리하는 시스템입니다.


여기 설명드릴 부분은 스킬 시스템의 일부로, 하나의 스킬 슬롯에 여러개의 스킬을 트리 형태로 할당할 수 있는 기능입니다.
<br/>
<br/>

<img src="https://github.com/BUOMACC/ProjectHistory/blob/main/Images/HA_Skill_02.png" width="100%" height="100%"/>
Root 스킬(Q / E)을 사용하는 동안 좌 / 우 마우스 버튼을 통해 트리에 연결된 스킬이 이어서 실행됩니다.  

또한 노란색 테두리로 표시되는 시간동안 키(E)를 다시 입력하면 두 번째 Root 스킬로 연계됩니다.  

### 1.2 구현
> 1.2.1. 어떻게 구현할 것인가?  
> 1.2.2. 멀티플레이 기반 처리  
> 1.2.3. Root 스킬로부터 다른 스킬로 연계될 때에는 언제든 입력 키가 바뀔 수 있음
> 1.2.3. 기획의 요구사항 파악  

* 하나의 Root 스킬이 다수의 Node 스킬을 가질 수 있어야 하므로 트리 구조로 개발했습니다.
* 무기에 따라 변경될 수 있기 때문에 필요한 스킬 데이터만 가지도록 인스턴스를 가집니다.




### 1.2.1 어떻게 구현할 것인가?
Root 스킬로부터 이어질 수 있는 스킬은 다수의 자식을 가지고 하나의 부모를 가지는 방식으로 트리 구조가 가장 적합했습니다.  
따라서 트리 방식으로 구현하고자 했으며, 하나의 스킬에 다수의 스킬들을 저장하고 관리할 수 있도록 했습니다. 

기능을 구현하기 전에 개인 판단하에 적합하지 않아보이는 행동에 대해 기획과 의논하고 예외 상황들을 고려하였습니다.
> + 동일한 스킬을 추가할 수 있는가? -> 트리의 라인별로 스킬 1개씩은 중복 ok
> + 동일한 스킬을 발동했을 때 무기마다 다른 모션이어야 하는가? -> 동일한 스킬로 취급되지만 모션은 달라야 함!
> + 특정 스킬은 Root로만 허용되는가? -> "Start" 타입을 가진 스킬은 Root에만 허용해야 함!
> + 두 번째 Root 스킬로 연계하지 않고 그대로 두면 쿨타임은?

플레이어는 로비에서 스킬을 장착할 수 있고 인게임에서 장착한 스킬 ID를 기반으로 스킬 객체로 인스턴스화 되어 서로 올바른 구조(트리)로 구성됩니다.   





### 1.2.2 멀티플레이 기반의 처리
치팅 방지를 위해 Dedicated Server에서 검증한 뒤 스킬 인스턴스들을 생성 및 구성하고, 클라이언트에게 통지하는 방식으로 구현했습니다.  
클라이언트는 서버에서 받은 데이터를 기반으로 서버와 일치하는 스킬 트리를 가지게 됩니다.  

다음으로 스킬을 처리하는 방식에 대해 고민했습니다.
> * Server-Side vs Client-Prediction  



<a name="quest"></a>
## 2. 의뢰(퀘스트)
의뢰 시스템에 대한 설명입니다.

<a name="mining-machine"></a>
## 3. 채굴기(BM)
채굴기 시스템에 대한 설명입니다.

<a name="spawner"></a>
## 4. 몬스터 / 아이템 Spawner
몬스터 및 아이템 스포너에 대한 설명입니다.

<a name="stat"></a>
## 5. Character Stat
캐릭터 스텟에 대한 설명입니다.

<a name="reconnect"></a>
## 6. 재접속 기능
재접속 기능에 대한 설명입니다.

<a name="result"></a>
## 7. 게임 결과(정산) 처리
게임 정산에 대한 설명입니다.

<a name="drag-popup"></a>
## 8. 드래그 팝업 베이스
드래그 팝업에 대한 설명입니다.

<a name="system-message"></a>
## 9. 시스템 메시지
시스템 메시지에 대한 설명입니다.

<a name="bp-to-cpp"></a>
## 10. Blueprint -> C++ 포팅
포팅에 대한 설명입니다.

<a name="gamemode"></a>
## 11. Dedicated Server 게임 모드 처리
게임 모드 처리에 대한 설명입니다.

<a name="plugin"></a>
## 12. 얕은 플러그인 및 엔진 수정
플러그인 및 엔진 수정에 대한 설명입니다.




















