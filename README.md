Coderpad Questions

1. Suppose we are given a string “aaabbbbbccccdaa”, then we to print “a3b5c4d1a2”. 
    Run length encoding: https://github.com/jhasuraj01/6companies30days/blob/main/goldman-sachs/run-length-encoding.md

```java
public class Solution {
    /**
     * Compresses the given string using run-length encoding
     * and returns the resulting compressed string.
     *
     * Example:
     *   Input:  "aaabbbbbccccdaa"
     *   Output: "a3b5c4d1a2"
     *
     * @param s the input string to compress
     * @return a new string containing the compressed representation
     */
    public String compressString(String s) {
        // Handle edge cases: null or empty input.
        if (s == null || s.length() == 0) {
            return "";
        }

        // StringBuilder is ideal for efficient appends.
        StringBuilder compressed = new StringBuilder();

        char[] chars = s.toCharArray();
        int charCount = 1;  // We'll start counting from the first character.

        // Traverse from the second character to the end.
        // Compare chars[i] with chars[i-1] to detect runs.
        for (int i = 1; i < chars.length; i++) {
            if (chars[i] == chars[i - 1]) {
                // Same character, increment run count
                charCount++;
            } else {
                // Different character encountered, write out the previous character and its count
                compressed.append(chars[i - 1]);
                if (charCount > 1) {
                    compressed.append(charCount);
                }
                // Reset count for the new character
                charCount = 1;
            }
        }

        // Append the final character and its count (if > 1) after loop finishes
        compressed.append(chars[chars.length - 1]);
        if (charCount > 1) {
            compressed.append(charCount);
        }

        // Return the compressed representation
        return compressed.toString();
    }

    // Example usage:
    public static void main(String[] args) {
        Solution sol = new Solution();
        String input = "aaabbbbbccccdaa";
        String result = sol.compressString(input);
        System.out.println("Compressed: " + result);  // Expected: "a3b5c4d1a2"
    }
}
```

2. Given an array in which there are arrays that are of length two, the first index of that array has the student name and the second index has the marks scored. Find the maximum average scored by any student. The array can have multiple subjects of marks for a particular student.

```java

🧠 Thought Process
We need to:
1. Track total scores and count of scores for each student.
2. Compute average per student.
3. Keep track of the maximum average seen.

Use a Map<String, int[]>:
- Key: Student name
- Value: int[2] → [0]: total score, [1]: count

At the end, iterate through the map and calculate average per student, storing the max.

✅ Constraints
1. Scores are integers.
2. You can assume valid input (no missing or malformed entries).
3. Student names are case-sensitive.


import java.util.*;

public class Solution {

    /**
     * Returns the maximum average score among all students.
     *
     * @param records A list of student name and score pairs.
     * @return Maximum average score as double.
     */
    public static double getMaxAverageScore(List<List<String>> records) {
        if (records == null || records.isEmpty()) {
            throw new IllegalArgumentException("Input records cannot be null or empty");
        }

        // Map to store: student -> [totalScore, count]
        Map<String, int[]> studentScores = new HashMap<>();

        for (List<String> record : records) {
            if (record.size() != 2) continue;

            String name = record.get(0);
            int score = Integer.parseInt(record.get(1));

            studentScores.computeIfAbsent(name, k -> new int[2]);
            studentScores.get(name)[0] += score; // total
            studentScores.get(name)[1] += 1;     // count
        }

        double maxAverage = Double.MIN_VALUE;

        for (Map.Entry<String, int[]> entry : studentScores.entrySet()) {
            int total = entry.getValue()[0];
            int count = entry.getValue()[1];
            double avg = (double) total / count;
            maxAverage = Math.max(maxAverage, avg);
        }

        return maxAverage;
    }

    public static void main(String[] args) {
        List<List<String>> input = Arrays.asList(
            Arrays.asList("Alice", "90"),
            Arrays.asList("Bob", "80"),
            Arrays.asList("Alice", "100"),
            Arrays.asList("Bob", "70"),
            Arrays.asList("Charlie", "95")
        );

        double result = getMaxAverageScore(input);
        System.out.println("Maximum average score: " + result); // Output: 95.0
    }
}

```

3. a/b + c/d = simple form

```java
import java.io.*;

class Solution {

    /**
     * Computes the Greatest Common Divisor (GCD) using Euclidean Algorithm.
     */
    static int gcd(int a, int b) {
        if (a == 0)
            return b;
        return gcd(b % a, a);
    }

    /**
     * Simplifies the given numerator and denominator by dividing both with their GCD,
     * and prints the result in the format "numerator/denominator".
     */
    static void lowerFraction(int numerator, int denominator) {
        if (denominator == 0) {
            throw new ArithmeticException("Denominator cannot be zero");
        }

        int gcdValue = gcd(Math.abs(numerator), Math.abs(denominator));

        numerator /= gcdValue;
        denominator /= gcdValue;

        // Normalize: keep denominator positive
        if (denominator < 0) {
            numerator *= -1;
            denominator *= -1;
        }

        System.out.println(numerator + "/" + denominator);
    }

    /**
     * Adds two fractions (numerator1/denominator1 + numerator2/denominator2),
     * and prints the result in its simplest form.
     */
    static void addFractions(int numerator1, int denominator1, int numerator2, int denominator2) {
        if (denominator1 == 0 || denominator2 == 0) {
            throw new ArithmeticException("Denominator cannot be zero");
        }

        int gcdDenominators = gcd(denominator1, denominator2);
        int lcmDenominator = (denominator1 / gcdDenominators) * denominator2;

        int adjustedNumerator1 = numerator1 * (lcmDenominator / denominator1);
        int adjustedNumerator2 = numerator2 * (lcmDenominator / denominator2);

        int resultNumerator = adjustedNumerator1 + adjustedNumerator2;

        lowerFraction(resultNumerator, lcmDenominator);
    }
}

```

