# 2D-RPG 모바일 액션 게임 통합 기획서 v0.5

작성 기준일: 2026-09-20

## 1. 프로젝트 정의
- 모바일용 가로형 벨트스크롤 로그라이트 액션 게임
- Godot 4.x Stable
- Android 우선 출시
- 가로 화면 전용
- 사용자: 기획 및 플레이 테스트
- Claude Code: 메인 구현
- ChatGPT: 기획 보완, 아키텍처 검토, 코드 리뷰, 이미지/에셋 제작
- GitHub 저장소: jhyiplab9278-hub/2D-RPG
- 핵심 제약: 2D 캐릭터 애니메이션 에셋 제작량

## 2. 전투 핵심
- 주무기: 카타나
- 보조무기: 권총
- 점프 없음
- 지상 중심 이동 및 전투
- 적만 런치 가능
- 기본 조작: 좌측 가상 스틱 + A/B/C/D
- A: 카타나 기본 공격
- B: 대시/회피
- C: 카타나 고유 스킬
- D: 권총 사격

### 기본 4타 콤보
1. Attack_01: 빠른 횡베기
2. Attack_02: 반대 방향 대각 올려베기
3. Attack_03: 전진 대각 내려베기
4. Attack_04: 강한 피니시

### 대표 차별화 후보
- 콤보 편집
- 검흔 기억
- 권총 × 검흔 상호작용

## 3. 캐릭터 디자인 기준
### 여성 기준 캐릭터
- 긴 은백색 머리
- 붉은 눈
- 검은 하이테크 전투복
- 붉은 기계 관절 및 붉은 라인
- 붉은 에너지 카타나
- 슬림한 체형
- strict side-view 대응

### 좌우반전 친화 규칙
- 검집은 뒤 중앙 배치
- 권총 홀더는 제거
- 권총은 Shoot / Shoot_Link 시에만 등장
- 카타나는 검을 뽑은 상태에서 기본 양손 파지
- 허리 천/코트테일은 좌우반전 친화형으로 설계
- 기본 방향 1세트 제작 후 flip_h 사용
- 방향에 민감한 특수동작만 예외적으로 별도 에셋 검토

## 4. 스프라이트 제작 규격
- 픽셀아트 사용하지 않음
- 고해상도 2D 애니메이션 일러스트 스타일
- 하나의 이미지에는 하나의 동작만
- 좌→우 시간 순서
- 단일 행 우선
- 프레임 간 매우 넓은 gutter
- 캐릭터가 작아져도 분리 안정성 우선
- strict side-view
- 동일 카메라, 동일 스케일, 동일 캐릭터, 동일 무기
- 전신 노출
- 순수 #00FF00 배경
- 그림자/바닥/반사 없음
- 각 프레임 발 중앙 아래 동일 baseline의 파란 기준점
- 글자/UI/번호/화살표 금지
- VFX는 캐릭터와 검의 실제 위치를 가리지 않음
- VFX는 옆 프레임을 침범하지 않음

### 후처리
AI Sprite Sheet
→ Frame Split
→ #00FF00 제거
→ Anchor Marker Detection
→ Pivot 정렬
→ 기준점 제거
→ 동일 크기 투명 캔버스
→ 개별 PNG
→ Texture Atlas
→ Godot SpriteFrames

## 5. Godot 구조
CharacterBody2D
- AnimatedSprite2D
- AnimationPlayer
- CollisionShape2D
- Hurtbox
- WeaponHitbox
- VFX

실제 이동은 CharacterBody2D가 담당
Hitbox는 이미지에서 자동 추출하지 않고 데이터로 관리

## 6. 개발 Phase / Day

### Phase 0 — Foundation
- Day 1: Godot 프로젝트 / 저장소 / 폴더 / Android 가로모드
- Day 2: Player 이동 / 8방향 / 벨트스크롤 깊이
- Day 3: Hitbox / Hurtbox / Pushbox / Depth Tolerance / Invulnerability

### Phase 1 — Core Combat
- Day 4: 여성 Master Character 확정
- Day 5: Attack_01 제작 파이프라인
- Day 6: Godot Attack_01 재생
- Day 7: Attack_02
- Day 8: Attack_03
- Day 9: Attack_04
- Day 10: 4타 콤보 연결
- Day 11: Dash / Dash Cancel
- Day 12: Pistol Prototype

### Phase 2 — Combat Sandbox
- Day 13: Enemy Base
- Day 14: 기본 근접 적
- Day 15: 원거리 적
- Day 16: 돌진형 또는 방패형
- Day 17: Launch
- Day 18: Pistol Air Extension
- Day 19: Hit Feel Pass
- Day 20: Combat Sandbox Test

### Phase 3 — Signature Combat
- Day 21: ComboData
- Day 22: Combo Editor
- Day 23: Attack Property
- Day 24: Sword Trace
- Day 25: Trace Reactivation
- Day 26: Pistol × Trace
- Day 27: Trace Chain
- Day 28: Trace Visual Language
- Day 29: Signature Combat Test
- Day 30: 유지/단순화 결정 Gate

### Phase 4 — Roguelite Run
- Day 31: RunManager
- Day 32: Encounter
- Day 33: 보상 선택
- Day 34: Cyber Chip
- Day 35: Protocol
- Day 36: 경로 분기
- Day 37: Elite Modifier
- Day 38: Mini Boss
- Day 39: Run Duration Test
- Day 40: Full Run Prototype

### Phase 5 이후
- 카타나 스타일 3종으로 차별성 검증
- 빌드 시스템 확장
- 적 8~10종 + Elite Modifier 중심 콘텐츠 확장
- 보스 3~4종
- UX / 모바일 최적화 / 밸런스
- Google Play 출시 준비

## 7. 아이데이션 백로그
확정 전이며 해당 Day에서 prototype 후 채택 여부 결정
- Just Dodge
- Dash Cancel
- Launch Combo
- Enemy Collision
- Execution
- Combo Editor
- Sword Trace Memory
- Pistol × Sword Trace
- Trace Combination
- Pistol Charge
- Style Meter
- Elite Modifier
- Protocol
- Cyber Chip
- Daily Seed
- Boss Rush
- Endless

### 우선 추천
- Combo Editor
- Sword Trace
- Pistol × Trace
- Dash Cancel
- Launch
- Pistol Charge
- Elite Modifier

## 8. 업무 흐름
사용자 기획/결정
→ ChatGPT 기획 구체화 및 에셋 설계
→ Claude Code 구현
→ GitHub 기능 단위 Commit
→ ChatGPT 코드/구조 검토
→ Godot PC 테스트
→ Android 테스트
→ 사용자 플레이 테스트
→ 수정

## 9. Git 규칙
- main: 안정 버전
- dev: 개발 통합
- feature/*: 기능 단위
- 1 작업 = 1 기능
- 구현 전 관련 문서 갱신
- 검증 후 Commit
- Day 종료 시 /reports 기록

## 10. 현재 우선순위
1. 여성 기준 캐릭터 고정
2. Attack_01 확정
3. Attack_02
4. Attack_03
5. Attack_04
6. 기준점 기반 자동 정렬
7. Godot 실제 재생
8. Idle / Run / Dash / Shoot / Hurt / Death 확장

## 11. 현재 확정 에셋
- Female Master Character: 확정
- Female Attack_01: 확정
- Female Attack_02: 재작업 중
