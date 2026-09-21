# LeetCode 3524 – Find X Value of Array I

## Problem
Count non-empty subarrays according to the remainder of their product modulo `k`.

The answer contains `k` values:
- `ans[0]` = number of subarrays with product % k = 0
- `ans[1]` = number of subarrays with product % k = 1
- ...
- `ans[k-1]` = number of subarrays with product % k = k-1

## Approach

Use Dynamic Programming with product remainders.

`dp[r]` stores the number of subarrays ending at the previous index whose product modulo `k` is `r`.

For every `num`:
1. Start a new subarray `[num]`.
2. Extend every previous subarray with `num`.
3. Calculate the new remainder:
   `newRemainder = (oldRemainder * (num % k)) % k`
4. Add the new counts to the answer.

## Java Solution

```java
class Solution {
    public long[] resultArray(int[] nums, int k) {
        long[] ans = new long[k];
        long[] dp = new long[k];

        for (int num : nums) {
            long[] newDp = new long[k];

            int rem = num % k;
            newDp[rem]++;

            for (int r = 0; r < k; r++) {
                if (dp[r] > 0) {
                    int newRem = (r * rem) % k;
                    newDp[newRem] += dp[r];
                }
            }

            for (int r = 0; r < k; r++) {
                ans[r] += newDp[r];
            }

            dp = newDp;
        }

        return ans;
    }
}
```

## Dry Run

Example:

```text
nums = [1, 2, 3, 4, 5]
k = 3
```

After processing:

```text
num = 1 → dp = [0, 1, 0]
num = 2 → dp = [0, 0, 2]
num = 3 → dp = [3, 0, 0]
```

Continuing the same process gives:

```text
answer = [9, 2, 4]
```

## Complexity

- **Time:** `O(n × k)`
- **Space:** `O(k)`

Because `k` is small, this is effectively a linear-time solution.

## Key Learning

This problem is a useful example of **DP on modulo states**.

Instead of storing every subarray, we only store how many subarrays end at the current position for each possible remainder.
