## Java의 정렬  
1. 가변 정렬  
정렬 메소드를 호출했을 때, 인자로 넘겨진 정렬 그 자체가 스스로 정렬되는 가변(mutable) 정렬은 Arrays.sort와 Collections.sort 두 정적 메소드로 할 수 있습니다.
```java
// Arrays.sort
        int[] arr = new int[size];
        for(int i = 0; i < size; i++) arr[i] = rand.nextInt();
        Arrays.sort(arr);
        
// Collections.sort
        ArrayList<Integer> arr = new ArrayList<>();
        for(int i = 0; i < size; i++) arr.add(rand.nextInt());
        Collections.sort(arr);
```
2. 불변 정렬  
정렬 메소드를 호출했을 때, 인자로 넘겨진 정렬은 변하지 않고 정렬된 새로운 값을 리턴하는 불변(immutable) 정렬은 Stream<T>의 메소드로 할 수 있습니다.
```java
  // arr의 타입은 int[]. IntegerStream
  arr = Arrays.stream(arr).sorted().toArray();
  
  // arr의 타입은 ArrayList<Integer>. Stream<Integer>
  arr = arr.stream().sorted().collect(Collectors.toCollection(ArrayList::new));
```
3. 역순 정렬  
내장 라이브러리의 타입이 정렬되기 위해서는 Comparable<T>가 구현되어 있어야 합니다. 따라서, 역순 정렬 역시 함께 사용할 수 있습니다. 역순 정렬을 사용하는 가장 간단한 방법은 Collections.reverseOrder()를 이용하는 것입니다.  
아래의 Arrays.sort는 primitive-type에 대해서는 적용되지 않습니다. 제네릭 T[] 타입을 요구하지만 primitive-type은 제네릭 타입의 인자로 사용할 수 없기 때문입니다.  
JVM은 제네릭 타입을 Object로 캐스팅하는데, primitive-type은 Object가 아닙니다. .reverseOrder()는 java.util.Comparator<T> 타입을 반환합니다.  
```java

          Arrays.sort(arr, Collections.reverseOrder()); // error?
          Collections.sort(arr, Collections.reverseOrder());
```  

4. Comparable<T>  
새로운 타입을 만들었다고 해 봅시다. 이 타입의 정렬 기준을 컴파일러는 알 수 없으므로, 우리가 새로 만들어줘야 합니다. 이때 Comparable<T> 인터페이스를 구현합니다.  
Comparable<T> 인터페이스 구현은 public int compareTo(T o)를 직접 만들어주면 됩니다. 클래스의 this가 가리킬 인스턴스를 부등호의 좌측 값, compareTo 메소드의 인자를 부등호의 우측 값이라 했을 때,  
좌 < 우 이면 -1 리턴,  
좌 == 우 이면 0 리턴,  
좌 > 우 이면 1을 리턴하게 하면 됩니다. 이때, 부호가 같으면 꼭 +/-1을 리턴하지 않아도 됩니다. 예를 들어 -2를 리턴하는 것도 괜찮습니다.

```java
  class Obj implements Comparable<Obj> {
        int value;
        
        @Override
        public int compareTo(Obj o) {
            if(this.value < o.value) return -1;
            else if(this.value == o.value) return 0;
            else return 1;
        }
    }
```
5. Comparator<T>  
앞서 살펴본 .sort 함수는 두 번째 매개변수로 인터페이스 Comparator<T>의 인스턴스를 받습니다.  
만일 우리가 기존의 Comparable 규칙을 망가뜨리지 않고 다른 식으로 정렬하고 싶거나, 한두 번만 다른 규칙으로 정렬하고 싶다면 Comparator<T>를 정렬 메소드에 넘겨주면 됩니다.  
Comparator<T>는 익명 클래스를 사용하여 인스턴스를 만듭니다. 이때는 int compare(T o1, T o2)라는 메소드를 새로 오버라이딩합니다.  
추상 메소드가 하나이므로, 함수형 인터페이스입니다. 따라서 람다 표현식을 넘기는 것도 괜찮습니다.
```java
        // lambda expression
        Comparator<Integer> comp = (x, y) -> { return x - y;};
        
        // anonymous class
        Comparator<Integer> compp = new Comparator<Integer>() {
            @Override
            public int compare(Integer o1, Integer o2) {
                return o1 - o2;
            }
        };
```
6. 성능  
7. Arrays.sort와 Collections.sort  
