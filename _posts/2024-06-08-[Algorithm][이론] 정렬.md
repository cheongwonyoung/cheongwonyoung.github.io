---
title: "[Algorithm] 정렬"
excerpt: ""

categories:
  - ALGORITHM_STUDY
tags:
  - []
# toc(Table of Contents.) : true시 포스트의 목차가 보임.
toc: true
# true로 해주면 목차가 스크롤을 따라 움직이게 됨.
toc_sticky: true

# date : 글 처음 작성일
date: 2024-06-08
# last_modified_at : 글 마지막 수정일
last_modified_at: 2024-07-09
---

# 정렬

### 버블정렬

- 장점 : 구현이 쉽다(코드가 직관적)
- 단점 : 시간복잡도가 O(n<sup>2</sup>)로 비효율적인다.

- 인접한 두 원소를 비교하며, 큰 수를 계속하여 뒤로 보내 정렬하는 방식.(오름차순으로 정렬하고자 할 때)
- 크기가 커질수록 비효율적인 알고리즘이며, 대부분의 실제 구현에서는 다른 정렬 알고리즘을 사용한다.
  ![alt text](/assets/img/image.png)
  오름차순으로 정렬할 때,  
   위와 같이 인접한 두 원소의 값을 비교해가며 왼쪽의 값이 오른쪽의 값보다 클 경우 두 원소의 위치를 변경시킨다.
  이를 코드로 구현하면 아래와 같다.

```java
for(int i = 0; i < arr.length-1; i++) {
    // -i를 하는 이유
    // 제일 큰 수가 맨 마지막으로 정렬이 됨 (맨 마지막은 비교 안해도 됨)
    for(int j = 0; j < arr.length-1-i; j++) { // 앞 숫자와 뒤 숫자 서로 비교할 반복문
        // j = 0일 때, arr[0] > arr[0+1]로 앞자리가 숫자가 더 크다면 위치 변경
        if(arr[j] > arr[j+1]) {
            int tmp = arr[j];
            arr[j] = arr[j+1];
            arr[j+1] = tmp;
        }
    }
}
```

### 선택정렬

- 장점 : 버블정렬과 마찬가지로 구현이 쉽다.
  실제로 측정해보면 버블정렬에 비해서는 조금 더 빠르다.
- 단점 : 버블정렬과 마찬가지로 시간 복잡도가 O(n<sup>2</sup>)로 비효율적이다.

- 현재 위치에 들어갈 데이터를 찾아 선택하는 알고리즘 방식
- 시간 복잡도는 O(n<sup>2</sup>)이다.
- 정렬 방법

1. 최솟값의 위치를 찾는다
2. 최솟값과 맨 앞 자리의 값을 교환한다
3. 정렬된 위치 이후부터 1~2번 과정을 반복한다.

![alt text](/assets/img/image-1.png)

```java
for(int i = 0; i < size - 1; i++) {
    int min_index = i;

    // 최솟값을 갖고있는 인덱스 찾기
    for(int j = i + 1; j < size; j++) {
        if(a[j] < a[min_index]) {
            min_index = j;
        }
    }

    // i번째 값과 찾은 최솟값을 서로 교환
    int temp = a[i];
    a[i] = a[min_index];
    a[min_index] = temp;
}
```

### 삽입정렬

- 장점 : 최선의 경우 O(n)이라는 빠른 효율성을 가지고 있다.
- 단점 : 최악의 경우 O(n<sup>2</sup>)라는 시간복잡도를 가지며, 성능의 편차가 심한 정렬방식이다.
- 배열의 모든 요소를 앞에서부터 차례대로 이미 정렬도니 배열 부분(자신의 앞 부분)과 비교하여, 자신의 위치를 찾아 삽입하는 정렬 방식
- 정렬 방법

1. 택배열의 두번째 원소부터 선택
2. 선택한 원소를 적절한 위치에 넣기 위해, 선택한 원소의 왼쪽 방향으로 삽입할 위치를 찾아가며 위치변경

```java
for (int i = 1; i < array.length; i++) {
    int key = i;

    for (int j = i - 1; j >= 0; j--) {
        if (array[key] < array[j]) {
            int temp = array[key];
            array[key] = array[j];
            array[j] = temp;
            key = j;
        }
    }
}
```

### 퀵정렬

- 장점 : 분할 과정에서 O(logN)이라는 시간복잡도를 가지고, 전체적으로 O(NlogN)이라는 시간복잡도를 가지게 된다.
- 단점 : 기준값(Pivot)에 따라서 시간복잡도가 달라진다. 이상적인 값을 택할 땐 NlogN의 시간복잡도를 가지지만, 최악의 Pivot을 선택할 경우
  O(n<sup>2</sup>) 이라는 시간복잡도를 갖게 된다.
