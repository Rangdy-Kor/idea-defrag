---
publish: true
---

# 나만의 Minecraft 전용 DSL

---

## 개념

---

- **Entity**:
  - 게임 세계에 존재하는 모든 객체이다.
  - 플레이어, 몹, 아이템, 발사체, 블록 엔티티 등을 포함한다.
- **Query**:
  - 하나 이상의 Entity를 선택하는 식이다.
  - Query의 결과는 언제나 Entity의 집합이다.
- **Local Variable**:
  - 각 Entity의 독립적으로 저장되는 데이터이다.
  - 동일한 이름의 Local Variable이더라도 Entity마다 서로 다른 값을 가진다.
- **Global Variable**:
  - 세계 단위로 저장되는 데이터이다.
  - 모든 Entity가 동일한 Global Variable을 공유한다.

**주변 10블록 반경 안의 땅을 딛고 있는 모든 좀비 죽이기**:

```
@entity[
	type = minecraft:zombie,
	distance <= 10f,
	component.on_ground=true
] | kill -target @this
```

**새로운 엔티티 구분하기**:

```
@entity[
	variable.new_entity = none
] | set variable.new_entity = true

# 초기화 로직

@entity[
	variable.new_entity=true
] | set @this.variable = false
```

**움직임이 10분 간 멈춘 플레이어 쫓아내기**:

```
$player = @entity[
	gamemode = survival,
	permission<4
]

$player[
	variable.{
		idle_time = none,
		last_position = none,
	}
] | set variable.idle_time = 0 | 
	set variable.last_position = [0f, 0f, 0f]


loop 1s {
	$player[
		component.position = variable.last_position
	] |	set variable.idle_time += 1
	
	$player[
		component.position != variable.last_position
	] | set variable.{
		idle_time = 0,
		last_position = component.position
	}
}

$player[
	variable.idle_time >= 600
] | kick -target @this -message "AFK"
```
