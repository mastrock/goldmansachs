
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

