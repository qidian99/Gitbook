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

