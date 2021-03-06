# Heap 그리고 Heapq 모듈

본 문서에는 자료구조 힙(Heap)과 파이썬 모듈 `Heapq`에 관하여 공부한 내용을 기록한다.

## Heap 이란?
---

- 완전 이진 트리 중 하나로 우선순위 큐를 위하여 만들어진 자료구조다.
- **여러개의 값들 중 최댓값 또는 최솟값을 빠르게 찾아내기 위하여 고안된 자료구조다. (역시 모든 것은 필요에 의해 만들어진다.)**
- 힙은 일종의 반정렬 상태를 유지한다.
    -   큰 값이 상위 레벨에 있꼬 작은 값이 하위 레벨에 존재한다.
    -   즉, 부모 노드의 키 값이 자식 노드의 키 값 보다 항상 큰(또는 작은) 이진 트리를 의미한다.
- 힙은 중복된 값을 허용한다. (이진 트리는 중복 값을 허용하지 않는다.)

<br>

## Heap의 종류
---

힙은 Max Heap, Min Heap으로 나뉘어진다.

- **MaxHeap**
    - 부모 노드의 키 값이 자식 노드의 키 값 보다 항상 크거나 같은 완전 이진 트리
    - Key(부모 노드) >= Key(자식 노드)

- **MinHeap**
    - 부모 노드의 키 값이 자식 노드의 키 값 보다 항상 작거나 같은 완전 이진 트리
    - Key(부모 노드) <= Key(자식 노드)

![image](https://user-images.githubusercontent.com/33051018/83404099-f007dd00-a444-11ea-9989-a72229ce203a.png)

위 사진은 부모 노드의 키 값이 자식 노드의 키 값 보다 항상 작거나 같으므로 Min Heap이다.

## Heap 모듈 사용방법

파이썬이 제공하는 내장 모듈 `Heapq`의 기본 힙은 부모 노드가 자식 노드의 값 보다 작은 최소 힙이다.


`heapq` 모듈의 `heappush()` 함수를 이용하여 힙에 원소를 추가할 수 있다.

```python
heapq.heappush(heap, 3)
heapq.heappush(heap, 1)
heapq.heappush(heap, 5)
print(heap)

# [1, 3, 5]
```

`heappush()` 함수의 첫번째 인자는 `heap`, 원소를 추가할 대상 리스트이며 두번째 인자는 추가할 원소다.

`push`한 데이터 중 가장 작은값이 0번째 인덱스에 위치하며 순차적으로 크기에 따라 위치한다.

내부적으로 이진 트리에 원소를 추가하는 `heappush()` 함수는 `O(logN)` 의 시간 복잡도를 가진다.

`heap` 모듈에서 원소를 삭제하고자 할 때는 `heappop()` 함수를 이용하여 원소를 삭제할 수 있다.

삭제할 원소가 존재하는 리스트를 인자로 넘길 경우, 가장 작은 값을 삭제하며 이를 리턴한다.

```python
print(heapq.heappop(heap))
# 1
print(heapq)
# [3, 5]
```

이 또한 내부적으로 이진 트리의 원소를 삭제하는 `heappop()` 함수는 `O(logN)` 시간 복잡도를 갖는다.

하지만 0번쨰 인덱스에 최솟값이 위치한다고 해서, 1번 인덱스에 두번째로 작은 원소, 2번 인덱스에 세번째로 작은 원소가 위치한다고 보장할 수는 없다.

왜냐하면 힙은 `heappop()` 함수를 호출할 때 마다, 원소를 삭제하고 이진 트리의 재배치를 통하여 매번 새로운 최솟값을 0번째 인덱스에 위치시키기 때문이다.

따라서, 두번째로 작은 값에 접근하기 위해서는 `heappop()` 하여 가장 작은 값을 삭제하고 `heap[0]`을 통해 0번째 인덱스에 접근해야 한다.

또한, 기존 리스트를 `heap` 형태로 변환하기 위해서는 `heapify()` 함수를 이용한다.

```python
heap = [3, 4, 1, 2, 7, 8]
heapq.heapify(heap)
print(heap)
# [1, 2, 3, 4, 7, 8]
```

이는, 비어있는 리스트를 생성한 이후 `heappush()` 함수를 통해 원소를 하나씩 추가한 효과를 갖는다.

따라서, `heappify()` 함수의 시간복잡도는 인자로 넘기는 리스트의 길이에 비례하여 `O(N)` 시간 복잡도를 갖는다.

파이썬의 `heapq` 모듈이 `MinHeap`만 제공한다고 하여 `MaxHeap`을 사용할 수 없는것은 아니다.

힙에 튜플을 원소로 추가하거나 삭제하면, 튜플 내에서 맨 앞에 있는 값을 기준으로 최소 힙이 구성되는 원리를 이용하여 (우선 순위, 값) 구조의 튜플을 힙에 추가하거나 삭제하여 `MaxHeap`을 만들어 낼 수 있다.

```python

import heapq

numArr = [3, 5, 1, 4, 8 , 2, 7]
heap = []

for num in numArr:
    heapq.heappush(heap, (-num, num))   # (우선 순위, 값)

print(heap)
#[(-8, 8), (-5, 5), (-7, 7), (-3, 3), (-4, 4), (-1, 1), (-2, 2)]

while heap:
    print(heapq.heappop(heap)[1])
# -8, -7, -5, -4, -3, -2, -1
```


<br>


## 파이썬에서의 힙
---

파이썬에서 힙은 `heapq` 모듈과 `Queue` 모듈의 `PriorityQueue` 클래스를 통해 구현가능하다.

다만 둘 모두 `minheap`을 기준으로 구현되어 있다. 따라서 가장 앞에 있는 원소가 가장 작은 원소다.

둘의 공통점과 차이점은 아래와 같다.

- 공통점
    - 둘 다 `minheap` 으로 구현되어 있다. 즉 가장 앞의 원소가 가장 작은 원소다.

- 차이점
    - PriorityQueue는 클래스이고, heapq는 모듈이다.

둘 중에서는 `heapq` 모듈이 훨씬 빠른 속도를 보여준다. 데이터 삽입 시 속도 차이가 약 10배 정도 난다.

### 힙은 언제 사용하는가?

- 힙은 데이터가 지속적으로 정렬되어야 하며
- 데이터의 삽입/삭제가 빈번하게 일어날 경우 사용한다.



<br>



### Reference

- [나무위키](https://namu.wiki/w/%ED%9E%99%20%ED%8A%B8%EB%A6%AC)
- [초보몽키의 개발공부로그](https://wayhome25.github.io/cs/2017/04/19/cs-23/)



