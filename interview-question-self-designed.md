# Interview Question \(Self-designed\)

## Find all children of a location

### Question

```java
/**
 * Given a list of locations and the name of a single location, return a list of its children.
 * 
 * The location A is the children of another location B if the parentId of A equals to the id 
 * of B or the id of one of B's children;
 * 
 * Hint/Extra: What data structure can you use to reduce the time complexity of the algorithm
 *
 * Definition for location.
 * class Location {
 *     String name;
 *     String id;
 *     String parentId;
 * }
 */


import java.io.*;
import java.util.*;
class Location {
  
  private String name;
  private String id;
  private String parentId;

  Location(String name, String id, String parentId) {
    this.name = name;
    this.id = id;
    this.parentId = parentId;
  }
  
  public String getName() {
    return this.name;
  }
  
  public String getId() {
    return this.id;
  }
  
  public String getParentId() {
    return this.parentId;
  }
  
}

public class Solution {
  public static List<Location> findLocationChildren(List<Location> locations, String name) {
        return locations;
  }
  
  public static void main(String[] args) {
    // Generate locations array
    Location loc;
    List<Location> locations = new ArrayList<>(
      Arrays.asList(
        new Location("ISAFE1", "a01", null),
        new Location("ISAFE2", "a02", "a01"),
        new Location("ISAFE3", "a03", null),
        new Location("ISAFE4", "a04", "a02"),
        new Location("ISAFE5", "a05", "a01"),
        new Location("ISAFE6", "a06", "a04"),
        new Location("ISAFE7", "a07", "a06")
      )
    );
    
    
    
    List<Location> result = findLocationChildren(locations, "ISAFE2");
    
    for (Location l : result) {
      System.out.println(l.getName());
    }
  }
}
```

### Answer

```java
/**
 * Given a list of locations and the name of a single location, return a list of its descendants.
 * 
 * The location A is the children of another location B if the parentId of A equals to the id 
 * of B or the id of one of B's descendants;
 * 
 * Hint/Extra 1: What data structure can you use to reduce the time complexity of the algorithm
 * Extra 2: If you are using recursion to implement the algorithm, can you think up of a iterative approach?
 * Extra 3: Can you modify your code such that all the locations are printed out in such an order 
 *  that child locations are placed immediately after parent location, and siblings are 
 *  ordered in alphabetical order.
 * Extra 4: Given a sorted list of locations, re-order the list after an attribute change in 
 * a particular location.
 *
 * Definition for location.
 * class Location {
 *     String name;
 *     String id;
 *     String parentId;
 * }
 */


import java.io.*;
import java.util.*;
class Location {
  
  private String name;
  private String id;
  private String parentId;

  Location(String name, String id, String parentId) {
    this.name = name;
    this.id = id;
    this.parentId = parentId;
  }
  
  public String getName() {
    return this.name;
  }
  
  public String getId() {
    return this.id;
  }
  
  public String getParentId() {
    return this.parentId;
  }
  
}

public class Solution {
  public static List<Location> findLocationChildren(List<Location> locations, String name) {
    // Current root location, identified by the name parameter
    Location root = null;
    // The result array
    List<Location> res = new ArrayList<>();
    // The map to map parentId to Location instance
    Map<String, List<Location>> map = new HashMap<>();
    
    for (Location l : locations) {
      String pid = l.getParentId();
      if (map.getOrDefault(pid, null) == null) {
        map.put(pid, new ArrayList<>());
      }
      List<Location> children = map.get(pid);
      children.add(l);
      if (l.getName().equals(name)) {
        root = l; 
      }
    }
    
    // If root location does not exist in the locations array
    if (root == null) {
      System.out.println("Root location not found");
      return res;
    }
    
    // Using stack to get all the location children
    Stack<Location> stack = new Stack<>();
    stack.push(root);
    Location curr;
    while (!stack.empty()) {
      curr = stack.pop();
      String currId = curr.getId();
      res.add(curr);
      List<Location> children = map.getOrDefault(currId, null);
      if (children == null) {
        continue;
      }
      for (Location child: children) {
        stack.push(child); 
      }
    }
    
    
    
    return res;
  }
  
  public static void main(String[] args) {
    // Generate locations array
    Location loc;
    List<Location> locations = new ArrayList<>(
      Arrays.asList(
        new Location("ISAFE1", "a01", null),
        new Location("ISAFE2", "a02", "a01"),
        new Location("ISAFE3", "a03", null),
        new Location("ISAFE4", "a04", "a02"),
        new Location("ISAFE5", "a05", "a01"),
        new Location("ISAFE6", "a06", "a04"),
        new Location("ISAFE7", "a07", "a06"),
        new Location("ISAFE8", "a08", "a02")
      )
    );
    
    
    
    List<Location> result = findLocationChildren(locations, "ISAFE2");
    
    for (Location l : result) {
      System.out.println(l.getName());
    }
    
    /*
    Expected output:
      ISAFE2
      ISAFE8
      ISAFE4
      ISAFE6
      ISAFE7
    */
  }
}
```

