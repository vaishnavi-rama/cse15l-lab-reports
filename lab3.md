## Lab Report 3

### Part 1 
The bug I chose from week 4's lab is in the `ArrayExamples.java` class and is the `static int[] reversed(int[] arr)` method which is supposed to return a new array with all the elements in the original array reversed. To test this method, one JUnit test I wrote that failed is: 
```
 @Test
  public void testReversedNonEmpty() {
    int[] input1 = {1, 2, 3 };
    assertArrayEquals(new int[]{3, 2, 1}, ArrayExamples.reversed(input1));
  }
```
Another test I wrote that does not induce a failure is: 
```
  @Test
  public void testReversedEmpty() {
    int[] input1 = {};
    assertArrayEquals(new int[]{}, ArrayExamples.reversed(input1));
  }
```

After writing and running the JUnit tests, I got the following ouput, indicating that one test failed and the other passed:

![Image](junit_error.png)

Before making any changes, the reversed method is the following: 
```
  static int[] reversed(int[] arr) {
    int[] newArray = new int[arr.length];
    for(int i = 0; i < arr.length; i += 1) {
      arr[i] = newArray[arr.length - i - 1];
    }
    return arr;
  }
```
After looking at the test ouput, we see that the expected first value of the returned array is supposed to be 3, however it is 0. Looking more closely at the code insde the for loop, we realize that elements are being assigned to the old array from the new array instead of the other way around. We know that new empty arrays are intialized with the value 0, which could explain why the actual output in the test is returning 0. So to fix this method, I changed the assignment line inside the for loop to `newArray[i] = arr[arr.length - i - 1]` and changed the return value of the method to the new array. 

The new code is: 
```
  static int[] reversed(int[] arr) {
    int[] newArray = new int[arr.length];
    for(int i = 0; i < arr.length; i += 1) {
      newArray[i] = arr[arr.length - i - 1];
    }
    return newArray;
  }
```

### Part 2
The command I chose to use is `less`, which is used to view the contents of a file. 


**1. less -E \<filename\>**\
One option for this command is `-E`, which allows you to automatically exit back to the command line prompt, when you reach the end of the file after scrolling through. Otherwise you would have to type in "q" if you did not use this option. 

Example 1: This is an example of using the `less` command with the `-E` option on the `chapter-1.txt` file in the `./technical` directory. My working directory for this command is `/Users/vramanujan/docsearch/technical/911report`.  I was able to scroll through the whole file and when I reached the end, I was automatically brought back to the command line prompt. I ommitted most of the text file in between for conciseness as it contained a large amount of text. 

```
MacBook-Pro-56:911report vramanujan$ less -E chapter-1.txt
                
"WE HAVE SOME PLANES"

    Tuesday, September 11, 2001, dawned temperate and nearly cloudless in the eastern United States. Millions of men and women readied themselves for work. Some made their way to the Twin Towers, the signature structures of the World Trade Center complex in New York City. Others went to Arlington, Virginia, to the Pentagon. Across the Potomac River, the United States Congress was back in session. At the other end of Pennsylvania Avenue, people began to line up for a White House tour. In Sarasota, Florida, President G
:
... note rest of the .txt file is ommitted ...
MacBook-Pro-56:911report vramanujan$
```

Example 2: This is another example of the `less` command with the `-E` option, in a different sub-directory in `./technical` compared to the previous example. The working directory for this command is `/Users/vramanujan/docsearch/technical/plos`. As expected, the command terminated and brought me back to the command line prompt when I finished scrolling to the end of the file.  
```
MacBook-Pro-56:plos vramanujan$ less -E journal.pbio.0020012.txt

 The pathologist makes do with red wine until an effective drug is available, the
        biochemist discards the bread from her sandwiches, and the mathematician indulges in
        designer chocolate with a clear conscience. The demographer sticks to vitamin supplements,
        and while the evolutionary biologist calculates the compensations of celibacy, the
        population biologist transplants gonads, but so fa
r only those of his laboratory mice.
        Their common cause is to control and extend the he
althy lifespan of humans. They want to

... note rest of the .txt file is ommitted ...

MacBook-Pro-56:plos vramanujan$
```
Source: https://www.geeksforgeeks.org/less-command-linux-examples/

**2. -p [pattern] \<filename\>**
Another option is `-p`, after which you provide a pattern to search for. This command opens the file and starts at the first occurrence of this pattern. The pattern is also highlighted when the text file is opened. If no pattern match is found, I get an error back that said "Pattern not found (press RETURN)".   

