# Chapter 2: Meaningful Names

## Use Intention-Revealing Names
- A name does not reveal its intent if it requires a comment.
```java
int d; // elapsed time in days
```
- More intent-revealing of what it is measured and unit of that measurement
```java
int elapsedTimeInDays;
int daysSinceCreation;
int daysSinceModification;
int fileAgeInDays;
```

## Use Intention-Revealing Functions
- Before optimization
```java
public List<int[]> getThem() {
    List<int[]> list1 = new ArrayList<int[]>();
    for (int[] x : theList)
        if (x[0] == 4) list1.add(x);
    return list1;
}
```
- After optimization
```java
public List<Cell> getFlaggedCells() {
    List<Cell> flaggedCells = new ArrayList<Cell>();
    for (Cell cell : gameBoard)
        if (cell.isFlagged())
            flaggedCells.add(cell);
    return flaggedCells;
}
```
    - Meaningful name: __gameBoard__
    - Intention-revealing function: __isFlagged__

## Make Meaningful Distinctions
- Reads much better when _source_ and _destination_ are used as arguments
```java
public static void copyChars(char a1[], char a2[]) {
    for (int i = 0; i < a1.length; i++) {
        a2[i] = a1[i];
    }
}
```
- Distinctions must be make in meaningful ways or according to naming conventions

|   Indistinguishable Names   |
|---|---|
|  moneyAmount |  money |
|  customerInfo |  customer |
|  accountData |  account |
|  theMessage |  message |

## Use Pronounceable Names
> If you can’t pronounce it, you can’t discuss it without sounding like an idiot. This matters because programming is a social activity.

- Poor naming. New developers had to have the variables explained to them, and then they spoke about it in silly made-up words
```java
class DtaRcrd102 {
    private Date genymdhms;
    private Date modymdhms;
    private final String pszqint = "102";
    /* ... */
};
```
- After using proper English terms
```java
class Customer {
    private Date generationTimestamp;
    private Date modificationTimestamp;
    private final String recordId = "102";
    /* ... */
}
```

## Use Searchable Names
> One might easily grep for MAX_CLASSES_PER_STUDENT, but the number 7 could be more troublesome.

- Searches may turn up the digit as part of file names, and in various expressions where the value is used with different intent.

- Compare the following two versions of same functionalities
```java
for (int j=0; j<34; j++) {
    s += (t[j]*4)/5;
}
```
```java
int realDaysPerIdealDay = 4;
const int WORK_DAYS_PER_WEEK = 5;
int sum = 0;
for (int j=0; j < NUMBER_OF_TASKS; j++) {
    int realTaskDays = taskEstimate[j] * realDaysPerIdealDay;
    int realTaskWeeks = (realdays / WORK_DAYS_PER_WEEK);
    sum += realTaskWeeks;
}
```
- It is much easier to find *WORK_DAYS_PER_WEEK* than to find all the places where 5 was used

## Interfaces and Implementations
- Choose implementation if it is necessary to pick one between it and interface.
- Calling it __ShapeFactoryImp__, or even the hideous __CShapeFactory__, is preferable to encoding the interface.

## Avoid Mental Mapping
> Readers shouldn’t have to mentally translate your names into other names they already know.

- :-1:: In general programmers are pretty smart people. Smart people sometimes like to show off their smarts by demonstrating their mental juggling abilities.
- :+1:: One difference between a smart programmer and a professional programmer is that the professional understands that **_clarity is king_**.

## Method Names
> Methods should have verb or verb phrase names

- When constructors are overloaded, use static factory methods with names that describe the arguments. For example,
```java
Complex fulcrumPoint = Complex.FromRealNumber(23.0);
```
is generally better than
```java
Complex fulcrumPoint = new Complex(23.0);
```

## Use Problem Domain Names
> When there is no “programmer-eese” for what you’re doing, use the name from the problem domain.

At least the programmer who maintains your code can ask a domain expert what it means.

## Add Meaningful Context
```java
private void printGuessStatistics(char candidate, int count) {
    String number;
    String verb;
    String pluralModifier;
    if (count == 0) {
        number = "no";
        verb = "are"; pluralModifier = "s";
    } else if (count == 1) {
        number = "1";
        verb = "is"; pluralModifier = "";
    } else {
        number = Integer.toString(count);
        verb = "are";
        pluralModifier = "s";
    }
    String guessMessage = String.format("There %s %s %s%s", verb, number, candidate, pluralModifier );
    print(guessMessage);
}
```
The improvement of context also allows the algorithm to be made much cleaner by breaking it into many smaller functions
```java
public class GuessStatisticsMessage {
    private String number;
    private String verb;
    private String pluralModifier;
    public String make(char candidate, int count) {
        createPluralDependentMessageParts(count);
        return String.format(
        "There %s %s %s%s",
        verb, number, candidate, pluralModifier);
    }
    private void createPluralDependentMessageParts(int count) {
        if (count == 0) {
            thereAreNoLetters();
        } else if (count == 1) {
            thereIsOneLetter();
        } else {
            thereAreManyLetters(count);
        }
    }
    private void thereAreManyLetters(int count) {
        number = Integer.toString(count);
        verb = "are";
        pluralModifier = "s";
    }
    private void thereIsOneLetter() {
        number = "1";
        verb = "is";
        pluralModifier = "";
    }
    private void thereAreNoLetters() {
        number = "no";
        verb = "are";
        pluralModifier = "s";
    }
}

```