4. grid [][] contain coins find optimal path to collect most coins (only allowed north and east)

5. Given a string ” A B A B C A B A B C D”, you have to compress it into the following format:
A B * C * D.
Here, till A B * of the output, A B repeats twice, but till A B * C *, A B A B C repeats twice. I could not solve this and went to next question.

6. Given a matrix of numbers, the task was to collect the maximum numbers possible when going from bottom left corner of the matrix to top right corner of the matrix. The only two directions you can move are up and right. I was able to solve this.

7. Given two fractions which are represented as an array of two elements (numerator and denominator), the task to find the reduced fraction which is the sum of two fractions.

8. Given two sorted arrays, find the median of them.
9.  Trapping rain water problem.
10. Find minimum sub array whose sum is at least to the given target. eg: Sub array: 1, 2, 5, 6, 11, 2 Target: 12 Answer: 11, 2

11.  https://leetcode.com/problems/trapping-rain-water/ (Trapping the rainwater).
<img width="1571" alt="image" src="https://github.com/user-attachments/assets/a4a02a12-3ef7-40ab-ab4d-51bbad9b8464" /># goldmansachs
```java
class Solution {
    public int trap(int[] height) {

        // an elevation map — basically an array of bars of varying height, with equal width 1.

        // Water can be trapped between bars, but only if there is a taller or equally tall bar on both the left and
        //right of the current bar.

        // The amount of water trapped above a bar is determined by the minimum of the highest bar to its left and
        //right, minus the height of the current bar.

        int trapped_rain = 0;
        int[] maxLeft = new int [height.length];
        int[] maxRight = new int [height.length];

        maxLeft[0] = height[0];
        maxRight[height.length - 1] = height[height.length - 1];

        for (int i = 1; i < height.length; i++) {
            maxLeft[i] = Math.max(height[i], maxLeft[i - 1]);
        }

        for (int i = height.length - 2; i >= 0; i--) {
            maxRight[i] = Math.max(height[i], maxRight[i + 1]);
        }

        for (int i = 0; i < height.length; i++){
            trapped_rain += Math.max(0, Math.min(maxLeft[i], maxRight[i]) - height[i]);
        }

        return trapped_rain;
        
    }
}
```
13. A variant of Knapsack (0-1 )- https://www.geeksforgeeks.org/0-1-knapsack-problem-dp-10/ (She wanted the DP solution only. She was not accepting the recursive solution)

14. There are some students, sitting in a circle. Each student is assigned a roll no (1 to n). There is a teacher who was given an initial roll no and he has to remove the student with initial roll no. and then has to remove the student who was at that position starting from the removed student. Your function should return the last student left.
Example: 2, 3, 1, 4, 5 start with 3, remove 3, then remove 5, then remove 1 (follow circle), then remove 4. Ans – 2. Problem is similar to https://www.geeksforgeeks.org/josephus-problem-set-1-a-on-solution/

15. Given a list of students with their scores in different subjects find the student with a max average score.

16. Median of two sorted arrays (without extra space). Link – https://www.geeksforgeeks.org/median-two-sorted-arrays-different-sizes-ologminn-m/
17. Given a log file, each line begins with some IP address, find the most frequent IP address.

18. https://www.geeksforgeeks.org/find-minimum-element-in-a-sorted-and-rotated-array/
19. https://www.geeksforgeeks.org/median-two-sorted-arrays-different-sizes-ologminn-m/ O(n) accepted
20. https://leetcode.com/problems/string-compression/
21. https://www.geeksforgeeks.org/container-with-most-water/
22. Repeated Character Whose First Appearance is Leftmost
23. Find the first maximum length even word from a string
24. Write a function that takes input and output as shown under:-
    Input (string) Output (string)
    —– ——
    aaa a3
    aabbcc a2b2c2
    aaabcdd a3b1c1d2
    a a1

25. a) You are an avid rock collector who lives in southern California. Some rare and desirable rocks just became available in New York, so you are planning a cross-country road trip. There are several other rare rocks that you could pick up along the way. You have been given a grid filled with numbers, representing the number of rare rocks available in various cities across the country. Your objective is to find the optimal path from So_Cal to New_York that would allow you to accumulate the most rocks along the way.

Note: You can only travel either north (up) or east (right).
b) Consider adding some additional tests in doTestsPass().
c) Implement optimalPath() correctly.
d) Here is an example:
^
{{0, 0, 0, 0, 5}, New_York (finish) N
{0, 1, 1, 1, 0},
So_Cal (start) {2, 0, 0, 0, 0}} S
v
The total for this example would be 10 (2+0+1+1+1+0+5).

25. Given a string. Write a function to find the first non-repeating character in it. If there is no non-repeating character, return 0;
e.g.
Input (string) Output (char)
—– ——
aabbccd d
abbccddee a
iijjkkllmm 0

26. Implement a function that takes two unsigned integers as arguments namely numerator & denominator and outputs a string representing the fraction in decimal form. If there is a repeating and non-terminating digit that appears in the decimal form, write that in parenthesis.
e.g.
Input (UINT n, UINT d) Output (string)
—– ——
2, 5 0.4
1, 2 0.5
1, 3 0.(3)
12, 5 2.4
11, 20 0.55
5, 3 1.(6)

27. Repeating first index. "rahulgorai" - return r

28. Largest tree in a forest

