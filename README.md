# DSA-_Coding_Patterns
contains all the coding patterns along with questions that are commonly asked in companies
### Sliding Window Pattern

**Pattern Explanation:**
The sliding window pattern is useful for solving problems that involve arrays or lists where you need to examine a contiguous block of elements. This block, or "window," moves through the array by sliding from one end to another. It is particularly efficient for problems where the window size is either fixed or dynamically changes based on conditions. This pattern helps in reducing the complexity of nested loops, making the solution more efficient.

**When to Use:**
- When you need to find the maximum, minimum, or sum of elements in a contiguous subarray.
- When you need to keep track of a dynamic subset of elements that satisfy a certain condition.
- When the problem involves sequentially processing elements with overlapping sections.

**Common Constraints:**
- Array size and element values can vary widely.
- Fixed or variable window sizes.
- Conditions to maintain within the window (e.g., sum, product, unique elements).

**Tips:**
1. **Fixed Size Window:** Use a single loop to slide the window from start to end, keeping track of the desired property (e.g., sum).
2. **Variable Size Window:** Use two pointers to dynamically adjust the window size based on conditions (e.g., sum >= target).
3. **Efficiency:** The sliding window pattern often reduces time complexity from O(N^2) to O(N) for many problems.

Let's go through the problems with both brute force and optimized solutions in Java.

#### Problem 1: Maximum Sum Subarray of Size K

**Problem Statement:** Given an array of positive numbers and a positive number `k`, find the maximum sum of any contiguous subarray of size `k`.

**Brute Force Approach:**
```java
public class MaxSumSubarrayOfSizeK {
    public static int findMaxSumSubArray(int k, int[] arr) {
        int maxSum = 0;

        for (int i = 0; i <= arr.length - k; i++) {
            int currentSum = 0;
            for (int j = i; j < i + k; j++) {
                currentSum += arr[j];
            }
            maxSum = Math.max(maxSum, currentSum);
        }

        return maxSum;
    }

    public static void main(String[] args) {
        int[] arr = {2, 1, 5, 1, 3, 2};
        int k = 3;
        System.out.println("Maximum sum of a subarray of size " + k + ": " + findMaxSumSubArray(k, arr)); // Output: 9
    }
}
```

**Optimized Approach:**
```java
public class MaxSumSubarrayOfSizeK {
    public static int findMaxSumSubArray(int k, int[] arr) {
        int maxSum = 0, windowSum = 0;
        int windowStart = 0;

        for (int windowEnd = 0; windowEnd < arr.length; windowEnd++) {
            windowSum += arr[windowEnd];

            if (windowEnd >= k - 1) {
                maxSum = Math.max(maxSum, windowSum);
                windowSum -= arr[windowStart];
                windowStart++;
            }
        }

        return maxSum;
    }

    public static void main(String[] args) {
        int[] arr = {2, 1, 5, 1, 3, 2};
        int k = 3;
        System.out.println("Maximum sum of a subarray of size " + k + ": " + findMaxSumSubArray(k, arr)); // Output: 9
    }
}
```

#### Problem 2: Smallest Subarray with a Given Sum

**Problem Statement:** Given an array of positive integers and a positive integer `S`, find the length of the smallest contiguous subarray whose sum is greater than or equal to `S`. Return 0 if no such subarray exists.

**Brute Force Approach:**
```java
public class SmallestSubarrayWithGivenSum {
    public static int findMinSubArray(int S, int[] arr) {
        int minLength = Integer.MAX_VALUE;

        for (int i = 0; i < arr.length; i++) {
            int currentSum = 0;
            for (int j = i; j < arr.length; j++) {
                currentSum += arr[j];
                if (currentSum >= S) {
                    minLength = Math.min(minLength, j - i + 1);
                    break;
                }
            }
        }

        return minLength == Integer.MAX_VALUE ? 0 : minLength;
    }

    public static void main(String[] args) {
        int[] arr = {2, 1, 5, 2, 3, 2};
        int S = 7;
        System.out.println("Smallest subarray length: " + findMinSubArray(S, arr)); // Output: 2
    }
}
```

