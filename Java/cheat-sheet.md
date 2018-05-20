# Java Cheat Sheet
## Input
### import Scanner and create a reader
```Java
import java.util.Scanner;
public class ProgramName {
    public static void main(String[] args) {
        Scanner reader = new Scanner(System.in);

        // code here
    }
}
```

### reading a string
```Java
String text = reader.nextLine();
```

### reading (/converting) an integer
```Java
int number = Integer.parseInt(reader.nextLine());
```

## Strings
### Methods
```Java
string.equals(string2);
string.length();
string.charAt();
string.substring(x,y);
string.indexOf(string2);
string.concat();
string.replace();
string.trim();
string.split();
string.toLowerCase();
string.toUpperCase();
```

## ArrayLists
```Java
import java.util.ArrayList;

public class ListProgram {

    public static void main(String[] args) {
        ArrayList<String> wordList = new ArrayList<String>();

        wordList.add("First");
        wordList.add("Second");
    }
}
```

### Methods
```Java
list.size();
list.add();
list.get(i);
list.remove(); // takes index or string as parameter (for string arrays)
               // list.remove(Integer.valueOf(num))
list.contains(string);
```

## Collections
```Java
import java.util.Collections;
```

### Methods
```Java
Collections.sort(list);
           .shuffle();
           .reverse();
```

## Arrays
Unlike with ArrayLists, the size of the array (the amount of cells in an array) cannot be changed. Growing an array always requires creating a new array and copying the cells of the old array to the new one.
```Java
import java.util.Arrays;

int cells = 99;
int[] array = new int[cells];
```

## HashMaps
```Java
import java.util.HashMap;
```

### Methods
```Java
collection.keySet()
collection.values()
```

## Randomizing
```Java
import java.util.Random;

public class Randomizing {
    public static void main(String[] args) {
        Random randomizer = new Random(); // creates a random number generator
        int i = 0;

        while (i < 10) {
            // Generates and prints out a new random number on each round of the loop

            // returns a random integer within the range 0 to the integer given as parameter- 1
            System.out.println(randomizer.nextInt(10)); // or .nextDouble()
            i++;
        }
    }
}
```
## Dates
```Java
int day = Calendar.getInstance().get(Calendar.DATE);
```