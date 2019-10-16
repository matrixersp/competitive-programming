# Codewars Kata Solutions on JavaScript

## Katas

- [5kyu](#5kyu)
  - [Did you mean ...?](#did-you-mean-)
  - [Perimeter of squares in a rectangle](#Perimeter-of-squares-in-a-rectangle)
- [6kyu](#6kyu)
  - [Numericals of a String](#numericals-of-a-string)
  - [Tic-Tac-Toe-like table Generator](#tic-tac-toe-like-table-generator)
  - [Highest Scoring Word](#highest-scoring-word)
  - [CamelCase Method](#camelcase-method)
  - [Unique In Order](#unique-in-order)
  - [Evil Autocorrect Prank](#evil-autocorrect-prank)
  - [The Vowel Code](#the-vowel-code)
  - [Simple Time Difference](#simple-time-difference)
- [7kyu](#7kyu)
  - [Drying Potatoes](#drying-potatoes)
  - [Sum of Odd Numbers](#sum-of-odd-numbers)
  - [VAPORCODE](#vaporcode)
  - [The First Non Repeated Character In A String](#the-first-non-repeated-character-in-a-string)
  - [Fun with lists: length](#fun-with-lists-length)

## [5kyu](#katas)

### [Did you mean ...?](#katas)

I'm sure, you know Google's "Did you mean ...?", when you entered a search term and mistyped a word. In this kata we want to implement something similar.

You'll get an entered term (lowercase string) and an array of known words (also lowercase strings). Your task is to find out, which word from the dictionary is most similar to the entered one. The similarity is described by the minimum number of letters you have to add, remove or replace in order to get from the entered word to one of the dictionary. The lower the number of required changes, the higher the similarity between each two words.

Same words are obviously the most similar ones. A word that needs one letter to be changed is more similar to another word that needs 2 (or more) letters to be changed. E.g. the mistyped term berr is more similar to beer (1 letter to be replaced) than to barrel (3 letters to be changed in total).

Extend the dictionary in a way, that it is able to return you the most similar word from the list of known words.

Examples:

```js
fruits = new Dictionary([
  "cherry",
  "pineapple",
  "melon",
  "strawberry",
  "raspberry"
]);
fruits.findMostSimilar("strawbery"); // must return "strawberry"
fruits.findMostSimilar("berry"); // must return "cherry"

things = new Dictionary(["stars", "mars", "wars", "codec", "codewars"]);
things.findMostSimilar("coddwars"); // must return "codewars"

languages = new Dictionary([
  "javascript",
  "java",
  "ruby",
  "php",
  "python",
  "coffeescript"
]);
languages.findMostSimilar("heaven"); // must return "java"
languages.findMostSimilar("javascript"); // must return "javascript" (same words are obviously the most similar ones)
```

#### Solution

```js
function Dictionary(words) {
  this.words = words;
}

Dictionary.prototype.findMostSimilar = function(term) {
  const arr = [];
  for (let i = 0; i < this.words.length; i++) {
    let count = 0,
      nextIdx = 0;
    this.words[i].split("").forEach(l => {
      if (term.indexOf(l, nextIdx) > -1) {
        count++;
        nextIdx = term.indexOf(l, nextIdx);
      } else count--;
    });
    arr.push(count);
  }
  return this.words[arr.indexOf(Math.max(...arr))];
};
```

### [Perimeter of squares in a rectangle](#katas)

The drawing shows 6 squares the sides of which have a length of 1, 1, 2, 3, 5, 8. It's easy to see that the sum of the perimeters of these squares is : 4 _ (1 + 1 + 2 + 3 + 5 + 8) = 4 _ 20 = 80

Could you give the sum of the perimeters of all the squares in a rectangle when there are n + 1 squares disposed in the same manner as in the drawing:

![something](https://i.imgur.com/EYcuB1wm.jpg)

#Hint: See Fibonacci sequence

#Ref: http://oeis.org/A000045

The function perimeter has for parameter n where n + 1 is the number of squares (they are numbered from 0 to n) and returns the total perimeter of all the squares.

```
perimeter(5)  should return 80
perimeter(7)  should return 216
```

#### Solution

```js
function perimeter(n) {
  if (n === 0) return 4;
  if (n === 1) return 8;

  const fib = [1, 1];
  for (let i = 2; i <= n; i++) {
    fib.push(fib[i - 1] + fib[i - 2]);
  }
  return fib.reduce((acc, cur) => (acc += cur * 4), 0);
}
```

## [6kyu](#katas)

### [Numericals of a String](#katas)

You are given an input string.

For each symbol in the string if it's the first character occurence, replace it with a '1', else replace it with the amount of times you've already seen it...

But will your code be performant enough?

Examples:

```js
input = "Hello, World!";
result = "1112111121311";

input = "aaaaaaaaaaaa";
result = "123456789101112";
```

#### Solution

```js
function numericals(s) {
  let res = "",
    chars = {};
  for (let i = 0; i < s.length; i++) {
    let cur = s[i];
    if (chars.hasOwnProperty(cur)) {
      chars[cur] += 1;
    } else {
      chars[cur] = 1;
    }
    res += chars[cur];
  }
  return res;
}
```

### [Tic-Tac-Toe-like table Generator](#katas)

Do you have in mind the good old TicTacToe?

Assuming that you get all the data in one array, you put a space around each value, `|` as a columns separator and multiple `-` as rows separator, with something like `["O", "X", " ", " ", "X", " ", "X", "O", " "]` you should be returning this structure (inclusive of new lines):

```
 O | X |
-----------
   | X |
-----------
 X | O |
```

Now, to spice up things a bit, we are going to expand our board well beyond a trivial `3` x `3` square and we will accept rectangles of big sizes, still all as a long linear array.

For example, for `"O", "X", " ", " ", "X", " ", "X", "O", " ", "O"]` (same as above, just one extra `"O"`) and knowing that the length of each row is `5`, you will be returning

```
 O | X |   |   | X
-------------------
   | X | O |   | O
```

And worry not about missing elements, as the array/list/vector lenght is always going to be a multiple of the width.

#### Solution

```js
function displayBoard(board, width) {
  let result = "";
  for (let i = 0; i < board.length; i++) {
    if (i > 0 && i % width === 0) {
      result += "---".repeat(width) + "-".repeat(width - 1) + "\n";
    }

    result += " " + board[i] + " ";

    if (i + 1 < board.length) {
      if ((i + 1) % width === 0) result += "\n";
      else result += "|";
    }
  }
  return result;
}
```

### [Highest Scoring Word](#katas)

Given a string of words, you need to find the highest scoring word.

Each letter of a word scores points according to its position in the alphabet: a = 1, b = 2, c = 3 etc.

You need to return the highest scoring word as a string.

If two words score the same, return the word that appears earliest in the original string.

All letters will be lowercase and all inputs will be valid.

#### Solution

```js
function high(x) {
  const arr = x.split(" ");
  const scores = [];
  arr.map(w => {
    let wordScore = 0;
    for (let i = 0; i < w.length; i++) {
      wordScore += w[i].charCodeAt() - 96;
    }
    scores.push(wordScore);
    wordScore = 0;
  });
  return arr[scores.indexOf(Math.max(...scores))];
}
```

### [CamelCase Method](#katas)

Write simple .camelCase method (camel_case function in PHP, CamelCase in C# or camelCase in Java) for strings. All words must have their first letter capitalized without spaces.

For instance:

```js
"hello case".camelCase() => HelloCase
"camel case word".camelCase() => CamelCaseWord
```

#### Solution

```js
String.prototype.camelCase = function() {
  return this.split(" ")
    .map(w => w.slice(0, 1).toUpperCase() + w.slice(1))
    .join("");
};
```

### [Unique In Order](#katas)

Implement the function unique_in_order which takes as argument a sequence and returns a list of items without any elements with the same value next to each other and preserving the original order of elements.

For example:

```js
uniqueInOrder('AAAABBBCCDAABBB') == ['A', 'B', 'C', 'D', 'A', 'B']
uniqueInOrder('ABBCcAD')         == ['A', 'B', 'C', 'c', 'A', 'D']
uniqueInOrder([1,2,2,3,3])       == [1,2,3]
```

#### Solution

```js
var uniqueInOrder=function(iterable){
  const arr = iterable.constructor === Array ? iterable : iterable.split('');
  return arr.filter((el, i, array) =>  el !== array[i+1]);
}
```

### [Evil Autocorrect Prank](#katas)

Your friend won't stop texting his girlfriend. It's all he does. All day. Seriously. The texts are so mushy too! The whole situation just makes you feel ill. Being the wonderful friend that you are, you hatch an evil plot. While he's sleeping, you take his phone and change the autocorrect options so that every time he types "you" or "u" it gets changed to "your sister."

Write a function called `autocorrect` that takes a string and replaces all instances of `"you"` or `"u"` (not case sensitive) with `"your sister"` (always lower case).

Return the resulting string.

Here's the slightly tricky part: These are text messages, so there are different forms of "you" and "u".

For the purposes of this kata, here's what you need to support:

- "youuuuu" with any number of u characters tacked onto the end
- "u" at the beginning, middle, or end of a string, but NOT part of a word
- "you" but NOT as part of another word like youtube or bayou

#### Solution

```js
function autocorrect(input){
  return input.replace(/(?<!\w)(u|you+)(?!\w)/gi, 'your sister');
}
```

### [The Vowel Code](#katas)

Step 1: Create a function called encode() to replace all the lowercase vowels in a given string with numbers according to the following pattern:

a -> 1

e -> 2

i -> 3

o -> 4

u -> 5

For example, encode("hello") would return "h2ll4" There is no need to worry about uppercase vowels in this kata.

Step 2: Now create a function called decode() to turn the numbers back into vowels according to the same pattern shown above.

For example, decode("h3 th2r2") would return "hi there"

For the sake of simplicity, you can assume that any numbers passed into the function will correspond to vowels.

#### Solution

```js
const vowels = 'aeiou';
function encode(string) {
  return string.replace(/[aeiou]/g, match => {
    if (vowels.indexOf(match) > -1)
      return vowels.indexOf(match) + 1;
  });
}

function decode(string) {
  return string.replace(/[12345]/g, match => {    
    if(!Number.isNaN(match))
      return vowels[match - 1];
  });
}
```
### [Simple Time Difference](#katas)

In this Kata, you will be given a series of times at which an alarm goes off. Your task will be to determine the maximum time interval between alarms. Each alarm starts ringing at the beginning of the corresponding minute and rings for exactly one minute. The times in the array are not in chronological order. Ignore duplicate times, if any.

For example:
```
solve(["14:51"]) = "23:59". If the alarm goes off now, it will not go off for another 23 hours and 59 minutes.
solve(["23:00","04:22","18:05","06:24"]) == "11:40". The max interval that the alarm will not go off is 11 hours and 40 minutes.
```
In the second example, the alarm goes off 4 times in a day.

#### Solution

```js
function getMinutes(time) {
	const [h, m] = time.split(':');
	return Number(h) * 60 + Number(m);
}

function getTime(minutes) {
	const h = Math.floor(minutes / 60);
	const m = minutes % 60;
	return `${h > 9 ? h : '0' + h}:${m > 9 ? m : '0' + m}`;
}

function solve(arr) {
	const minutes = [];
	for (let i = 0; i < arr.length; i++) {
		minutes.push(getMinutes(arr[i]))
	}

	minutes.sort((a, b) => a - b);
	
	const intervals = [];
	for (let i = 1; i < minutes.length; i++) {
		intervals.push(minutes[i] - minutes[i - 1] - 1)
	}

	const DAY_IN_MINUTES = 24 * 60;
	intervals.unshift(minutes[0] + DAY_IN_MINUTES - minutes[minutes.length - 1] - 1);

	const maxMinutes = Math.max(...intervals);
	return getTime(maxMinutes);
}
```

## [7kyu](#katas)

### [Drying Potatoes](#katas)

All we eat is water and dry matter.

John bought potatoes: their weight is 100 kilograms. Potatoes contain water and dry matter.

The water content is 99 percent of the total weight. He thinks they are too wet and puts them in an oven - at low temperature - for them to lose some water.

At the output the water content is only 98%.

What is the total weight in kilograms (water content plus dry matter) coming out of the oven?

He finds 50 kilograms and he thinks he made a mistake: "So much weight lost for such a small change in water content!"

Can you help him?

Write function `potatoes` with

int parameter `p0` - initial percent of water-
int parameter `w0` - initial weight -
int parameter `p1` - final percent of water -

`potatoes` should return the final weight coming out of the oven `w1` truncated as an int.

Example:

`potatoes(99, 100, 98) --> 50`

#### Solution

```js
function potatoes(p0, w0, p1) {
  const nonWater = ((p0 - p1) * w0) / 100;
  const water = (p1 * w0) / 100;
  return Math.floor(w0 - nonWater / ((w0 - water) / w0));
}
```

### [Sum of Odd Numbers](#katas)

Given the triangle of consecutive odd numbers:

```
             1
          3     5
       7     9    11
   13    15    17    19
21    23    25    27    29
...
```

Calculate the row sums of this triangle from the row index (starting at index 1) e.g.:

```js
rowSumOddNumbers(1); // 1
rowSumOddNumbers(2); // 3 + 5 = 8
```

#### Solution

```js
function rowSumOddNumbers(n) {
  return (n * (n - 1) + 1) * n + n * (n - 1);
}
```

### [VAPORCODE](#katas)

ASC Week 1 Challenge 4 (Medium #1)

Write a function that converts any sentence into a V A P O R W A V E sentence. a V A P O R W A V E sentence converts all the letters into uppercase, and adds 2 spaces between each letter (or special character) to create this V A P O R W A V E effect.

Examples:

```js
vaporcode("Lets go to the movies") // output => "L  E  T  S  G  O  T  O  T  H  E  M  O  V  I  E  S"
vaporcode("Why isn't my code working?") // output => "W  H  Y  I  S  N  '  T  M  Y  C  O  D  E  W  O  R  K  I  N  G  ?"
```

#### Solution:

```js
function vaporcode(s) {
	return s.toUpperCase().replace(/\s/g, '').split('').join('  ');
}
```

### [The First Non Repeated Character In A String](#katas)

You need to write a function, that returns the first non-repeated character in the given string.

For example for string `"test"` function should return `'e'`.
For string `"teeter"` function should return `'r'`.

If a string contains all unique characters, then return just the first character of the string.
Example: for input `"trend"` function should return `'t'`

You can assume, that the input string has always non-zero length.

If there is no repeating character, return `null` in JS or Java, and `None` in Python.

#### Solution

```js
function firstNonRepeated(s) {
  for(let i = 0; i < s.length; i++) {
    const idx = s.indexOf(s[i]);
    if(s.indexOf(s[i], idx + 1) === -1) return s[i];
  }
  return null;
}
```

### [Fun with lists: length](#katas)

Implement the method **length**, which accepts a linked list (head), and returns the length of the list.

For example: Given the list: `1 -> 2 -> 3 -> 4`, **length** should return 4.

The linked list is defined as follows:

```js
function Node(data, next = null) {
  this.data = data;
  this.next = next;
}
```

Note: the list may be null and can hold any type of value.

#### Solution

```js
function length(head) {  
  let len = 0;
  while(head) {
    head = head ? head.next : null;
    len++;
  }
  
  return len;
}
```