```java

/* Problem Name is &&& Largest Tree &&& PLEASE DO NOT REMOVE THIS LINE. */
/*
 **  Instructions:
 **
 **  Given a forest ( one or more disconnected trees ), find the root of largest tree
 **  and return its Id. If there are multiple such roots, return the smallest Id of them.
 **
 **  Complete the largestTree function in the editor below.
 **  It has one parameter, immediateParent, which is a map containing key-value pair indicating
 **  child -> parent relationship. The key is child and value is the corresponding
 **  immediate parent.
 **  Constraints
 **    - Child cannot have more than one immediate parent.
 **    - Parent can have more than one immediate child.
 **    - The given key-value pair forms a well-formed forest ( a tree of n nodes will have n-1 edges )
 **
 **  Example:
 **
 **  Input:
 **  { { 1 -> 2 }, { 3 -> 4 } }
 **
 **  Expected output: 2
 **  Explanation: There are two trees one having root of Id 2 and another having root of Id 4.
 **  Both trees have size 2. The smaller number of 2 and 4 is 2. Hence the answer is 2.
 */

import java.util.*;

class Solution {
  /*
   **  Find the largest tree.
   */
  public static int largestTree(final Map<Integer, Integer> immediateParent) {
    return 0;
  }

  /*
   **  Returns true if the tests pass. Otherwise, returns false;
   */
  public static boolean testsPass() {
    // map of test cases to expected results
    final Map<Map<Integer, Integer>, Integer> testCases = new HashMap<>();

    // example
    final Map<Integer, Integer> testCaseOneKey = new HashMap<>() {{
      put(1, 2);
      put(3, 4);
    }};
    testCases.put(testCaseOneKey, 2);


    boolean passed = true;
    for (var entry : testCases.entrySet()) {
      final int actual = largestTree(entry.getKey());
      if (actual != entry.getValue()) {
        passed = false;
        System.out.printf("Failed for %s%n expected %s, actual %s%n", entry.getKey(), entry.getValue(), actual);
      }
    }

    return passed;
  }

  /*
   **  Execution entry point.
   */
  public static void main(String[] args) {
    // Run the tests
    if (testsPass()) {
      System.out.println("All tests pass");
    }
    else {
      System.out.println("Tests fail.");
    }
  }
}
```

Solutions

```java

/* Problem Name is &&& Largest Tree &&& PLEASE DO NOT REMOVE THIS LINE. */
/*
 **  Instructions:
 **
 **  Given a forest ( one or more disconnected trees ), find the root of largest tree
 **  and return its Id. If there are multiple such roots, return the smallest Id of them.
 **
 **  Complete the largestTree function in the editor below.
 **  It has one parameter, immediateParent, which is a map containing key-value pair indicating
 **  child -> parent relationship. The key is child and value is the corresponding
 **  immediate parent.
 **  Constraints
 **    - Child cannot have more than one immediate parent.
 **    - Parent can have more than one immediate child.
 **    - The given key-value pair forms a well-formed forest ( a tree of n nodes will have n-1 edges )
 **
 **  Example:
 **
 **  Input:
 **  { { 1 -> 2 }, { 3 -> 4 } }
 **
 **  Expected output: 2
 **  Explanation: There are two trees one having root of Id 2 and another having root of Id 4.
 **  Both trees have size 2. The smaller number of 2 and 4 is 2. Hence the answer is 2.
 */

import java.util.*;

class Solution {
  /*
   **  Find the largest tree.
   */
  public static int largestTree(final Map<Integer, Integer> immediateParent) {
    // 1. Build an adjacency map of parent -> list of children
    Map<Integer, List<Integer>> adjacencyMap = new HashMap<>();
    // Also track all unique nodes
    Set<Integer> allNodes = new HashSet<>();

    for (Map.Entry<Integer, Integer> entry : immediateParent.entrySet()) {
      int child = entry.getKey();
      int parent = entry.getValue();
      adjacencyMap
        .computeIfAbsent(parent, k -> new ArrayList<>())
        .add(child);

      allNodes.add(child);
      allNodes.add(parent);
    }

    // 2. Identify all roots: any node that isn't a child in the immediateParent map
    //    (i.e., it doesn't appear as a key in immediateParent)
    Set<Integer> childNodes = immediateParent.keySet();
    List<Integer> roots = new ArrayList<>();
    for (int node : allNodes) {
      if (!childNodes.contains(node)) {
        roots.add(node);
      }
    }

    // 3. For each root, do a DFS or BFS to count tree size.
    //    Track the largest size found so far.
    int maxSize = 0;
    int smallestRootForMaxSize = Integer.MAX_VALUE;

    for (int root : roots) {
      int size = getTreeSize(root, adjacencyMap);
      if (size > maxSize) {
        maxSize = size;
        smallestRootForMaxSize = root;
      } else if (size == maxSize && root < smallestRootForMaxSize) {
        smallestRootForMaxSize = root;
      }
    }

    // 4. Return the smallest ID root among the largest trees
    return smallestRootForMaxSize;
  }

  // Helper method to get size of tree starting from a given root
  private static int getTreeSize(int root, Map<Integer, List<Integer>> adjacencyMap) {
    Deque<Integer> stack = new ArrayDeque<>();
    Set<Integer> visited = new HashSet<>();
    stack.push(root);
    visited.add(root);

    int size = 0;
    while (!stack.isEmpty()) {
      int node = stack.pop();
      size++;
      for (int child : adjacencyMap.getOrDefault(node, Collections.emptyList())) {
        if (!visited.contains(child)) {
          visited.add(child);
          stack.push(child);
        }
      }
    }
    return size;
  }

  /*
   **  Returns true if the tests pass. Otherwise, returns false;
   */
  public static boolean testsPass() {
    // map of test cases to expected results
    final Map<Map<Integer, Integer>, Integer> testCases = new HashMap<>();

    // example
    final Map<Integer, Integer> testCaseOneKey = new HashMap<>() {{
      put(1, 2);
      put(3, 4);
    }};
    testCases.put(testCaseOneKey, 2);

    boolean passed = true;
    for (var entry : testCases.entrySet()) {
      final int actual = largestTree(entry.getKey());
      if (actual != entry.getValue()) {
        passed = false;
        System.out.printf("Failed for %s%n expected %s, actual %s%n",
                          entry.getKey(), entry.getValue(), actual);
      }
    }

    return passed;
  }

  /*
   **  Execution entry point.
   */
  public static void main(String[] args) {
    // Run the tests
    if (testsPass()) {
      System.out.println("All tests pass");
    }
    else {
      System.out.println("Tests fail.");
    }
  }
}

```


