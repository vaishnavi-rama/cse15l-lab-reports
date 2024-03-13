## Lab Report 4

### Part 1 - Debugging Scenario
The file directory (starting from the default directory that the terminal starts in is) used for setup is the following: 
- ArrayEx.java
- ArrayTests.java
- test.sh
- lib/
    - hamcrest-core-1.3.jar
    - junit-4.13.2.jar

The lib folder contains the standard testing library used through the class. 

Before fixing any bugs, here are the contents of all the files: 

The contents of the `ArrayEx.java` file are: 
```
import java.util.*;

public class ArrayEx{
  static int[] reversed(int[] arr) {
    int[] newArray = new int[arr.length];
  
    for (int i = arr.length - 1; i > 0; i--){
        newArray[i] = arr[arr.length - i - 1];
    }
    return newArray;
  }
}
```
The contents of `ArrayTests.java` are:
```
import static org.junit.Assert.*;
import org.junit.*;

public class ArrayTests {
	@Test 
	public void testReverse() {
    int[] input1 = { 3 };
    int[] res = ArrayEx.reversed(input1);
    assertArrayEquals(new int[]{ 3 }, res);
	}

  @Test
  public void testReverseMult() {
    int[] input1 = { 3, 2, 1 };
    int[] res1 = ArrayEx.reversed(input1);
    assertArrayEquals(new int[]{3, 2, 1}, res1);
	}
}
```

The `test.sh` file contains:
```
set -e

#javac Server.java ChatServer.java
#java ChatServer 4000

javac -cp .:lib/hamcrest-core-1.3.jar:lib/junit-4.13.2.jar *.java
java -cp .:lib/hamcrest-core-1.3.jar:lib/junit-4.13.2.jar org.junit.runner.JUnitCore ArrayTests
```
Student Post (with `test.sh` script, `ArrayTests.java`, and `ArrayEx.java` attached): I started off by running `javac ArrayEx.java` to compile the ArrayEx.java file and then ran the tests with my bash script through the command `bash test.sh`. The following is the terminal output from running the two commands. As you can see, although we were able to successfully compile the files, both tests we wrote failed. I'm not very sure what the error is, my guess is there is a bug with how the new array is created. 

```
[user@sahara ~]$ javac ArrayEx.java
[user@sahara ~]$ bash test.sh
JUnit version 4.13.2
.E.E
Time: 0.008
There were 2 failures:
1) testReverse(ArrayTests)
arrays first differed at element [0]; expected:<3> but was:<0>
at org.junit.internal.ComparisonCriteria.arrayEquals(ComparisonCriteria.java:78)
at org.junit.internal.ComparisonCriteria.arrayEquals(ComparisonCriteria.java:28)
at org.junit.Assert.internalArrayEquals(Assert.java:534)
at org.junit.Assert.assertArrayEquals(Assert.java:418)
at org.junit.Assert.assertArrayEquals(Assert.java:429)
at ArrayTests.testReverse(ArrayTests.java:9)
... 30 trimmed
Caused by: java.lang.AssertionError: expected:<3> but was:<0>
at org.junit.Assert.fail(Assert.java:89)
at org.junit.Assert.failNotEquals(Assert.java:835)
at org.junit.Assert.assertEquals(Assert.java:120)
at org.junit.Assert.assertEquals(Assert.java:146)
at org.junit.internal.ExactComparisonCriteria.assertElementsEqual(ExactComparisonCriteria.java:8)
at org.junit.internal.ComparisonCriteria.arrayEquals(ComparisonCriteria.java:76)
... 36 more

2) testReverseMult(ArrayTests)
arrays first differed at element [0]; expected:<3> but was:<0>
at org.junit.internal.ComparisonCriteria.arrayEquals(ComparisonCriteria.java:78)
at org.junit.internal.ComparisonCriteria.arrayEquals(ComparisonCriteria.java:28)
at org.junit.Assert.internalArrayEquals(Assert.java:534)
at org.junit.Assert.assertArrayEquals(Assert.java:418)
at org.junit.Assert.assertArrayEquals(Assert.java:429)
at ArrayTests.testReverseMult(ArrayTests.java:16)
... 30 trimmed
Caused by: java.lang.AssertionError: expected:<3> but was:<0>
at org.junit.Assert.fail(Assert.java:89)
at org.junit.Assert.failNotEquals(Assert.java:835)
at org.junit.Assert.assertEquals(Assert.java:120)
at org.junit.Assert.assertEquals(Assert.java:146)
at org.junit.internal.ExactComparisonCriteria.assertElementsEqual(ExactComparisonCriteria.java:8)
at org.junit.internal.ComparisonCriteria.arrayEquals(ComparisonCriteria.java:76)
... 36 more

FAILURES!!!
Tests run: 2,  Failures: 2
```

