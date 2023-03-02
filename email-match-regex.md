# Regular Expression Tutorial -  Matching an Email 

There are a number of ways an email validation regular expression(or regex) can be written.  With ICANN opening a large nuber of new top-leel domains, the email address can have a wide range of combinations and characters.  In addition to that internationals doamins can have country code resulting in multiple periods after '@'.  

All the complexities aside, one most impertant thing to keep in mind is that a regex will only check the valdility of an email structure not the legitimity of the email address.

For purposes of this tutorial, I have chosen a fairly open expression but it still has it limitation where it only allows limited set of special characters which are most commonly used in email addresses.  But the regex can be easily expanded to include others if needed.  

This tutorial explains how the following regex for Matching an Email, functions by breaking down each part of the expression and describing what it does.  

```
 /^([a-zA-Z0-9_\.-]+)@([\da-z\.-]+)\.([a-z\.]{2,6})$/
```

## Summary

The above regex for validating an email address follows the rules below:
* The email address can contain any combination of:
    * Lowercase alphabets (a-z)
    * Uppercase alphabets (A-Z)
    * Numbers (0-9)
    * Specials characters: ``` . _ - ```
* The top-level doamin name (after @) can contain any combination of:
    * Lowercase alphabets (a-z)
    * Specials characters: ``` . - ```
    * Multiple periods
* The last part of the address can be 2 to 6 character long and can contain:
    * Lowercase alphabets (a-z)
    * Specials characters: ``` . ```

Some examples of qualifying email addresses:
```
jane.doe@example.com
jane1@example.com
Jane.mcLaren@example.co.irl
jane.doe@example.co.uk
jane_doe@example.co.united
jane-doe@example.usa
```

Some examples of non-qualifying email address:
```
jane.doe@example.co_uk
jane$doe@example.com
jane_doe@example.england
```

Now let's dissect the regrex and see what each part signifies and how it conforms to the above rules.

## Table of Contents