29. Top K Frequent Elements
    <img width="558" alt="image" src="https://github.com/user-attachments/assets/51f4acf4-f8cf-4b04-b8fb-cd02b6a8b99f" />

    ```java
    /**
     * Returns the k most frequent elements from the given array of integers.
     *
     * Detailed Explanation of the Steps:
     *   1) Create a frequency map freqMap:
     *      - Key: Integer (the element)
     *      - Value: Integer (the frequency of that element)
     *      This lets us track how many times each integer appears in O(n) time.
     *
     *   2) Build an array of lists (buckets), where index i in 'buckets' corresponds 
     *      to a list of elements that appear i times.
     *      - The size of this array is nums.length + 1, because the highest possible
     *        frequency of any element is n (if all elements are identical).
     *
     *   3) For each unique number in freqMap, append it to the corresponding bucket 
     *      based on its frequency.
     *
     *   4) Traverse the buckets from the highest frequency index down to the lowest,
     *      and collect elements until we've collected k of them. 
     *
     * Time Complexity:
     *   - Step 1 (frequency map creation): O(n).
     *   - Step 2 (bucket array creation) : O(n).
     *   - Step 3 (filling buckets)       : O(n) in worst case (each element is unique).
     *   - Step 4 (collecting top k)      : O(n) in worst case.
     *   => Overall O(n).
     *
     * Space Complexity:
     *   - freqMap can have up to n entries in the worst case => O(n).
     *   - buckets array is size n+1, with each index a list => O(n).
     *   => Overall O(n).
     *
     * @param nums array of integers
     * @param k number of most frequent elements to retrieve
     * @return an array of the k most frequent integers in any order
     */
    public int[] topKFrequent(int[] nums, int k) {
       // 1) Build the frequency map
        Map<Integer, Integer> freqMap = new HashMap<>();
        for (int num : nums) {
            freqMap.put(num, freqMap.getOrDefault(num, 0) + 1);
        }
        
        // 2) Create the buckets: an array of lists, where index = frequency
        // Maximum frequency can be n, hence length = n + 1
        List<Integer>[] buckets = new List[nums.length + 1];
        for (int i = 0; i < buckets.length; i++) {
            buckets[i] = new ArrayList<>();
        }
        
        // 3) Fill the buckets based on frequency
        // For each unique element, place it into the list corresponding to its frequency
        for (Map.Entry<Integer, Integer> entry : freqMap.entrySet()) {
            int number = entry.getKey();
            int frequency = entry.getValue();
            buckets[frequency].add(number);
        }
        
        // 4) Collect the top k frequent elements
        List<Integer> result = new ArrayList<>(k);
        
        // Start from the highest frequency and go down
        for (int freq = buckets.length - 1; freq > 0 && result.size() < k; freq--) {
            for (int val : buckets[freq]) {
                result.add(val);
                if (result.size() == k) {
                    break;
                }
            }
        }
        
        // Convert the result list to an array
        int[] output = new int[result.size()];
        for (int i = 0; i < result.size(); i++) {
            output[i] = result.get(i);
        }
        return output;
    }
    ```

30. Fraction to recurring decimal
<img width="558" alt="image" src="https://github.com/user-attachments/assets/b946edc1-f230-4a6e-b2e9-09bdee044872" />