Response from TA: Looking at your tests and the output, it appears that the first element that should have been returned for both cases is 3, but your implementation is returning 0. More specifically the expected values for both tests are [3] and [3, 2, 1] but because the first element is 0 in both cases, the tests are failing. However, there is no 0 element in either array. Can you take a look at your `ArrayEx.java` code to see if you are unintentionally changing the array values or can you think of a time when a value in a Java array is set to 0? Additionally, try looking at the whole array returned by both tests. Are the rest of the values 0 as well? 

Student response: To look at the whole array that is being returned by the `reversed(int[] arr)` method within the `ArrayEx.java` file, I added print statements to both my tests. I used an `Arrays.toString()` method that allowed me to print the contents of the array. I saw that the first array returned [0] and the second array was [0, 2, 3] as you can see from the terminal output below:
```
[user@sahara ~]$ bash test.sh
JUnit version 4.13.2
.[0]
E.[0, 2, 3]
--- rest of the output is similar so it is ommitted ----
```

The modified `ArrayTests.java` file is: 
```
import static org.junit.Assert.*;
import org.junit.*;
import java.util.*;

public class ArrayTests {
	@Test 
	public void testReverse() {
    int[] input1 = { 3 };
    int[] res = ArrayEx.reversed(input1);
    System.out.println(Arrays.toString(res));
    assertArrayEquals(new int[]{ 3 }, res);
	}

  @Test
  public void testReverseMult() {
    int[] input1 = { 3, 2, 1 };
    int[] res1 = ArrayEx.reversed(input1);
    System.out.println(Arrays.toString(res1));
    assertArrayEquals(new int[]{1, 2, 3}, res1);
	}
}
```

After looking over my code in the `reversed()` method, I believe I am not setting any array values to 0 by hard coding them. And I remember learning in previous programming classes that when an empty Java int array is created, it is intialized to 0 by default. Maybe that is where the 0 element is coming from? Because the second and third elements in the second test I wrote were correctly reversed, the problem may be with how I am calculating indices in my for loop as the first index remains unitialized in both tests as 0. Taking a look at my for loop and conditions, [`for(int i = arr.length - 1; i > 0; i--)`], I realize I am not including the first element of the array. Because my for loop only runs when `i > 0`, it terminates when `i = 0`. Therefore, the first element in the array is skipped and is never reversed. To fix this problem, I need to change my for loop condition to `i >= 0`, which would make it `for(int i = arr.length - 1; i >= 0; i--)`. 

Running the tests after compilling `javac ArrayEx.java` by typing `bash test.sh` into the terminal shows us that we successfully fixed the error and both tests pass. 

The following is the output generated from running the bash test script:
```[user@sahara ~]$ javac ArrayEx.java
[user@sahara ~]$ bash test.sh
JUnit version 4.13.2
..
Time: 0.005

OK (2 tests)
```

This is the updated `ArrayEx.java` file:
```
public class ArrayEx{
  static int[] reversed(int[] arr) {
    int[] newArray = new int[arr.length];
  
    for (int i = arr.length - 1; i >= 0; i--){
        newArray[i] = arr[arr.length - i - 1];
    }
    return newArray;
  }

}
```

### Part 2 - Reflection
Something I learned in lab was how to use jdb to debug and run through code. Building off what we learned in lecture, it was really cool that we could use commands such as suspend and print to see the values of the variables. Instead of using a lot of print statements to figure out what is happening in the code, using jdb to set breakpoints, which makes the whole debugging process easier and more efficient.  