**Optimized Approach:**
```java
public class SmallestSubarrayWithGivenSum {
    public static int findMinSubArray(int S, int[] arr) {
        int windowSum = 0, minLength = Integer.MAX_VALUE;
        int windowStart = 0;

        for (int windowEnd = 0; windowEnd < arr.length; windowEnd++) {
            windowSum += arr[windowEnd];

            while (windowSum >= S) {
                minLength = Math.min(minLength, windowEnd - windowStart + 1);
                windowSum -= arr[windowStart];
                windowStart++;
            }
        }

        return minLength == Integer.MAX_VALUE ? 0 : minLength;
    }

    public static void main(String[] args) {
        int[] arr = {2, 1, 5, 2, 3, 2};
        int S = 7;
        System.out.println("Smallest subarray length: " + findMinSubArray(S, arr)); // Output: 2
    }
}
```

#### Problem 3: Longest Substring with K Distinct Characters

**Problem Statement:** Given a string, find the length of the longest substring in it with no more than K distinct characters.

**Brute Force Approach:**
```java
import java.util.HashSet;

public class LongestSubstringWithKDistinct {
    public static int findLength(String str, int k) {
        int maxLength = 0;

        for (int i = 0; i < str.length(); i++) {
            HashSet<Character> distinctChars = new HashSet<>();
            for (int j = i; j < str.length(); j++) {
                distinctChars.add(str.charAt(j));
                if (distinctChars.size() > k) {
                    break;
                }
                maxLength = Math.max(maxLength, j - i + 1);
            }
        }

        return maxLength;
    }

    public static void main(String[] args) {
        String str = "araaci";
        int k = 2;
        System.out.println("Length of the longest substring: " + findLength(str, k)); // Output: 4
    }
}
```

**Optimized Approach:**
```java
import java.util.HashMap;

public class LongestSubstringWithKDistinct {
    public static int findLength(String str, int k) {
        int windowStart = 0, maxLength = 0;
        HashMap<Character, Integer> charFrequencyMap = new HashMap<>();

        for (int windowEnd = 0; windowEnd < str.length(); windowEnd++) {
            char rightChar = str.charAt(windowEnd);
            charFrequencyMap.put(rightChar, charFrequencyMap.getOrDefault(rightChar, 0) + 1);

            while (charFrequencyMap.size() > k) {
                char leftChar = str.charAt(windowStart);
                charFrequencyMap.put(leftChar, charFrequencyMap.get(leftChar) - 1);
                if (charFrequencyMap.get(leftChar) == 0) {
                    charFrequencyMap.remove(leftChar);
                }
                windowStart++;
            }

            maxLength = Math.max(maxLength, windowEnd - windowStart + 1);
        }

        return maxLength;
    }

    public static void main(String[] args) {
        String str = "araaci";
        int k = 2;
        System.out.println("Length of the longest substring: " + findLength(str, k)); // Output: 4
    }
}
```

#### Problem 4: Fruits into Baskets

**Problem Statement:** Given an array of characters where each character represents a fruit tree, you are given two baskets, and your goal is to put the maximum number of fruits in each basket. The only restriction is that each basket can only hold one type of fruit.

**Brute Force Approach:**
```java
import java.util.HashSet;

public class FruitsIntoBaskets {
    public static int findLength(char[] arr) {
        int maxLength = 0;

        for (int i = 0; i < arr.length; i++) {
            HashSet<Character> fruitSet = new HashSet<>();
            int currentLength = 0;
            for (int j = i; j < arr.length; j++) {
                fruitSet.add(arr[j]);
                if (fruitSet.size() > 2) {
                    break;
                }
                currentLength++;
            }
            maxLength = Math.max(maxLength, currentLength);
        }

        return maxLength;
    }

    public static void main(String[] args) {
        char[] arr = {'A', 'B', 'C', 'A', 'C'};
        System.out.println("Maximum number of fruits: " + findLength(arr)); // Output: 3
    }
}

**Optimized Approach:**
```java
import java.util.HashMap;