```java

/**
 * LeetCode Problem #166: Fraction to Recurring Decimal
 *
 * We want to convert a fraction (numerator/denominator) into its decimal string representation.
 * If the fractional part repeats, we must enclose the repeating sequence in parentheses.
 *
 * Below is a production-ready, thoroughly commented solution that:
 *  1) Handles signs correctly (including 0).
 *  2) Uses a Map<Long, Integer> to track the position of each remainder in the result,
 *     so we know where the repeating portion begins.
 *  3) Efficiently builds the output using StringBuilder.
 *  4) Avoids integer overflow by working with long values for divisions.
 *
 * Short and Simple Thought Process in Comments:
 *  1) Capture sign.
 *  2) Handle integer division part.
 *  3) Move to remainder and store each remainder in a map alongside the current length of the result.
 *     If we see the remainder again, that means the digits between the first occurrence and now are repeating.
 *  4) Insert parentheses accordingly and return.
 *
 * Choice of Data Structures:
 *  - StringBuilder for efficient string concatenations.
 *  - HashMap<Long, Integer> to map each remainder to the position in the resulting decimal string,
 *    enabling O(1) lookups to detect the start of the repeating cycle.
 *
 * Performance Complexity:
 *  - The main loop processes each new remainder at most once until it repeats or becomes zero,
 *    making the time complexity O(k), where k is the number of digits before repetition or termination.
 *  - Space complexity is also O(k) due to storing each unique remainder at most once in the map.
 *
 * This solution is designed for clarity, correctness, and speed.
 * It should run in constant space relative to the size of the integer range constraints,
 * but variable space/time in proportion to the output decimal length.
 */

class Solution {
    public String fractionToDecimal(int numerator, int denominator) {
        // Handle the zero numerator edge case up front: 0 / any_non_zero => "0"
        if (numerator == 0) return "0";

        // Use StringBuilder for efficient construction of the result.
        StringBuilder result = new StringBuilder();

        // ----------------------------
        // 1. Determine the sign
        // ----------------------------
        // We use XOR trick: if exactly one is negative => negative result
        boolean isNegative = (numerator < 0) ^ (denominator < 0);
        if (isNegative) {
            result.append("-");
        }

        // Convert both numerator and denominator to longs to avoid overflow.
        long longNumerator = Math.abs((long) numerator);
        long longDenominator = Math.abs((long) denominator);

        // ----------------------------
        // 2. Append the integer part
        // ----------------------------
        long integerPart = longNumerator / longDenominator;
        result.append(integerPart);

        // Calculate remainder
        long remainder = longNumerator % longDenominator;
        // If no remainder => perfect division => return current result
        if (remainder == 0) {
            return result.toString();
        }

        // ----------------------------
        // 3. Append decimal point
        // ----------------------------
        result.append(".");

        // ----------------------------
        // 4. Handle fractional part with repeating detection
        // ----------------------------
        // remainderIndexMap will store remainder -> the position in the result
        // where the corresponding digit was inserted
        // Key: remainder, Value: index in the StringBuilder
        Map<Long, Integer> remainderIndexMap = new HashMap<>();
        
        // Current index in the string builder is used to know where the next digit will appear
        // We'll insert the repeating parentheses once we see the same remainder again
        // remainderIndexMap helps detect and locate the repeating sequence
        while (remainder != 0) {
            // If we have already seen this remainder, that means the decimals are repeating
            if (remainderIndexMap.containsKey(remainder)) {
                // Insert "(" at the index where we first saw this remainder
                int startRepeatIndex = remainderIndexMap.get(remainder);
                result.insert(startRepeatIndex, "(");
                // Append ")" at the current end of the string
                result.append(")");
                break;
            }

            // Store the current remainder with its corresponding index in the result
            remainderIndexMap.put(remainder, result.length());

            // "Bring down a 0" => multiply remainder by 10
            remainder *= 10;

            // Next digit in the fractional portion
            long nextDigit = remainder / longDenominator;
            // Append it
            result.append(nextDigit);

            // Update remainder
            remainder %= longDenominator;
        }

        return result.toString();
    }
}
```


31. Find the number of unique substrings of size k, in a given string
    
```java
/**
 * Find the number of unique substrings of size k in a given string.
 *
 * Short & Simple Thought Process in Comments:
 * 1) Edge Cases: if k <= 0 or k > s.length(), no valid substring => return 0.
 * 2) Use Rolling Hash (Rabin-Karp style) to slide over the string in O(n) time.
 * 3) Maintain a HashSet<Long> of encountered hash values for distinct k-substrings.
 * 4) Return the size of the HashSet as the count of unique substrings.
 *
 * Choice of Data Structures:
 *  - We use a HashSet<Long> to quickly check for uniqueness in O(1) average time.
 *  - A rolling hash approach ensures we can compute each k-length substring hash in O(1)
 *    after the initial substring, drastically reducing computational overhead.
 *
 * Complexity:
 *  - Time: O(n) for a string of length n, because each step in the sliding window 
 *    updates the hash in constant time, and set operations are O(1) average.
 *  - Space: O(n) in the worst case if all k-length substrings are distinct.
 *
 * Below is a production-ready implementation with thorough explanatory comments.
 * We also include an example main method demonstration.
 */
import java.util.*;

public class SolutionUniqueSubstrings {
    
    // ---------------------------
    // Main API method
    // ---------------------------
    public int countUniqueSubstringsOfSizeK(String s, int k) {
        // 1) Edge case checks
        if (s == null || k <= 0 || k > s.length()) {
            return 0;
        }
        
        // 2) Rolling Hash parameters:
        //    We'll use base and modulus to reduce collisions
        //    base ~ some prime near the character set size, modulus ~ large prime
        //    to reduce collisions; for demonstration, we pick fairly large values.
        long base = 257L; // prime > typical ASCII or extended char set
        long mod = 1000000007L; // large prime for modulus
        
        // 3) Precompute the highest power of base^(k-1) mod for removing leading digit
        long highestPower = 1;
        for (int i = 0; i < k - 1; i++) {
            highestPower = (highestPower * base) % mod;
        }
        
        HashSet<Long> seen = new HashSet<>();   // tracks all distinct hash values
        long currentHash = 0;
        
        // 4) Build the hash of the first k-length substring
        for (int i = 0; i < k; i++) {
            // Convert character to int
            int charVal = s.charAt(i);
            currentHash = (currentHash * base + charVal) % mod;
        }
        // Add it to our set
        seen.add(currentHash);
        
        // 5) Now slide over the string from index k to end
        for (int i = k; i < s.length(); i++) {
            int newCharVal = s.charAt(i);
            int oldCharVal = s.charAt(i - k);
            
            // Remove the leading character's contribution, then add new char
            // a) Subtract leading char's 'weight' -> oldCharVal * highestPower
            long toRemove = (oldCharVal * highestPower) % mod;
            currentHash = (currentHash - toRemove) % mod;
            // in Java, negative mod fix
            if (currentHash < 0) {
                currentHash += mod;
            }
            
            // b) Multiply by base and add new char
            currentHash = (currentHash * base + newCharVal) % mod;
            
            // 6) Store the new hash
            seen.add(currentHash);
        }
        
        // The count of unique k-length substrings is simply the size of the set
        return seen.size();
    }
    
    // ---------------------------
    // Demonstration / Testing
    // ---------------------------
    public static void main(String[] args) {
        SolutionUniqueSubstrings solver = new SolutionUniqueSubstrings();

        // Example 1:
        String s1 = "abcabcbb";
        int k1 = 3;
        // Substrings of length 3:
        //  "abc", "bca", "cab", "abc", "bcb", "cbb"
        //  Distinct => "abc", "bca", "cab", "bcb", "cbb" => 5
        int result1 = solver.countUniqueSubstringsOfSizeK(s1, k1);
        System.out.println("Unique substrings of size " + k1 + " in \"" + s1 + "\": " + result1); 
        // Expected output => 5

        // Example 2:
        String s2 = "aaaaa";
        int k2 = 2;
        // Substrings of length 2: "aa", "aa", "aa", "aa"
        // Distinct => just "aa"
        int result2 = solver.countUniqueSubstringsOfSizeK(s2, k2);
        System.out.println("Unique substrings of size " + k2 + " in \"" + s2 + "\": " + result2); 
        // Expected output => 1

        // Example 3:
        String s3 = "abcdef";
        int k3 = 2;
        // Substrings of length 2: "ab", "bc", "cd", "de", "ef" => all distinct => 5
        int result3 = solver.countUniqueSubstringsOfSizeK(s3, k3);
        System.out.println("Unique substrings of size " + k3 + " in \"" + s3 + "\": " + result3);
        // Expected => 5
    }
}

```