## Ebay

### **countOccurences**

```java
import java.io.*;
import java.util.*;

class Solution {
  /*
   * Constraint: 0 <= n <= 1000
   * Write your solution here.
   */
  public static int countOccurences(int n) {
    
    int count = 1;
    
    for (int i = 1; i <= n; i++) {
      count += countOccurence(i); 
    }
    
    return count;
  }
  
  public static int countOccurence(int n) {
    // store the remainder after right shift by 1 bit
    int remain = n;
    int count = 0;
    
    
    int lastDigit;
    while (remain != 0) {
      // get the last digit
      lastDigit = remain % 10;
      
      // conditions to increment
      if (lastDigit == 0 || lastDigit == 2 || lastDigit == 4) {
        count++;
      }
      
      // right shift
      remain = remain / 10;
    }
    return count;
  }
  
  
  public static void main(String[] args) {
    
    System.out.println(countOccurences(14));
    System.out.println(countOccurences(22));
    System.out.println(countOccurences(128));
  }
}
```

```java
import java.io.*;
import java.util.*;

class Solution {
  /*
   * Write your solution here.
   */
  public static int countOccurences(int n) {
    
    
  }
  public static void main(String[] args) {
    ArrayList<String> strings = new ArrayList<String>();
    strings.add("Hello, World!");
    strings.add("Please put code below");
    for (String string : strings) {
      System.out.println(string);
    }
  }
}
```

### **restoreNumbersOnCircle**

```java
import java.io.*;
import java.util.*;

class Solution {
  /*
   * Constraint: 0 <= n <= 1000
   * Write your solution here.
   */
  public static List<Integer> restoreNumbersOnCircle(List<List<Integer>> pairs) {
    List<Integer> result = new ArrayList<>();
    return result;
  }
 
  
  public static void main(String[] args) {
    List<List<Integer>> pairs = Arrays.asList(
      Arrays.asList(3, 5),
      Arrays.asList(1, 4),
      Arrays.asList(2, 4),
      Arrays.asList(1, 5),
      Arrays.asList(2, 3)
    );
    List<Integer> result = restoreNumbersOnCircle(pairs);
    
    for (int num: result) {
      System.out.println(num);
    }
  }
}
```

```java
import java.io.*;
import java.util.*;

class Solution {
  /*
   * Constraint: 0 <= n <= 1000
   * Write your solution here.
   */
  public static int[] restoreNumbersOnCircle(int[][] pairs) {
    
    int n = pairs.length;
    
    if (n == 0) return new int[0];
    
    int[] result = new int[n];
    
    // forward map: arr[1] -> arr[2]    
    // reverse map: arr[2] -> arr[1]
    Map<Integer, Integer> forwardMap = new HashMap<>();
    Set<Integer> values = new HashSet<>();
    
    // loop and populate
    for (int[] pair: pairs) {
      int first = pair[0];
      int second = pair[1];
      
      if (forwardMap.getOrDefault(first, null) == null && !values.contains(second)) {
        forwardMap.put(first, second);
        values.add(second);
      } else {
        forwardMap.put(second, first);   
        values.add(first);
      }
    }
    
    // System.out.println(forwardMap.size());
    // System.out.println(backwardMap.size());
    
    int i = 0;
    int curr = pairs[0][0];
    while (i < n) {
      int temp;
      temp = forwardMap.get(curr);  
      result[i++] = curr;
      curr = temp;
    }
    
    return result;
  }
 
  
  public static void main(String[] args) {
    int[][] pairs = {
      {3, 5},
      {1, 4},
      {2, 4},
      {1, 5},
      {2, 3},
    };
    int[] result = restoreNumbersOnCircle(pairs);
    
    for (int num: result) {
      System.out.println(num);
    }
    
    // Should be [3, 5, 1, 4, 2]
  }
}
```

