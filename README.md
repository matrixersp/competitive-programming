# Codewars Kata Solutions on JavaScript

## Katas

- [4kyu](#4kyu)
  - [Next bigger number with the same digits](#next-bigger-number-with-the-same-digits)
- [5kyu](#5kyu)
  - [Did you mean ...?](#did-you-mean-)
  - [Perimeter of squares in a rectangle](#Perimeter-of-squares-in-a-rectangle)
  - [flatten()](#flatten)
  - [First non-repeating character](#first-non-repeating-character)
  - [What's a Perfect Power anyway?](#whats-a-perfect-power-anyway)
  - [RGB To Hex Conversion](#rgb-to-hex-conversion)
  - [Convert A Hex String To RGB](#convert-a-hex-string-to-rgb)
- [6kyu](#6kyu)
  - [Numericals of a String](#numericals-of-a-string)
  - [Tic-Tac-Toe-like table Generator](#tic-tac-toe-like-table-generator)
  - [Highest Scoring Word](#highest-scoring-word)
  - [CamelCase Method](#camelcase-method)
  - [Unique In Order](#unique-in-order)
  - [Evil Autocorrect Prank](#evil-autocorrect-prank)
  - [The Vowel Code](#the-vowel-code)
  - [Simple Time Difference](#simple-time-difference)
  - [Character with longest consecutive repetition](#character-with-longest-consecutive-repetition)
  - [Encrypt this!](#encrypt-this)
  - [Find within array](#find-within-array)
  - [Delete occurrences of an element if it occurs more than n times](#delete-occurrences-of-an-element-)
  - [I need more speed!](#i-need-more-speed)

- [7kyu](#7kyu)
  - [Drying Potatoes](#drying-potatoes)
  - [Sum of Odd Numbers](#sum-of-odd-numbers)
  - [VAPORCODE](#vaporcode)
  - [The First Non Repeated Character In A String](#the-first-non-repeated-character-in-a-string)
  - [Fun with lists: length](#fun-with-lists-length)
  - [Sort the Gift Code](#sort-the-gift-code)
  - [My Languages](#my-languages)
  - [Largest 5 digit number in a series](#largest-5-digit-number-in-a-series)
  - [Greet Me](#greet-me)
  - [Halving Sum](#halving-sum)
  - [The reject() function](#the-reject-function)
  - [Largest pair sum in array](#largest-pair-sum-in-array)
  - [Find min and max](#find-min-and-max)
  - [Sum even numbers](#sum-even-numbers)
  - [Round up to the next multiple of 5](#round-up-to-the-next-multiple-of-5)

## [4kyu](#katas)

### [Next bigger number with the same digits](#katas)
Create a function that takes a positive integer and returns the next bigger number that can be formed by rearranging its digits. For example:

```
12 ==> 21
513 ==> 531
2017 ==> 2071
```

```js
nextBigger(num: 12)   // returns 21
nextBigger(num: 513)  // returns 531
nextBigger(num: 2017) // returns 2071
```
If the digits can't be rearranged to form a bigger number, return -1 (or nil in Swift):

```
9 ==> -1
111 ==> -1
531 ==> -1
```
```js
nextBigger(num: 9)   // returns nil
nextBigger(num: 111) // returns nil
nextBigger(num: 531) // returns nil
```

#### solution

```js
function nextBigger(n) {
  const str = String(n);
  const len = str.length;

  if (len === 1) return -1;

  for (let i = len - 2; i >= 0; i--) {
    let index = findNextBiggerDigit(str, i);
    if (index > -1) {
      let arr = str.split("");
      arr[i] = str[index];
      arr[index] = str[i];
      return parseInt(
        arr
          .slice(0, i + 1)
          .concat(arr.slice(i + 1, arr.length).sort())
          .join("")
      );
    }
  }
  return -1;
}

function findNextBiggerDigit(str, i) {
  if (i === str.length - 2) {
    if (str[i] < str[i + 1]) return i + 1;
    else return -1;
  }

  for (let j = +str[i] + 1; j <= 9; j++) {
    let index = str.indexOf(j, i + 1);
    if (index > -1) return index;
  }
}
```

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
  "raspberry",
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
  "coffeescript",
]);
languages.findMostSimilar("heaven"); // must return "java"
languages.findMostSimilar("javascript"); // must return "javascript" (same words are obviously the most similar ones)
```

#### Solution

```js
function Dictionary(words) {
  this.words = words;
}

Dictionary.prototype.findMostSimilar = function (term) {
  const arr = [];
  for (let i = 0; i < this.words.length; i++) {
    let count = 0,
      nextIdx = 0;
    this.words[i].split("").forEach((l) => {
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

### [flatten()](#katas)

For this exercise you will create a global flatten method. The method takes in any number of arguments and flattens them into a single array. If any of the arguments passed in are an array then the individual objects within the array will be flattened so that they exist at the same level as the other arguments. Any nested arrays, no matter how deep, should be flattened into the single array result.

The following are examples of how this function would be used and what the expected results would be:

```js
flatten(1, [2, 3], 4, 5, [6, [7]]); // returns [1, 2, 3, 4, 5, 6, 7]
flatten("a", ["b", 2], 3, null, [[4], ["c"]]); // returns ['a', 'b', 2, 3, null, 4, 'c']
```

#### Solution

```js
function flatten(...args) {
  let isFlattened = false;
  let arr = [...args];
  while (!isFlattened) {
    isFlattened = true;
    for (let i = 0; i < arr.length; i++) {
      if (arr[i] && arr[i].constructor === Array) {
        arr.splice(i, 1, ...arr[i]);
        isFlattened = false;
      } else {
        arr.splice(i, 1, arr[i]);
      }
    }
  }
  return arr;
}
```

### [First non-repeating character](#katas)

Write a function named `first_non_repeating_letter` that takes a string input, and returns the first character that is not repeated anywhere in the string.

For example, if given the input `'stress'`, the function should return `'t'`, since the letter t only occurs once in the string, and occurs first in the string.

As an added challenge, upper- and lowercase letters are considered the same character, but the function should return the correct case for the initial letter. For example, the input `'sTreSS'` should return `'T'`.

If a string contains all repeating characters, it should return an empty string (`""`) or `None` -- see sample tests.

#### Solution

```js
function firstNonRepeatingLetter(s) {
  return (
    s.split("").find((l) => {
      const re = new RegExp(l, "gi");
      return s.match(re).length === 1;
    }) || ""
  );
}
```

### [What's a Perfect Power anyway?](#katas)

A [perfect power](https://en.wikipedia.org/wiki/Perfect_power) is a classification of positive integers:

> In mathematics, a perfect power is a positive integer that can be expressed as an integer power of another positive integer. More formally, n is a perfect power if there exist natural numbers m > 1, and k > 1 such that mk = n.

Your task is to check wheter a given integer is a perfect power. If it is a perfect power, return a pair `m` and `k` with mk = n as a proof. Otherwise return `Nothing`, `Nil`, `null`, `NULL`, `None` or your language's equivalent.

Note: For a perfect power, there might be several pairs. For example `81 = 3^4 = 9^2`, so `(3,4)` and `(9,2)` are valid solutions. However, the tests take care of this, so if a number is a perfect power, return any pair that proves it.

Examples:

```js
Test.describe("perfect powers", function () {
  Test.it("should work for some examples", function () {
    Test.assertSimilar(isPP(4), [2, 2], "4 = 2^2");
    Test.assertSimilar(isPP(9), [3, 2], "9 = 3^2");
    Test.assertEquals(isPP(5), null, "5 isn't a perfect number");
  });
});
```

#### Solution

```js
function isPP(n) {
  let m = 2,
    k = 2;
  while (Math.pow(m, k) <= n) {
    while (Math.pow(m, k) < n) k++;
    if (Math.pow(m, k) === n) return [m, k];
    k = 2;
    m++;
  }
  return null;
}
```

### [RGB To Hex Conversion](#katas)

The rgb function is incomplete. Complete it so that passing in RGB decimal values will result in a hexadecimal representation being returned. Valid decimal values for RGB are 0 - 255. Any values that fall out of that range must be rounded to the closest valid value.

Note: Your answer should always be 6 characters long, the shorthand with 3 will not work here.

The following are examples of expected output values:

```js
rgb(255, 255, 255); // returns FFFFFF
rgb(255, 255, 300); // returns FFFFFF
rgb(0, 0, 0); // returns 000000
rgb(148, 0, 211); // returns 9400D3
```

#### Solution

```js
function rgb(r, g, b) {
  function hex(n) {
    if (n >= 255) return "FF";
    else if (n <= 0) return "00";
    else return n.toString(16).toUpperCase().padStart(2, 0);
  }
  return hex(r) + hex(g) + hex(b);
}
```

### [Convert a hex string to RGB](#katas)

When working with color values it can sometimes be useful to extract the individual red, green, and blue (RGB) component values for a color. Implement a function that meets these requirements:

- Accepts a case-insensitive hexadecimal color string as its parameter (ex. `"#FF9933"` or `"#ff9933"`)
- Returns an object with the structure `{r: 255, g: 153, b: 51}` where r, g, and b range from 0 through 255

**Note**: your implementation does not need to support the shorthand form of hexadecimal notation (ie `"#FFF"`)

Example:

```
"#FF9933" --> {r: 255, g: 153, b: 51}
```

#### Solution

```js
function hexStringToRGB(hexString) {
  let rgb = [];
  for(let i = 1; i < hexString.length - 1; i+=2) {
    rgb.push(parseInt(hexString.substring(i, i+2), 16));
  }
  return {r: rgb[0], g: rgb[1], b: rgb[2]}
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
  arr.map((w) => {
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
String.prototype.camelCase = function () {
  return this.split(" ")
    .map((w) => w.slice(0, 1).toUpperCase() + w.slice(1))
    .join("");
};
```

### [Unique In Order](#katas)

Implement the function unique_in_order which takes as argument a sequence and returns a list of items without any elements with the same value next to each other and preserving the original order of elements.

For example:

```js
uniqueInOrder("AAAABBBCCDAABBB") == ["A", "B", "C", "D", "A", "B"];
uniqueInOrder("ABBCcAD") == ["A", "B", "C", "c", "A", "D"];
uniqueInOrder([1, 2, 2, 3, 3]) == [1, 2, 3];
```

#### Solution

```js
var uniqueInOrder = function (iterable) {
  const arr = iterable.constructor === Array ? iterable : iterable.split("");
  return arr.filter((el, i, array) => el !== array[i + 1]);
};
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
function autocorrect(input) {
  return input.replace(/(?<!\w)(u|you+)(?!\w)/gi, "your sister");
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
const vowels = "aeiou";
function encode(string) {
  return string.replace(/[aeiou]/g, (match) => {
    if (vowels.indexOf(match) > -1) return vowels.indexOf(match) + 1;
  });
}

function decode(string) {
  return string.replace(/[12345]/g, (match) => {
    if (!Number.isNaN(match)) return vowels[match - 1];
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
  const [h, m] = time.split(":");
  return Number(h) * 60 + Number(m);
}

function getTime(minutes) {
  const h = Math.floor(minutes / 60);
  const m = minutes % 60;
  return `${h > 9 ? h : "0" + h}:${m > 9 ? m : "0" + m}`;
}

function solve(arr) {
  const minutes = [];
  for (let i = 0; i < arr.length; i++) {
    minutes.push(getMinutes(arr[i]));
  }

  minutes.sort((a, b) => a - b);

  const intervals = [];
  for (let i = 1; i < minutes.length; i++) {
    intervals.push(minutes[i] - minutes[i - 1] - 1);
  }

  const DAY_IN_MINUTES = 24 * 60;
  intervals.unshift(
    minutes[0] + DAY_IN_MINUTES - minutes[minutes.length - 1] - 1
  );

  const maxMinutes = Math.max(...intervals);
  return getTime(maxMinutes);
}
```

### [Character with longest consecutive repetition](#katas)

For a given string `s` find the character `c` (or `C`) with longest consecutive repetition and return:

```js
[c, l];
```

where `l` (or `L`) is the length of the repetition. If there are two or more characters with the same `l` return the first.

For empty string return:

```js
["", 0];
```

#### Solution

```js
function longestRepetition(s) {
  if (!s) return ["", 0];
  const re = /(.)\1*/g;
  const matches = s.match(re);
  const arr = matches.map((l) => l.length);
  const max = Math.max(...arr);
  return [matches[arr.indexOf(max)][0], max];
}
```

### [Encrypt this!](#katas)

Description:

You want to create secret messages which can be deciphered by the Decipher this! kata. Here are the conditions:

Your message is a string containing space separated words.
You need to encrypt each word in the message using the following rules:
The first letter needs to be converted to its ASCII code.
The second letter needs to be switched with the last letter
Keepin' it simple: There are no special characters in input.

Examples:

```js
encryptThis("Hello") === "72olle"
encryptThis("good") === "103doo"
encryptThis("hello world") === "104olle 119drlo"
```

#### Solution

```js
var encryptThis = function (text) {
	return text.split(' ').map(w => {
		return w.charCodeAt(0) +
			(w.length > 2
				? w[w.length - 1] + w.slice(2, w.length - 1) + w[1]
				: w.length > 1 && w[1]);
	}).join(' ');
}
```

### [Find within array](#katas)

We'll create a function that takes in two parameters:

- a sequence (length and types of items are irrelevant)
- a function (value, index) that will be called on members of the sequence and their index. The function will return either true or false.

Your function will iterate through the members of the sequence in order until the provided function returns true; at which point your function will return that item's index.

If the function given returns false for all members of the sequence, your function should return -1.

```js
var trueIfEven = function(value, index) { return (value % 2 === 0) };
findInArray([1,3,5,6,7], trueIfEven) // should === 3
```

#### Solution

```js
var findInArray = function(arr, iterator) {
  for(let i = 0; i < arr.length; i++) {
    if(iterator(arr[i], i)) {
      return i;
    };
  }
  
  return -1;
};
```

### [Delete occurrences of an element if it occurs more than n times](#katas)
Enough is enough!

Alice and Bob were on a holiday. Both of them took many pictures of the places they've been, and now they want to show Charlie their entire collection. However, Charlie doesn't like this sessions, since the motive usually repeats. He isn't fond of seeing the Eiffel tower 40 times. He tells them that he will only sit during the session if they show the same motive at most N times. Luckily, Alice and Bob are able to encode the motive as a number. Can you help them to remove numbers such that their list contains each number only up to N times, without changing the order?

Task

Given a list lst and a number N, create a new list that contains each number of lst at most N times without reordering. For example if N = 2, and the input is [1,2,3,1,2,1,2,3], you take [1,2,3,1,2], drop the next [1,2] since this would lead to 1 and 2 being in the result 3 times, and then take 3, which leads to [1,2,3,1,2,3].

Example:
```
deleteNth ([1,1,1,1],2) // return [1,1]
deleteNth ([20,37,20,21],1) // return [20,37,21]
```
#### Solution

```js
function deleteNth(arr,n){
  let obj = {};
  let newArr = [];
  for(let i = 0; i < arr.length; i++) {
    if(obj[arr[i]] && obj[arr[i]] < n) {
      obj[arr[i]]++;
      newArr.push(arr[i]);
    }
    else if (!obj[arr[i]]) {
      obj[arr[i]] = 1;
      newArr.push(arr[i]);
    }
  }
  
  return newArr;
}
```

### [I need more speed!](#katas)

Write a function that will take in any array and reverse it.

Sounds simple doesn't it?

**NOTES**:

Array should be reversed in place! (no need to return it)
Usual builtins have been deactivated. Don't count on them.
You'll have to do it fast enough, so think about performances

#### Solution

```js
function reverse(arr) {
   let len = arr.length;
   for(let i = 0; i < Math.floor(arr.length / 2); i++) {
    let cur = arr[i];
    arr[i] = arr[len - i - 1];
    arr[len - i - 1] = cur;
   }
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
vaporcode("Lets go to the movies"); // output => "L  E  T  S  G  O  T  O  T  H  E  M  O  V  I  E  S"
vaporcode("Why isn't my code working?"); // output => "W  H  Y  I  S  N  '  T  M  Y  C  O  D  E  W  O  R  K  I  N  G  ?"
```

#### Solution:

```js
function vaporcode(s) {
  return s.toUpperCase().replace(/\s/g, "").split("").join("  ");
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
  for (let i = 0; i < s.length; i++) {
    const idx = s.indexOf(s[i]);
    if (s.indexOf(s[i], idx + 1) === -1) return s[i];
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
  while (head) {
    head = head ? head.next : null;
    len++;
  }

  return len;
}
```

### [Sort the Gift Code](#katas)

Santa's senior gift organizer Elf developed a way to represent up to 26 gifts by assigning a unique alphabetical character to each gift. After each gift was assigned a character, the gift organizer Elf then joined the characters to form the gift ordering code.

Santa asked his organizer to order the characters in alphabetical order, but the Elf fell asleep from consuming too much hot chocolate and candy canes! Can you help him out?

Sort the Gift Code
Write a function called sortGiftCode/sort_gift_code/SortGiftCode that accepts a string containing up to 26 unique alphabetical characters, and returns a string containing the same characters in alphabetical order.

Examples:

```js
sortGiftCode("abcdef"); //=> returns 'abcdef'
sortGiftCode("pqksuvy"); //=> returns 'kpqsuvy'
sortGiftCode("zyxwvutsrqponmlkjihgfedcba"); //=> returns 'abcdefghijklmnopqrstuvwxyz'
```

#### Solution

```js
function sortGiftCode(code) {
  return code.split("").sort().join("");
}
```

### [My Languages](#katas)

You are given a dictionary/hash/object containing some languages and your test results in the given languages. Return the list of languages where your test score is at least `60`, in descending order of the results.

Note: the scores will always be unique (so no duplicate values)

Examples:

```js
{"Java": 10, "Ruby": 80, "Python": 65}    -->  ["Ruby", "Python"]
{"Hindi": 60, "Dutch" : 93, "Greek": 71}  -->  ["Dutch", "Greek", "Hindi"]
{"C++": 50, "ASM": 10, "Haskell": 20}     -->  []
```

#### Solution

```js
function myLanguages(langs) {
  return Object.entries(langs)
    .sort((a, b) => b[1] - a[1])
    .filter((a) => a[1] >= 60)
    .map((a) => a[0]);
}
```

### [Largest 5 digit number in a series](#katas)
In the following 6 digit number:

```
283910
```
`91` is the greatest sequence of 2 consecutive digits.

In the following 10 digit number:

```
1234567890
```
`67890` is the greatest sequence of 5 consecutive digits.

Complete the solution so that it returns the greatest sequence of five consecutive digits found within the number given. The number will be passed in as a string of only digits. It should return a five digit integer. The number passed may be as large as 1000 digits.

Adapted from ProjectEuler.net

#### Solution 

```js
function solution(digits){
  max = +digits.slice(0, 5);
  for(let i = 0; i < digits.length; i++) {
    if(digits.slice(i, i + 5) > max) max = +digits.slice(i, i + 5);
  }
  return max;
}
```


### [Greet Me](#katas)

Write a method that takes one argument as name and then greets that name, capitalized and ends with an exclamation point.

Example:

```
"riley" --> "Hello Riley!"
"JACK"  --> "Hello Jack!"
```

#### Solution

```js
var greet = function (n) {
  return 'Hello ' + n.slice(0, 1).toUpperCase() + n.slice(1).toLowerCase() + '!';
};
```

### [Halving Sum](#katas)

Given a positive integer n, calculate the following sum:

```
n + n/2 + n/4 + n/8 + ...
```
All elements of the sum are the results of integer division.

Example:

```
25  =>  25 + 12 + 6 + 3 + 1 = 47
```

#### Solution

```js
function halvingSum(n) {
  let sum = n;
  for(let i = 2; i <= n; i *= 2) {
    sum += parseInt(n / i);
  }
  
  return sum;
}
```

### [The reject() function](#katas)

Implement a function which filters out array values which satisfy the given predicate.

```
reject([1, 2, 3, 4, 5, 6], (n) => n % 2 === 0)  =>  [1, 3, 5]
```

#### Solution

```js
const reject = (a, p) => a.filter(v => !p(v));
```

### [Largest pair sum in array](#katas)
Given a sequence of numbers, find the largest pair sum in the sequence.

For example

```
[10, 14, 2, 23, 19] --> 42 (i.e. sum of 23 and 19)
[99, 2, 2, 23, 19]  --> 122 (i.e. sum of 99 and 23)
```
Input sequence contains minimum two elements and every element is an integer.

#### Solution

```js
function largestPairSum(numbers)
{
  let max = numbers[0] + numbers[1];
  for(let i = 0; i < numbers.length; i++) {
    for(let j = i + 1; j < numbers.length; j++) {
      if(numbers[i] + numbers[j] > max) max = numbers[i] + numbers[j];   
    }
  }
  
  return max;
}
```

### [Find min and max](#katas)

Implement a function that returns the minimal and the maximal value of a list (in this order).

#### Solution
```js
function getMinMax(arr){
  let min = arr[0];
  let max = arr[0];
  
  arr.forEach(val => {
    if(val > max) max = val;
    if(val < min) min = val;
  });
  
  return [min, max];
};
```
### [Sum even numbers](#katas)
Write a function named sumEvenNumbers, taking a sequence of numbers as single parameter. Your function must return the sum of the even values of this sequence.

Only numbers without decimals like 4 or 4.0 can be even.

**Input**  
sequence of numbers: those numbers could be integers and/or floats.
For example, considering this input value : `[4,3,1,2,5,10,6,7,9,8]`, then your function should return `30` (because `4 + 2 + 10 + 6 + 8 = 30`).

#### Solution
```js
function sumEvenNumbers(input) {
  return input.reduce((acc, cur) => acc += cur % 2 === 0 ? cur : 0, 0);
}
```

### [Round up to the next multiple of 5](#katas)

Given an integer as input, can you round it to the next (meaning, "higher") 5?

Examples:
```
input:    output:
0    ->   0
2    ->   5
3    ->   5
12   ->   15
21   ->   25
30   ->   30
-2   ->   0
-5   ->   -5
etc.
```
Input may be any positive or negative integer (including 0).

You can assume that all inputs are valid integers.

#### Solution

```js
function roundToNext5(n){
  if(n % 5 === 0) return n;
  if((n + 1) % 5 === 0) return n + 1;
  if((n + 2) % 5 === 0) return n + 2;
  if((n + 3) % 5 === 0) return n + 3;
  if((n + 4) % 5 === 0) return n + 4;
}
```