- 분할 정복(divide and conquer) 방법을 통해 주어진 배열 정렬
- 정렬 방법

1. 데이터를 분할하는 기준 값 pivot을 설정
2. pivot을 기준으로 start와 end를 기준으로 lo와 hi를 움직인다.
   - 2-1. arr[lo] < arr[pivot]일 때, lo는 오른쪽으로 한 칸 이동
   - 2-2. arr[hi] > arr[pivot]일 때, end를 왼쪽으로 한 칸 이동
   - 2-3. 더이상 이동 못할 때까지 2-1과2-2 반복
3. arr[lo]와 arr[hi] 값을 교체하고 start~(hi-1)과 (hi+1)~end로 부분 정렬에 들어간다

```java
// pivot을 배열의 가장 왼쪽의 숫자로 잡은 경우
private static void quickSort(int[] arr, int start, int end) {
    // start가 end보다 크거나 같다면 정렬할 원소가 1개 이하이므로 정렬하지 않고 return
    if (start >= end)
      return;

    // 가장 왼쪽의 값을 pivot으로 지정, 실제 비교 검사는 start+1 부터 시작
    int pivot = start;
    int lo = start + 1;
    int hi = end;

    // lo는 현재 부분배열의 왼쪽, hi는 오른쪽을 의미
    // 서로 엇갈리게 될 경우 while문 종료
    while (lo <= hi) {
      while (lo <= end && arr[lo] <= arr[pivot]) // 피벗보다 큰 값을 만날 때까지
        lo++;
      while (hi > start && arr[hi] >= arr[pivot]) // 피벗보다 작은 값을 만날 때까지
        hi--;
      if (lo > hi)
        // 엇갈리면 피벗과 교체
        int temp = array[hi];
        array[hi] = array[pivot];
        array[pivot] = temp;
      else
        // 엇갈리지 않으면 lo, hi 값 교체
        int temp = array[lo];
        array[lo] = array[hi];
        array[hi] = temp;
      }

    // 엇갈렸을 경우,
    // 피벗값과 hi값을 교체한 후 해당 피벗을 기준으로 앞 뒤로 배열을 분할하여 정렬 진행
    quickSort(arr, start, hi - 1);
    quickSort(arr, hi + 1, end);

  }
```

### 병합정렬

- 장점 : 항상 O(NlogN)의 시간복잡도를 가진다. 퀵소팅과 달리 Pivot을 성정하지 않고 무조건 절반으로 분할하기 때문에 Pivot에 따라 성능이 안좋아지거나 하는 경우가 없다.
- 단점 : 임시배열에 원본맵을 계속해서 옮겨주면서 정렬하는 방식이기 때문에 추가적인 메모리가 필요하다.
- 원소가 저장되어 있는 배열을 계속 쪼개서 길이가 1인 배열을 만들고, 그 이후 정렬하면서 합치는 알고리즘

![alt text](/assets/img/image-2.png)

```java
src = new int[]{1, 9, 8, 5, 4, 2, 3, 7, 6};
tmp = new int[src.length];

public static void mergeSort(int start, int end) {
    if (start<end) {
        int mid = (start+end) / 2;
        mergeSort(start, mid);
        mergeSort(mid+1, end);

        int p = start;
        int q = mid + 1;
        int idx = p;

        while (p<=mid || q<=end) {
            if (q>end || (p<=mid && src[p]<src[q])) {
                tmp[idx++] = src[p++];
            } else {
                tmp[idx++] = src[q++];
            }
        }

        for (int i=start;i<=end;i++) {
            src[i]=tmp[i];
        }
    }
}
```

### 힙 정렬(Heap Sort) -- 책엔 없었음 (우선순위 큐를 사용하기에 작성)

- 장점 : 항상 O(NlogN)이라는 시간복잡도를 가짐
- 단점 : 퀴정렬과 같은 다른 O(NlogN)에 비해 실제 측정에서 느리다.
- 최솟값 또는 최댓값을 빠르게 찾아내기 위해 완전이진트리 형태로 만들어진 자료구조
- PriorityQueue를 사용해서 Heap을 사용할 수 있다.

```java
PriorityQueue<Integer> heap = new PriorityQueue<Integer>();

heap.add(4);
heap.add(2);
heap.add(1);
heap.add(6);

// 힙에서 우선순위가 가장 높은 원소(root노드)를 하나씩 뽑는다.
// 1,2,4,6
for(int i = 0; i < 4; i++) {
    heap.poll();
}
```

### 기수정렬

- 장점 : O(N)이라는 시간복잡도로 매우 빠르다.
- 단점 : '버킷'이라는 추가적인 메모리가 할당되어야 한다. 즉, 메모리가 많이 소요되는 정렬법이다. 또한, 데이터 타입이 일정한 경우에만 가능하다.