- [Anchors](#anchors)
- [Quantifiers](#quantifiers)
- [Grouping Constructs](#grouping-constructs)
- [Bracket Expressions](#bracket-expressions)
- [Character Classes](#character-classes)
- [Character Escapes](#character-escapes)

## Regex Components

A regex is considered a literal, so it's pattern must be wrapped in slash characters (/).  It is certainly true in our example:

```/```^([a-zA-Z0-9_\.-]+)@([\da-z\.-]+)\.([a-z\.]{2,6})$```/```

Following are the components we have used in this regex:

### Anchors

A regrex has two ****anchors** ```^``` and ```$```.

At the beginning of the regex is the "anchor" ```^``` which signifies the string of characters that follows it.  The string that follows can be an exact match or a range of possible matches displayed in [bracket expressions](#bracket-expressions) ```[]``` explained below.  Bracket expressions are a type of [character class](#character-classes) also explained below.

In our regex, anchor ```^``` is followed by what are called [grouping contructs](#grouping-constructs) which break up the string in different parts, each fulfilling different requirements.  Grouping constructs are defined by parantheses ```()``` around them. As you can see we have three groups each containing a bracket expression.

At the end of the regrex is the other "anchor" ```$``` which signifies the string that ends with characters that precedes it.  Here also, the string that preceded can be an exact match ot a range of possible matches defined in bracket expresion or group construct. 

In our regrex, ```$``` is also preceded by ```{2,6}``` which is not part of qualitfying pattern but is a special component called a [quantifier](#quantifiers) which we will discuss later. ```+``` is also a quantifier we are using in our regex.

Next, let us examine the three Group Contructs we have in our "Matching an Email" regex.

### Grouping Constructs

Grouping Contructs group a section of a regex is by using parentheses ```()```. Each section within parentheses is known as a **subexpression**.  

There are two types of grouping contructs: **capturing** and **non-capturing**.  Capturing group defines the aprt of regex that is included in match and captures or stores the match for posible reuse.  Non-capturing group defines the part of regex which will not be included in match. A capturing group can be made a non-capturing group by adding ```?:``` in front. 

In our regex, all three groups are capturing group so they all are used for matching the strings.

* 1st Capturing Group ```([a-zA-Z0-9_\.-]+)``` has following parts.  
    * The bracket expression ```[a-zA-Z0-9_\.-]``` which will be used for matching
    * The quantifier ```+``` which matches the previous token (the bracket expression ```[a-zA-Z0-9_\.-]```) between one and unlimited times (greedy)
* It is followed by ```@``` matches the character @ literally (case sensitive)
* 2nd Capturing Group ```([\da-z\.-]+)``` with following parts
    * The bracket expression ```[\da-z\.-]``` which will be used for matching
    * The quantifier ```+``` which matches the previous token (the bracket expression ```[\da-z\.-]```) between one and unlimited times (greedy)
* It is followed by ```\.``` matches the character . literally (case sensitive).  ```\``` is a [character escape](#character-escapes) explained below.
* 3rd Capturing Group ```([a-z\.]{2,6})``` with following parts
    * The bracket expression ```[a-z\.]``` which will be used for matching
    * The quantifier {2,6} matches the previous token ```[a-z\.]``` between 2 and 6 times, as many times as possible (greedy)

Let's understand the three bracket expression we have with some examples:

### Bracket Expressions

Anything inside a set of square brackets ([]) represents a range of characters that we want to match. These patterns are known as **bracket expressions** or a **positive character group**, because they define the characters we want to include. We can write these expressions to include all of the characters we want to match.  A hyphen (-) is commonly used between alphanumeric characters (letters and numbers only) to represent a range of those possible characters for example [a-d] means [abcd], [103] means [0123].

Let us break down our 3 bracket expression.

* ```[a-zA-Z0-9_\.-]``` 
    * **a-z** matches a single character in the range between lowercase a and z  (case sensitive)
    * **A-Z** matches a single character in the range between uppercase A and Z  (case sensitive)
    * **0-9** matches a single character in the range between 0 and 9  (case sensitive)
    * **_** matches the character _ literally (case sensitive)
    * **\.**matches the character . literally (case sensitive)
    * **-** matches the character - literally (case sensitive)
    * Example of string that will match:
        ```
         jane.doe
         jane1
         Jane.doe1
         jane.mcLaren
         jane-doe
         jane_doe
        ```
    * Examples of string that will fail the match:
        ```
        jane$doe ($ is not an qualifying character)
        ```
* ```[\da-z\.-]```
    * **\d** matches a digit (equivalent to [0-9]). \d is type a **character class**.
    * **a-z** matches a single character in the range between lowercase a and z (case sensitive)
    * **\.** matches the character . literally (case sensitive)
    * **-** matches the character - literally (case sensitive)
    * Examples of matching strings:
        ```
        example
        example1
        example.co
        example-test
        ```
    * Examples of non-matching strings:
        ```
        example.CO
        example$
        ```
* ```[a-z\.]```
    * **a-z** matches a single character in the range between lowercase a and z (case sensitive)
    * **\.** matches the character . literally (case sensitive)
    * Examples of matching strings:
      ```
        com
        eng.l
        ```
    * Examples of non-matching strings:
        ```
        COM
        com1
        com$
        ```

Some of our grouping constructs are followed by quantifiers.  Let's examine how quantifiers work.

### Quantifiers

Quantifiers set the limits of the string that a regex  (or group contruct) matches. They frequently include the minimum and maximum number of characters that the regex is looking for.

Quantifiers are inherently **greedy**, meaning they match as many occurrences of particular patterns as possible.  Each of these quantifiers can be made **lazy** by adding the ? symbol after it, meaning it will match as few occurrences as possible.

Here are the commonly used quantifiers:
* ```*```: matches the pattern zero or more times
* ```+```: matches the pattern one or more times
* ```?```: matches the pattern zero or one time
* ```{}```: Curly brackets can provide three different ways to set limits for a match:
    * **{ n }**: matches the pattern exactly n number of times
    * **{ n, }**: matches the pattern at least n number of times
    * **{ n, x }**: matches the pattern from a minimum of n number of times to a maximum of x number of times

In our "Maching an Email" regex, we used ```+``` quantifier in 1st and 2nd group construct to matches the previous token between one and unlimited times (greedy).  

We also used ```{2,6}``` in the 3rd construct that matches the preceding string pattern a minimum of 2 times and a maximum of 6 times. This means that strings like **co**, **com**, **united** are qualified but **england** is not.

### Character Classes

A character class in a regex defines a set of characters, any one of which can occur in an input string to fulfill a match.

Here are some common character classes:
* ```.```: matches any character except the newline character (\n)
* ```\d```: matches any Arabic numeral digit. This class is equivalent to the bracket expression [0-9].  We used in our 2nd group construct.
* ```\w```: matches any alphanumeric character from the basic Latin alphabet, including the underscore (_). This class is equivalent to the bracket expression [A-Za-z0-9_].
* ```\s```: matches a single whitespace character, including tabs and line breaks

Each of the last three character classes can be changed to perform an inverse match by capitalizing the letter character. For example, \D matches a non-digit character.


### Character Escapes

The backslash (\) in a regex allows a character to be interpreted literally. For example, the open square ([) is used to begin a bracket expression, but adding a backslash before the open square bracker (\[) means that the regex should look for the open square bracket character instead of beginning to define a bracket expression. 

It's important to note that all special characters, including the backslash (\), lose their special significance inside bracket expressions and could be used without the backslash.

In our regress, we used the backslash with **@** after 1st group contruct and with **.** after 2nd group construct so that we can interpret them literally as mandatory part of an email address.

Let's look at out "Matching an Email" regex once more.  Hopefully the above explanation now makes it easier to understand.

```
 /^([a-zA-Z0-9_\.-]+)@([\da-z\.-]+)\.([a-z\.]{2,6})$/
``````

## Author

Hope this was helpful to you as it was to me to understand the various regex components!  

I am currently working towards a Certification in Full-stack Web Development through University of Berkeley Extension Cding Bootcamp.  If you have any questions or suggestions, please feel free to contact me.  My Github profile is  https://github.com/rbhumbla1.