public class FruitsIntoBaskets {
    public static int findLength(char[] arr) {
        int windowStart = 0, maxLength = 0;
        HashMap<Character, Integer> fruitFrequencyMap = new HashMap<>();

        for (int windowEnd = 0; windowEnd < arr.length; windowEnd++) {
            char rightFruit = arr[windowEnd];
            fruitFrequencyMap.put(rightFruit, fruitFrequencyMap.getOrDefault(rightFruit, 0) + 1);

            while (fruitFrequencyMap.size() > 2) {
                char leftFruit = arr[windowStart];
                fruitFrequencyMap.put(leftFruit, fruitFrequencyMap.get(leftFruit) - 1);
                if (fruitFrequencyMap.get(leftFruit) == 0) {
                    fruitFrequencyMap.remove(leftFruit);
                }
                windowStart++;
            }

            maxLength = Math.max(maxLength, windowEnd - windowStart + 1);
        }

        return maxLength;
    }

    public static void main(String[] args) {
        char[] arr = {'A', 'B', 'C', 'A', 'C'};
        System.out.println("Maximum number of fruits: " + findLength(arr)); // Output: 3
    }
}
```

#### Problem 5: Longest Substring with Distinct Characters

**Problem Statement:** Given a string, find the length of the longest substring, which has all distinct characters.

**Brute Force Approach:**
```java
import java.util.HashSet;

public class LongestSubstringWithDistinctCharacters {
    public static int findLength(String str) {
        int maxLength = 0;

        for (int i = 0; i < str.length(); i++) {
            HashSet<Character> charSet = new HashSet<>();
            for (int j = i; j < str.length(); j++) {
                if (charSet.contains(str.charAt(j))) {
                    break;
                }
                charSet.add(str.charAt(j));
                maxLength = Math.max(maxLength, j - i + 1);
            }
        }

        return maxLength;
    }

    public static void main(String[] args) {
        String str = "aabccbb";
        System.out.println("Length of the longest substring: " + findLength(str)); // Output: 3
    }
}
```

**Optimized Approach:**
```java
import java.util.HashMap;

public class LongestSubstringWithDistinctCharacters {
    public static int findLength(String str) {
        int windowStart = 0, maxLength = 0;
        HashMap<Character, Integer> charIndexMap = new HashMap<>();

        for (int windowEnd = 0; windowEnd < str.length(); windowEnd++) {
            char rightChar = str.charAt(windowEnd);
            if (charIndexMap.containsKey(rightChar)) {
                windowStart = Math.max(windowStart, charIndexMap.get(rightChar) + 1);
            }
            charIndexMap.put(rightChar, windowEnd);
            maxLength = Math.max(maxLength, windowEnd - windowStart + 1);
        }

        return maxLength;
    }

    public static void main(String[] args) {
        String str = "aabccbb";
        System.out.println("Length of the longest substring: " + findLength(str)); // Output: 3
    }
}
```

#### Problem 6: Longest Substring with Same Letters after Replacement

**Problem Statement:** Given a string with lowercase letters only, if you are allowed to replace no more than ‘k’ letters with any letter, find the length of the longest substring having the same letters after replacement.

**Brute Force Approach:**
```java
public class LongestSubstringWithSameLettersAfterReplacement {
    public static int characterReplacement(String str, int k) {
        int maxLength = 0;

        for (int i = 0; i < str.length(); i++) {
            int[] charCount = new int[26];
            int maxCount = 0, replacements = 0;
            for (int j = i; j < str.length(); j++) {
                charCount[str.charAt(j) - 'a']++;
                maxCount = Math.max(maxCount, charCount[str.charAt(j) - 'a']);
                replacements = (j - i + 1) - maxCount;

                if (replacements > k) {
                    break;
                }

                maxLength = Math.max(maxLength, j - i + 1);
            }
        }

        return maxLength;
    }

    public static void main(String[] args) {
        String str = "aabccbb";
        int k = 2;
        System.out.println("Length of the longest substring: " + characterReplacement(str, k)); // Output: 5
    }
}
```

**Optimized Approach:**
```java
import java.util.HashMap;