32. Problem: 
"""
A popular online retailer allows vendors to specify different prices in advance
for the same item throughout the day. We now need to design an algorithm that
helps identify the lowest price for the item at any point of the day.

Assumptions:

For the algorithm, assume all vendors are selling the same product
and there is only one product being sold. Given a list that has
vendor information - ( startTime, endTime, price ) of the deal,
return a sorted list with different possible intervals and
the least price of the product during the interval.

The interval is inclusive of start and end time.

All the 3 values passed by the vendor are integers.

sampleInput = { new Interval( 1, 5, 20 ), new Interval( 3, 8, 15 ), new Interval( 7, 10, 8 ) };
expectedOutput = { new Interval( 1, 2, 20 ), new Interval( 3, 6, 15 ), new Interval( 7, 10, 8 ) };

"""

Solution:

```java

/*
 * ****************************************************************************
 *  A Popular Online Retailer:
 *  -------------------------
 *  Vendors can each specify a startTime, endTime, and price for a product deal,
 *  covering an inclusive time interval [startTime..endTime].
 *  Multiple vendors can overlap in time, each with their own price.
 *  
 *  We want to combine these possibly-overlapping intervals to produce a list
 *  of sorted, non-overlapping intervals in which each interval reflects the
 *  cheapest (lowest) price available during that particular timespan.
 *  
 *  Example:
 *    Input intervals: 
 *      (1, 5, 20),  (3, 8, 15),  (7, 10, 8)
 *    Sorted by startTime, they overlap in the ranges [3..5] and [7..8].
 *    We must figure out the minimum price in each overlapping segment.
 *    The final intervals become:
 *      [1..2]  -> only one interval (1..5, 20) active => price = 20
 *      [3..6]  -> intervals (1..5, 20) & (3..8, 15) overlap => min price = 15
 *      [7..10] -> intervals (3..8, 15) & (7..10, 8) overlap => min price = 8
 *  
 *  --> Expected Output: (1, 2, 20), (3, 6, 15), (7, 10, 8)
 *
 * ****************************************************************************
 *
 *  Implementation Approach:
 *  ------------------------
 *  1) We represent each vendor interval [startTime, endTime, price] in a 
 *     small internal structure "Interval".
 *
 *  2) We convert each input interval into two "events":
 *       - (time = startTime,  price, delta = +1) to indicate the price 
 *         is becoming active at time = startTime.
 *       - (time = endTime+1,  price, delta = -1) to indicate the price 
 *         stops being active immediately after endTime (because it's inclusive).
 *     This technique is commonly known as a "Line Sweep" or "Sweep Line" approach.
 *
 *  3) Sort all these events by:
 *       (a) ascending time
 *       (b) if tie on time, then process +1 (start) before -1 (end)
 *
 *  4) Maintain a min-heap (PriorityQueue<Integer>) holding all currently active prices, 
 *     coupled with a HashMap<Integer, Integer> to track how many times a given price 
 *     is active. (Because multiple intervals can share the same price.)
 *
 *     - When we see an event with delta = +1, we add/increment that price in the map,
 *       and if it’s newly encountered, we push it into the min-heap.
 *     - When we see an event with delta = -1, we decrement that price’s active count 
 *       in the map. If the count goes to zero, we’ll lazily remove it from the min-heap 
 *       at some point (i.e., we keep popping from the heap until the top’s count 
 *       is nonzero).
 *
 *  5) The top of the min-heap at any time gives us the currently cheapest price 
 *     among those intervals that are active. As we move from one event’s time to the next, 
 *     if the cheapest price changes, we conclude the previous interval ended, 
 *     and we begin a new interval with the updated minimum price.
 *
 *  6) We store these finalized intervals in a result list, carefully handling boundaries:
 *     - The "start" time for a cheapest-price interval is the first moment 
 *       that price became the cheapest.
 *     - The "end" time is the moment right before the price changes or no interval 
 *       remains active.
 *
 *  7) Output:
 *     The final result is a sorted list of intervals with distinct time segments 
 *     and the lowest price in each segment.
 *
 * ****************************************************************************
 *
 *  Complexity:
 *  -----------
 *  - Let N = number of input intervals.
 *  - We produce up to 2N events, then sort them in O(N log N) time.
 *  - We iterate over these events once (O(N)), each event insertion/removal 
 *    from a min-heap is O(log N) worst-case, so total O(N log N).
 *  - The space complexity is O(N) for storing events, the heap, and the map.
 *
 * ****************************************************************************
 *
 *  Implementation Details for a Single CoderPad-Class "Solution":
 *  --------------------------------------------------------------
 *  - We'll define a nested static class Interval within this single "Solution" class.
 *  - We'll implement a main(...) method at the bottom that demonstrates usage 
 *    by creating sample input and printing the result.
 *  - We'll provide a helper method "getLowestPriceIntervals(Interval[] input)" 
 *    which performs the line-sweep approach and returns a List<Interval>.
 *
 * ****************************************************************************
 *
 *  Short & Simple Thought Process:
 *    - Convert intervals to "start" & "end+1" events
 *    - Sort events
 *    - Sweep across times:
 *       + Add new intervals' prices in min-heap
 *       + Remove intervals' prices that ended 
 *       + Track changes of the top of the heap to finalize intervals
 *
 * ****************************************************************************
 *
 *  Explanation of Data Structures:
 *  -------------------------------
 *  1) PriorityQueue<Integer> (Min-Heap): 
 *     - We store all active prices so that we can efficiently query 
 *       the smallest price in O(1) time (top of the heap).
 *     - Each insertion/removal is O(log N).
 *
 *  2) HashMap<Integer, Integer> activeCount:
 *     - Maps a price -> the count of how many intervals are currently contributing 
 *       that price (due to overlapping intervals).
 *     - This is crucial for "lazy" deletion. We only pop from the heap while 
 *       the top price has a count of zero in the map (meaning it’s no longer active).
 *
 * ****************************************************************************
 */

import java.util.*;

public class Solution {
    
    /**
     * A minimal data structure to store intervals: [startTime, endTime, price].
     * We'll keep it as a static nested class inside "Solution" to satisfy
     * the single-class requirement in this coderpad-style file.
     */
    static class Interval {
        public int start;
        public int end;
        public int price;
        
        public Interval(int s, int e, int p) {
            this.start = s;
            this.end = e;
            this.price = p;
        }
        
        @Override
        public String toString() {
            // Just for easy printing
            return "(" + start + ", " + end + ", " + price + ")";
        }
    }
    
    /**
     * Main public method that performs the line-sweep approach:
     * Takes an array of intervals describing (startTime, endTime, price).
     * Returns a sorted list of intervals with the minimal price in each segment.
     */
    public List<Interval> getLowestPriceIntervals(Interval[] intervals) {
        // Edge case: no input intervals => empty result
        if (intervals == null || intervals.length == 0) {
            return new ArrayList<>();
        }
        
        // Step 1: Build the "events" array. 
        //   We'll store them as int[]: { time, price, delta }
        //   where delta = +1 => start, delta = -1 => end
        //   "end" in the sense that the price ends right after 'endTime' => event at (endTime + 1).
        List<int[]> events = new ArrayList<>();
        
        for (Interval iv : intervals) {
            // Start event
            events.add(new int[] { iv.start, iv.price, 1 });
            // End event (immediately after iv.end => iv.end + 1) 
            // be careful if iv.end == Integer.MAX_VALUE, but we'll ignore that corner here
            events.add(new int[] { iv.end + 1, iv.price, -1 });
        }
        
        // Step 2: Sort events primarily by ascending time,
        //         then break ties by delta in descending order (start before end).
        // This ensures if two events share the same time, we process start events (+1) 
        // before end events (-1).
        events.sort((a, b) -> {
            if (a[0] != b[0]) return Integer.compare(a[0], b[0]);  // compare time
            return Integer.compare(b[2], a[2]); // for tie on time, +1 should come first
        });
        
        // Step 3: We'll keep a PriorityQueue to track all active prices (min-heap)
        //         and a HashMap<price, count> for how many intervals are currently active with that price.
        PriorityQueue<Integer> minHeap = new PriorityQueue<>();
        Map<Integer, Integer> activeCount = new HashMap<>();
        
        // We'll store the final intervals in a list
        List<Interval> result = new ArrayList<>();
        
        // We track the current minimum price and the time that min price started
        int currentMinPrice = -1;
        int currentMinStart = -1;
        
        // We'll also track the "previous time" so we know the range covered by the last stable price
        int prevTime = events.get(0)[0]; 
        
        // A small helper to "clean" the top of the heap from stale entries 
        // (i.e., those with count=0 in the activeCount).
        Runnable cleanTop = () -> {
            while (!minHeap.isEmpty() && activeCount.getOrDefault(minHeap.peek(), 0) == 0) {
                minHeap.poll();
            }
        };
        
        // Step 4: Process the events in ascending time order
        int i = 0;
        while (i < events.size()) {
            int curTime = events.get(i)[0];
            
            // If we've moved forward in time from prevTime => finalize intervals from [prevTime..curTime-1].
            if (curTime != prevTime) {
                cleanTop.run(); // ensure the heap top is valid
                // The valid minimum is the top of the heap, or -1 if empty
                int validMin = minHeap.isEmpty() ? -1 : minHeap.peek();
                
                // If we had a "currentMinPrice" that was valid, it covered [start..(curTime-1)]
                if (currentMinPrice != -1) {
                    // We finalize the old interval if the old min hasn't changed
                    // If the min changed earlier, we would have closed it. 
                    // But let's be consistent with the logic:
                    if (currentMinPrice == validMin) {
                        // The min remained the same across [prevTime..(curTime-1)]
                        // Do nothing special, we continue
                    } else {
                        // The min must have changed or become -1 
                        // => we close out the old interval at (prevTime - 1)? 
                        // Actually, we do it at (curTime - 1) because we didn't finalize it earlier
                        result.add(new Interval(currentMinStart, curTime - 1, currentMinPrice));
                        currentMinPrice = -1; 
                    }
                }
                // If we never had a valid min or if we ended it, we do nothing here.

                // Next we check if there's a valid new min for [curTime..∞) 
                // and update currentMinPrice if needed.
                if (validMin != -1 && currentMinPrice == -1) {
                    currentMinPrice = validMin;
                    currentMinStart = curTime;
                }
                
                // Move forward in time
                prevTime = curTime;
            }
            
            // Now handle all events at the current time
            int timeGroup = curTime;
            while (i < events.size() && events.get(i)[0] == timeGroup) {
                int[] ev = events.get(i);
                int eventTime = ev[0];
                int price = ev[1];
                int delta = ev[2];
                
                if (delta == 1) { // start event
                    activeCount.put(price, activeCount.getOrDefault(price, 0) + 1);
                    // If it's newly added (count=1), push into minHeap
                    if (activeCount.get(price) == 1) {
                        minHeap.offer(price);
                    }
                } else { // delta == -1 => end event
                    // decrement the count
                    if (activeCount.containsKey(price)) {
                        activeCount.put(price, activeCount.get(price) - 1);
                    }
                }
                i++;
            }
            
            // After processing all events at this time, see if the min changed
            cleanTop.run();
            int newMin = minHeap.isEmpty() ? -1 : minHeap.peek();
            
            if (newMin != currentMinPrice) {
                // If we had an old min, close it out at (curTime-1)
                if (currentMinPrice != -1) {
                    result.add(new Interval(currentMinStart, curTime - 1, currentMinPrice));
                }
                // Start a new interval if newMin != -1
                currentMinPrice = newMin;
                if (currentMinPrice != -1) {
                    currentMinStart = curTime;
                }
            }
        }
        
        // The process above closes intervals each time the min changes or time jumps. 
        // Because we only do that from "prevTime..(curTime-1)", 
        // we do not have any leftover interval beyond the last event, 
        // as there's no more active coverage after the final end events.
        
        return result;
    }
    
    // -----------------------------------------------------------------------
    //  MAIN: A simple demonstration of usage in a single coderpad class.
    // -----------------------------------------------------------------------
    public static void main(String[] args) {
        Solution solution = new Solution();
        
        // Sample Input
        Interval[] sampleInput = {
            new Interval(1, 5, 20),
            new Interval(3, 8, 15),
            new Interval(7, 10, 8)
        };
        
        // Expected Output:
        //   [ (1, 2, 20), (3, 6, 15), (7, 10, 8) ]
        
        List<Interval> result = solution.getLowestPriceIntervals(sampleInput);
        
        System.out.println("Result Intervals (lowest price coverage):");
        for (Interval iv : result) {
            System.out.println(iv);
        }
        // Visually confirm that it matches the expected intervals.
    }
}


```
<img width="667" alt="image" src="https://github.com/user-attachments/assets/fd65f5b2-df9d-40ad-a44a-e8e5dc3d0fdb" />
<img width="667" alt="image" src="https://github.com/user-attachments/assets/b527e053-1319-4ba7-86b7-e4bde5e74b85" />



