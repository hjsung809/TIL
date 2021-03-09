# 컬렉션 프레임 워크 팁



## 고정된 객체들로 구성된 List

이런 경우 Arrays.asList(T... a) 메소드를 사용하면 간편하다

```java
List<T> list = Arrays.asList(T... a);
```

아래와 같이 사용하면된다.

```
List<String> list1 = Arrays.asList("홍길동", "신용권", "감자바");
```



## ArrayList 와 Vector

두개의 차이점은 Vector가 동기화된(synchronized) 메소드로 구성되어 있기 떄문에 멀티 스레드가 동시에 이 메소드들을 실행할 수 없고 하나의 스레드가 실행을 완료해야만 다른 스레드들가 실행할 수있다는 점이다. Thread Safe한 것이다.



## HashSet

HashSet은 Set인터페이스의 구현 클래스이다. HashSet은 데이터를 저장하기전에 hashcode() 메소드를 호출해서 해시코드를 얻어내고, 이미 저장되어 있는 객체들의 해시코드와 비교한다. 동일한 해쉬코드가 있으면 다시 equals() 메소드로 두 객체를 비교해서 true가 나오면 동일한 객체로 판단하고 중복 저장을 하지 않는다. String과 같은 내장 클래스는 이미 hashcode() 메소드와 equals() 메소드가 적절히 오버라이딩 되어있다. 만약 사용자가 정의한 클래스에 대해 HashSet을 사용하고 싶다면 hashcode()메소드와 equals() 메소드를 적절히 오버라이딩 해주어야 한다.



## Map의 entrySet()

entrySet()의 리턴형은 Set<Map.Entry<K, V>> 인데, Set의 iterator()를 사용해서 순회할 수 있고, 순회에서 Map.Entry<K, V>형 변수에서 getKey(), getValue() 메소드로 요소의 키와 값에 동시에 접근할 수있다.



## HashMap

HashMap도 HashSet과 같이 hashcode() 와 equals() 메소드를 사용한다. key 값에 대해서  동등한지 판단하기 위해서 사용하는 것이다. 새로운 클래스를 정의하고 HashMap에 그 클래스를 키로 사용하고싶다면 hashcode() 메서드와 equals() 메서드를 적절히 재정의 해야한다.

HashTable도 HashMap과 다르지 않지만 Thread Safe하다는 특징을 가진다.



## TreeSet

### 검색

TreeSet은 이진 트리를 기반으로한 Set이다. 이진 트리는 부모보다 작은 값은 왼쪽, 큰 값은 오른쪽에 저장하는 자료구조이다. TreeSet은 Set 인터페이스에 대입해도 되지만 TreeSet 클래스 타입으로 사용하는 경우가 많은데 범위 검색과 관련된 메소드를 사용하기 위해서이다.

- E first(): 트리에서 가장 낮은 값을 가진 객체를 리턴.
- E last(): 트리에서 가장 높은 값을 가진 객체를 리턴.
- E lower(E): 주어진 객체보다 바로 아래 객체를 리턴.
- E higher(E): 주어진 객체보다 바로 위 객체를 리턴
- E floor(E): 주어진 객체와 동등한 객체가 있으면 리턴, 없으면 바로 아래 객체를 리턴.
- E celling(E): 주어진 객체와 동등한 객체가 있으면 리턴, 없으면 바로 위의 객체를 리턴.
- E pollFirst(): 제일 낮은 객체를 꺼내오고 컬렉션에서 제거함.
- E pollLast(): 제일 높은 객체를 꺼내오고 컬렉션에서 제거함.



### 정렬

트리를 정렬된 순서로 순회하려면 다음 두가지 메소드를 사용하면된다

- Iterator<E> descendingIterator(): 내림차순으로 정렬된 Iterator를 리턴
- NavigableSet<E> descendingSet(): 내림차순으로 정렬된 NavigableSet 반환.

만약 오름차순으로 정렬된 객체를 얻고 싶으면 descendingSet() 메소드를 2번쓰면된다.

```java
NavigableSet<E> descendingSet = treeSet.descendingSet();
NavigableSet<E> ascendingSet = descendingSet.descendingSet();
```



### 범위 검색

TreeSet이 가지고 있는 범위 검색과 관련된 메소드는 아래와 같다.

- NavigableSet<E> headSet(E, boolean inclusive): 주어진 객체보다 낮은 객체들을 NavigableSet으로 리턴. 
- NavigableSet<E> tailSet(E, boolean inclusive): 주어진 객체보다 높은 객체들을 NavigableSet으로 리턴.
- NavigableSet<E> subSet(E from, boolean fromInclusive, E to, boolean toInclusive): from과 to 사이의 객체들을 NavigableSet으로 리턴.



## TreeMap

TreeSet과 비슷하지만 키와 값이 저장된 Map.Entry를 저장하는 컬렉션이다. 키값을 기준으로 낮은것은 부모의 왼쪽 노드, 높은 것은 부모의 오른쪽 노드에 저장된다. 객체를 저장하면 자동으로 정렬이된다. 이또한 Map 타입으로 쓰기보다는 TreeMap 타입으로 써서 범위 검색과 관련된 메소드를 사용한다.

### 검색

- firstEntry(), lastEntry()
- lowerEntry(K), higherEntry(K)
- floorEntry(K), cellingEntry(K)
- pollFirstEntry(), pollLastEntry()

키에 대해 진행한다는 점을 빼고는 TreeSet과 같은 원리이다. 



### 정렬

- NavigableSet<K> descendingSet()
- NaviagableMap<K, V> descendingMap()

descendingSet()은 키에 대해 내림차순으로 정렬된 셋을 리턴하고, descendingMap()은 키에 대해 내림차순으로 정렬된 맵을 리턴한다.



### 범위 검색

키의 범위에 따라 검색할 수 있는 메서드는 아래와 같으며 원리는 Set과 같으니 생략한다.

- NavigableMap<K, V> headMap(K, boolean Inclusive)
- NavigableMap<K, V> tailMap(K, boolean Inclusive)
- NavigableMap<K, V> subMap(K, boolean, K, boolean)





## Comparable과 Comparator

TreeSet과 TreeMap을 쓰려면 대상 클래스(Map의 경우 Key)가  java.lang.Comparable 인터페이스를 구현하고 있어야한다. 이 인터페이스는 int compareTo() 메소드를 제공하는데, 대상 인스턴스보다 낮은 인스턴스가 들어오면 -1을 리턴하고 반대의 경우 1을 리턴, 동등하다면 0을 리턴하게끔 구현해야한다. 대상 클래스가 Comparable을 구현하고 있지 않다면 ClassCastException이 발생하는데, TreeSet또는 TreeMap의 생성자에 정렬자(Comparator)를 제공하면 Comparable 비구현 객체도 사용할 수 있다.

Comparator는 int compare(T o1, T o2) 와 같은 형태의 인터페이스이며, 두 객체가 동등하면 0, o1과 o2가 순서대로 오게하려면 음수, 거꾸로 오게하려면 양수를 사용하면된다. 정수의 경우 o1 - o2를 리턴하면된다.