public class LongestSubstringWithSameLettersAfterReplacement {
    public static int characterReplacement(String str, int k) {
        int windowStart = 0, maxLength = 0, maxCount = 0;
        HashMap<Character, Integer> charFrequencyMap = new HashMap<>();

        for (int windowEnd = 0; windowEnd < str.length(); windowEnd++) {
            char rightChar = str.charAt(windowEnd);
            charFrequencyMap.put(rightChar, charFrequencyMap.getOrDefault(rightChar, 0) + 1);
            maxCount = Math.max(maxCount, charFrequencyMap.get(rightChar));

            if (windowEnd - windowStart + 1 - maxCount > k) {
                char leftChar = str.charAt(windowStart);
                charFrequencyMap.put(leftChar, charFrequencyMap.get(leftChar) - 1);
                windowStart++;
            }

            maxLength = Math.max(maxLength, windowEnd - windowStart + 1);
        }

        return maxLength;
    }

    public static void main(String[] args) {
        String str = "aabccbb";
        int k = 2;
        System.out.println("Length of the longest substring: " + characterReplacement(str, k)); // Output: 5
    }
}
```

#### Problem 7: Longest Subarray with Ones after Replacement

**Problem Statement:** Given a binary array, if you can replace no more than ‘k’ 0s with 1s, find the length of the longest contiguous subarray having all 1s.

**Brute Force Approach:**
```java
public class LongestSubarrayWithOnesAfterReplacement {
    public static int findLength(int[] arr, int k) {
        int maxLength = 0;

        for (int i = 0; i < arr.length; i++) {
            int zeroCount = 0;
            for (int j = i; j < arr.length; j++) {
                if (arr[j] == 0) {
                    zeroCount++;
                }
                if (zeroCount > k) {
                    break;
                }
                maxLength = Math.max(maxLength, j - i + 1);
            }
        }

        return maxLength;
    }

    public static void main(String[] args) {
        int[] arr = {0, 1, 1, 0, 0, 1, 1, 0, 1};
        int k = 2;
        System.out.println("Length of the longest subarray: " + findLength(arr, k)); // Output: 6
    }
}
```

**Optimized Approach:**
```java
public class LongestSubarrayWithOnesAfterReplacement {
    public static int findLength(int[] arr, int k) {
        int windowStart = 0, maxLength = 0, maxOnesCount = 0;

        for (int windowEnd = 0; windowEnd < arr.length; windowEnd++) {
            if (arr[windowEnd] == 1) {
                maxOnesCount++;
            }

            if (windowEnd - windowStart + 1 - maxOnesCount > k) {
                if (arr[windowStart] == 1) {
                    maxOnesCount--;
                }
                windowStart++;
            }

            maxLength = Math.max(maxLength, windowEnd - windowStart + 1);
        }

        return maxLength;
    }

    public static void main(String[] args) {
        int[] arr = {0, 1, 1, 0, 0, 1, 1, 0, 1};
        int k = 2;
        System.out.println("Length of the longest subarray: " + findLength(arr, k)); // Output: 6
    }
}
```

#### Problem 8: Permutation in a String

**Problem Statement:** Given a string and a pattern, find out if the string contains any permutation of the pattern.

**Brute Force Approach:**
```java
import java.util.HashMap;

public class PermutationInString {
    public static boolean findPermutation(String str, String pattern) {
        for (int i = 0; i <= str.length() - pattern.length(); i++) {
            if (checkPermutation(str.substring(i, i + pattern.length()), pattern)) {
                return true;
            }
        }
        return false;
    }

    private static boolean checkPermutation(String str, String pattern) {
        HashMap<Character, Integer> charFrequencyMap = new HashMap<>();
        for (char ch : pattern.toCharArray()) {
            charFrequencyMap.put(ch, charFrequencyMap.getOrDefault(ch, 0) + 1);
        }

        for (char ch : str.toCharArray()) {
            if (!charFrequencyMap.containsKey(ch)) {
                return false;
            }
            charFrequency

Map.put(ch, charFrequencyMap.get(ch) - 1);
            if (charFrequencyMap.get(ch) == 0) {
                charFrequencyMap.remove(ch);
            }
        }

        return charFrequencyMap.isEmpty();
    }

    public static void main(String[] args) {
        String str = "oidbcaf";
        String pattern = "abc";
        System.out.println("Permutation exist: " + findPermutation(str, pattern)); // Output: true
    }
}
```

**Optimized Approach:**
```java
import java.util.HashMap;