33. find the second smallest number in an array

```java
/**
 * Efficient implementation to find the second smallest number in an array.
 *
 * Rationale:
 * - Time complexity is O(n): We traverse the array once.
 * - Space complexity is O(1): No extra data structures used, only two tracking variables.
 * - We handle edge cases like:
 *     * Arrays with fewer than 2 distinct elements (throws exception)
 *     * Arrays with duplicates
 *     * Negative values and integer boundaries
 * - The algorithm tracks both the smallest and the second smallest elements in one pass.
 * - Proper use of Integer.MAX_VALUE ensures correctness even with negative numbers.
 */

public class Solution {

    /**
     * Returns the second smallest distinct element in the array.
     *
     * @param nums the array of integers
     * @return the second smallest distinct integer
     * @throws IllegalArgumentException if fewer than 2 distinct elements exist
     */
    public int findSecondSmallest(int[] nums) {
        if (nums == null || nums.length < 2) {
            throw new IllegalArgumentException("Array must contain at least two elements.");
        }

        int first = Integer.MAX_VALUE;
        int second = Integer.MAX_VALUE;

        for (int num : nums) {
            if (num < first) {
                second = first;
                first = num;
            } else if (num > first && num < second) {
                second = num;
            }
        }

        if (second == Integer.MAX_VALUE) {
            throw new IllegalArgumentException("Array must contain at least two distinct elements.");
        }

        return second;
    }
}

/*
Test cases:

Input: [3, 1, 4, 1, 5]
Output: 3

Input: [2, 2, 2]
Throws IllegalArgumentException (no second distinct element)

Input: [Integer.MAX_VALUE, Integer.MIN_VALUE, 0]
Output: 0

Input: [-5, -1, -7, -3]
Output: -5

Input: [1, 2]
Output: 2

Input: [5, 1, 2, 1, 5, 2]
Output: 2

Input: [7, 7, 6, 5, 5, 5, 6]
Output: 6

Input: [1, Integer.MAX_VALUE]
Output: Integer.MAX_VALUE
*/
```