Example 1: This is an example of how the `-p` option works with the `less` command. The directory I am working in is `/Users/vramanujan/docsearch/technical/biomed`. The file was opened at the first occurence of the word "released" and then I was able to scroll (both up/down) through the file as expected. 

```
MacBook-Pro-56:biomed vramanujan$ less -p increased 1468-6708-3-1.txt

        associated with increased mortality in those over 
age 65.
        Six large controlled population-based studies of
        non-smoking older adults have investigated the association
        between body mass index (BMI) and mortality, contr
olling
        for relevant covariates [ 1 2 3 4 5 6 ] . All stud
ies found
:
... note rest of the .txt file is ommitted ...
```

Example 2: This is a seperate example using the `-p` option with the `less` command. Although the file is opened at the first instance of the pattern match, all the remaining matches are also highlighted in the terminal window. The working directory is `/Users/vramanujan/docsearch/technical/government/About_LSC`.

```
MacBook-Pro-56:About_LSC vramanujan$ less -p of Comments_on_semiannual.txt

I am pleased to transmit the comments of the Legal Services
Corporation ("LSC" or "Corporation") Board of Directors ("Board")
regarding the Semiannual Report of LSC's Office of Inspector
General ("OIG") for the six-month period of October 1, 2000 through
March 31, 2001.
:
... note rest of the .txt file is ommitted ...
```
Source: https://www.geeksforgeeks.org/less-command-linux-examples/

**3. -s \<filename\>**
A third option to use with the `less` command is `-s`, which combines all successive blank lines into one line when the file is being read in the terminal. 

Example 1: This is an example of how the `-s` option works with the `less` command. The working directory is `/Users/vramanujan/docsearch/technical/government/Env_Prot_Agen` and as you can see in the ouput below there is a maximum of one empty line in between sentences / paragraphs in the text. 

```
MacBook-Pro-56:Env_Prot_Agen vramanujan$ less -s ctf7-10.txt

7.1 TYPES OF DILUTION WATER

7.1.1 The type of dilution water used in effluent toxicity
tests will depend largely on the objectives of the study.

7.1.1.1
If the objective of the test is to estimate the absolute
chronic
:
... note rest of the .txt file is ommitted ...

```

Example 2: This is another example of how the `-s` (squeeze blank lines) command works by replacing mutliple blank lines in succession with a single blank line. My working directory for this is `/Users/vramanujan/docsearch/technical/biomed`.  

```
MacBook-Pro-56:biomed vramanujan$ less -s 1468-6708-3-7.txt

...file beginning is removed as there aren't any blank lines

        positive effects of peripheral alpha-1 antagonists on
        surrogate endpoints such as cholesterol levels and
        tolerability. In light of the results of ALLHAT, we
        performed a review of the literature on doxazosin,
        terazosin, and prazosin.
      
        Methods
        Medline and the Cochrane databases were used
        :
... note rest of the .txt file is ommitted...

```

Source: https://www.geeksforgeeks.org/less-command-linux-examples/

**4. -N \<filename\>**
A fourth option is to use `-N` with the `less` option and this will open the file in the terminal with line numbers. 

Example 1: This is an example of the `-N` option and as you can see in the output, the line numbers are displayed at the start of each line, and are repeated as necessary to fit the same line in the terminal output. The working directory is /Users/vramanujan/docsearch/technical/911report. 

```
MacBook-Pro-56:911report vramanujan$ less -N chapter-6.txt
      1 
      2     
      3         
      4             FROM THREAT TO THREAT
      5             In chapters 3 and 4 we described how
      5  the U.S. government adjusted its existing
      6                 agencies and capacities to addre
      6 ss the emerging threat from Usama Bin Ladin and
      6 his
      7                 associates. After the August 199
      7 8 bombings of the American embassies in Kenya
      :
... note rest of the .txt file is ommitted ...
```

Example 2: This is another example of the `-N` command, and similar to the last example, each line in the terminal output is preceded by a line number. Some line numbers are repeated to reflect the actual file line length. The working directory for this command is `/Users/vramanujan/docsearch/technical/plos`

```
MacBook-Pro-56:plos vramanujan$ less -N journal.pbio.0020035.txt
      1 
      2   
      3     
      4       
      5         
      6         For half a century, natural pr
      6 oducts from microorganisms have been t
      6 he main source of
      7         medicines for treating infecti
      7 ous disease. The most important chemic
      7 al class of these
:
```

Source: https://linuxize.com/post/less-command-in-linux/
