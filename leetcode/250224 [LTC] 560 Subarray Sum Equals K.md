# 250224 [LTC] 560. Subarray Sum Equals K

Date: 2025년 2월 24일
사이트: 리트코드
알고리즘: 누적합

<aside>
📌

**Topic**

누적합

</aside>

<aside>
📌

**문제**
**2월 24일 스터디 1회차 일정 (모든 시간은 KST 기준)**

**11시 00분 시작**

**11시 00분 ~ 11시 40분 각자 풀이 발표 및 설명**

- [Subarray Sum Equals K](https://leetcode.com/problems/subarray-sum-equals-k/description/)
</aside>

### **LTC. Subarray Sum Equals K**

**문제 간단 설명**

<aside>

정수 배열 `nums`와 정수 `k`가 주어졌을 때, **연속된 부분 배열(subarray)의 합이 k가 되는 경우의 수**를 반환하는 문제

</aside>

**문제 풀이**

<aside>

연속적인 부분 배열을 찾는것이기 때문에 누적합을 통해 연속된 부분 배열을 찾아주면 된다. 즉 현재의 누적합이 10이고 k = 2 라면 이전 인덱스들 중 배열의 누적합이 8인 것을 찾으면  그 사이의 값이 2가 되므로 연속되 부분 배열을 찾을 수 있다. 여기서의 핵심은 누접합과 인덱스 인데 현재 인덱스 값보다 큰 인덱스를 가진 배열의 누적합이 8이라면 -2가 되므로 k가 맞지 않기 때문에 항상 현재 인덱스보다 작은 값의 인덱스의 누적합만 체크해주면 된다. 따라서 누접합을 딕셔어리에 키값으로 두고 인덱스 값을 리스트로 넣게 되면 앞쪽에는 인덱스가 작은 값이 뒤쪽에는 인덱스가 큰값이 저장되기 때문에 필요한 누적합을 구하고 딕셔어리에서 인덱스 값을 비교하여 현재 인덱스값보다 작은것을 카운트 해주면 된다. 그리고 현재 누접합이 답이 될수있으므로 해당값도 따로 카운트 해주어야한다.

</aside>

**코드**

```python
from collections import defaultdict
class Solution:
    def subarraySum(self, nums: List[int], k: int) -> int:
        sumconut = 0
        arr = defaultdict(list)
        check = []
        for inx, v in enumerate(nums):
            sumconut += v
            arr[sumconut].append(inx)
            check.append(sumconut)

        result = 0

        for inx, v in enumerate(nums):
            current = check[inx] - k
            if check[inx] == k:
                result += 1
            if current in arr:
                for i in arr[current]:
                    if i < inx:
                        result += 1
                    else:
                        break
            
        return result
```