public class PermutationInString {
    public static boolean findPermutation(String str, String pattern) {
        HashMap<Character, Integer> charFrequencyMap = new HashMap<>();
        for (char ch : pattern.toCharArray()) {
            charFrequencyMap.put(ch, charFrequencyMap.getOrDefault(ch, 0) + 1);
        }

        int windowStart = 0, matched = 0;
        for (int windowEnd = 0; windowEnd < str.length(); windowEnd++) {
            char rightChar = str.charAt(windowEnd);
            if (charFrequencyMap.containsKey(rightChar)) {
                charFrequencyMap.put(rightChar, charFrequencyMap.get(rightChar) - 1);
                if (charFrequencyMap.get(rightChar) == 0) {
                    matched++;
                }
            }

            if (matched == charFrequencyMap.size()) {
                return true;
            }

            if (windowEnd >= pattern.length() - 1) {
                char leftChar = str.charAt(windowStart);
                windowStart++;
                if (charFrequencyMap.containsKey(leftChar)) {
                    if (charFrequencyMap.get(leftChar) == 0) {
                        matched--;
                    }
                    charFrequencyMap.put(leftChar, charFrequencyMap.get(leftChar) + 1);
                }
            }
        }

        return false;
    }

    public static void main(String[] args) {
        String str = "oidbcaf";
        String pattern = "abc";
        System.out.println("Permutation exist: " + findPermutation(str, pattern)); // Output: true
    }
}
```

#### Problem 9: String Anagrams

**Problem Statement:** Given a string and a pattern, find all anagrams of the pattern in the given string.

**Brute Force Approach:**
```java
import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;

public class StringAnagrams {
    public static List<Integer> findStringAnagrams(String str, String pattern) {
        List<Integer> resultIndices = new ArrayList<>();
        for (int i = 0; i <= str.length() - pattern.length(); i++) {
            if (checkPermutation(str.substring(i, i + pattern.length()), pattern)) {
                resultIndices.add(i);
            }
        }
        return resultIndices;
    }

    private static boolean checkPermutation(String str, String pattern) {
        HashMap<Character, Integer> charFrequencyMap = new HashMap<>();
        for (char ch : pattern.toCharArray()) {
            charFrequencyMap.put(ch, charFrequencyMap.getOrDefault(ch, 0) + 1);
        }

        for (char ch : str.toCharArray()) {
            if (!charFrequencyMap.containsKey(ch)) {
                return false;
            }
            charFrequencyMap.put(ch, charFrequencyMap.get(ch) - 1);
            if (charFrequencyMap.get(ch) == 0) {
                charFrequencyMap.remove(ch);
            }
        }

        return charFrequencyMap.isEmpty();
    }

    public static void main(String[] args) {
        String str = "ppqp";
        String pattern = "pq";
        System.out.println("Anagrams found at: " + findStringAnagrams(str, pattern)); // Output: [1, 2]
    }
}
```

**Optimized Approach:**
```java
import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;

public class StringAnagrams {
    public static List<Integer> findStringAnagrams(String str, String pattern) {
        List<Integer> resultIndices = new ArrayList<>();
        HashMap<Character, Integer> charFrequencyMap = new HashMap<>();
        for (char ch : pattern.toCharArray()) {
            charFrequencyMap.put(ch, charFrequencyMap.getOrDefault(ch, 0) + 1);
        }

        int windowStart = 0, matched = 0;
        for (int windowEnd = 0; windowEnd < str.length(); windowEnd++) {
            char rightChar = str.charAt(windowEnd);
            if (charFrequencyMap.containsKey(rightChar)) {
                charFrequencyMap.put(rightChar, charFrequencyMap.get(rightChar) - 1);
                if (charFrequencyMap.get(rightChar) == 0) {
                    matched++;
                }
            }

            if (matched == charFrequencyMap.size()) {
                resultIndices.add(windowEnd - pattern.length() + 1);
            }

            if (windowEnd >= pattern.length() - 1) {
                char leftChar = str.charAt(windowStart);
                windowStart++;
                if (charFrequencyMap.containsKey(leftChar)) {
                    if (charFrequencyMap.get(leftChar) == 0) {
                        matched--;
                    }
                    charFrequencyMap.put(leftChar, charFrequencyMap.get(leftChar) + 1);
                }
            }
        }

        return resultIndices;
    }

    public static void main(String[] args) {
        String str = "ppqp";
        String pattern = "pq";
        System.out.println("Anagrams found at: " + findStringAnagrams(str, pattern)); // Output: [1, 2]
    }
}
```

#### Problem 10: Longest Subarray with Ones after Replacement

**Problem Statement:** Given a binary array, if you can replace no more than ‘k’ 0s with 1s, find the length of the longest contiguous subarray having all 1s.

**Brute Force Approach:**
```java
public class LongestSubarrayWithOnesAfterReplacement {
    public static int findLength(int[] arr, int k) {
        int maxLength = 0;

        for (int i = 0; i < arr.length; i++) {
            int zeroCount = 0;
            for (int j = i; j < arr.length; j++) {
                if (arr[j] == 0) {
                    zeroCount++;
                }
                if (zeroCount > k) {
                    break;
                }
                maxLength = Math.max(maxLength, j - i + 1);
            }
        }

        return maxLength;
    }

    public static void main(String[] args) {
        int[] arr = {0, 1, 1, 0, 0, 1, 1, 0, 1};
        int k = 2;
        System.out.println("Length of the longest subarray: " + findLength(arr, k)); // Output: 6
    }
}
```

**Optimized Approach:**
```java
public class LongestSubarrayWithOnesAfterReplacement {
    public static int findLength(int[] arr, int k) {
        int windowStart = 0, maxLength = 0, maxOnesCount = 0;

        for (int windowEnd = 0; windowEnd < arr.length; windowEnd++) {
            if (arr[windowEnd] == 1) {
                maxOnesCount++;
            }

            if (windowEnd - windowStart + 1 - maxOnesCount > k) {
                if (arr[windowStart] == 1) {
                    maxOnesCount--;
                }
                windowStart++;
            }

            maxLength = Math.max(maxLength, windowEnd - windowStart + 1);
        }

        return maxLength;
    }

    public static void main(String[] args) {
        int[] arr = {0, 1, 1, 0, 0, 1, 1, 0, 1};
        int k = 2;
        System.out.println("Length of the longest subarray: " + findLength(arr, k)); // Output: 6
    }
}
```

#### Problem 11: Longest Substring with At Most K Distinct Characters

**Problem Statement:** Given a string, find the length of the longest substring in it with at most K distinct characters.

**Brute Force Approach:**
```java
import java.util.HashSet;

public class LongestSubstringWithKDistinct {
    public static int findLength(String str, int k) {
        int maxLength = 0;

        for (int i = 0; i < str.length(); i++) {
            HashSet<Character> distinctChars = new HashSet<>();
            for (int j = i; j < str.length(); j++) {
                distinctChars.add(str.charAt(j));
                if (distinctChars.size() > k) {
                    break;
                }
                maxLength = Math.max(maxLength, j - i + 1);
            }
        }

        return maxLength;
    }

    public static void main(String[] args) {
        String str = "araaci";
        int k = 2;
        System.out.println("Length of the longest substring: " + findLength(str, k)); // Output: 4
    }
}
```

**Optimized Approach:**
```java
import java.util.HashMap;

public class LongestSubstringWithKDistinct {
    public static int findLength(String str, int k) {
        int windowStart = 0, maxLength = 0;
        HashMap<Character, Integer> charFrequencyMap = new HashMap<>();

        for (int windowEnd = 0; windowEnd < str.length(); windowEnd++) {
            char rightChar = str.charAt

(windowEnd);
            charFrequencyMap.put(rightChar, charFrequencyMap.getOrDefault(rightChar, 0) + 1);

            while (charFrequencyMap.size() > k) {
                char leftChar = str.charAt(windowStart);
                charFrequencyMap.put(leftChar, charFrequencyMap.get(leftChar) - 1);
                if (charFrequencyMap.get(leftChar) == 0) {
                    charFrequencyMap.remove(leftChar);
                }
                windowStart++;
            }

            maxLength = Math.max(maxLength, windowEnd - windowStart + 1);
        }

        return maxLength;
    }

    public static void main(String[] args) {
        String str = "araaci";
        int k = 2;
        System.out.println("Length of the longest substring: " + findLength(str, k)); // Output: 4
    }
}
```
