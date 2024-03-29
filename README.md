# Codewars Kata Solutions in JavaScript
## Katas
### [4kyu](#katas)

<details>
 <summary><strong>Next bigger number with the same digits</strong> (click to show)</summary>

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

</details>

### [5kyu](#katas)

<details>
<summary><strong>Did you mean ...?</strong> (click to show)</summary>


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

</details>

<details>
<summary><strong>Perimeter of squares in a rectangle</strong> (click to show)</summary>

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

</details>

<details>
<summary><strong>flatten()</strong> (click to show)</summary>

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

</details>

<details>
<summary><strong>First non-repeating character</strong> (click to show)</summary>

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

</details>

<details>
<summary><strong>What's a Perfect Power anyway?</strong> (click to show)</summary>

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

</details>

<details>
<summary><strong>RGB To Hex Conversion</strong> (click to show)</summary>

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

</details>

<details>
<summary><strong>Convert a hex string to RGB</strong> (click to show)</summary>

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

</details>

<details>
<summary><strong>Calculating with Functions</strong> (click to show)</summary>

This time we want to write calculations using functions and get the results. Let's have a look at some examples:

```
seven(times(five())); // must return 35
four(plus(nine())); // must return 13
eight(minus(three())); // must return 5
six(dividedBy(two())); // must return 3
```
Requirements:

- There must be a function for each number from 0 ("zero") to 9 ("nine")
- There must be a function for each of the following mathematical operations: plus, minus, times, dividedBy (divided_by in Ruby and Python)
- Each calculation consist of exactly one operation and two numbers
- The most outer function represents the left operand, the most inner function represents the right operand
- Divison should be integer division. For example, this should return 2, not 2.666666...:
```
eight(dividedBy(three()));
```

#### Solution

```js
function zero(o) {
  if(o) return calc(0, o.b, o.op);
  return 0;
}

function one(o) {
  if(o) return calc(1, o.b, o.op);
  return 1;
}

function two(o) {
  if(o) return calc(2, o.b, o.op);
  return 2;
}

function three(o) {
  if(o) return calc(3, o.b, o.op);
  return 3;
}

function four(o) {
  if(o) return calc(4, o.b, o.op);
  return 4;
}

function five(o) {
  if(o) return calc(5, o.b, o.op);
  return 5;
}

function six(o) {
  if(o) return calc(6, o.b, o.op);
  return 6;
}

function seven(o) {
  if(o) return calc(7, o.b, o.op);
  return 7;
}

function eight(o) {
  if(o) return calc(8, o.b, o.op);
  return 8;
}

function nine(o) {
  if(o) return calc(9, o.b, o.op);
  return 9;
}

function plus(b) {
  return {op: '+', b}
}

function minus(b) {
  return {op: '-', b}
}

function times(b) {
  return {op: '*', b}
}

function dividedBy(b) {
  return {op: '/', b}
}

function calc(a, b, op) {
  switch(op) {
    case '+': return Math.floor(a + b);
    case '-': return Math.floor(a - b);
    case '*': return Math.floor(a * b);
    case '/': return Math.floor(a / b);
  }
}
```

</details>

<details>
<summary><strong>Valid Parentheses</strong> (click to show)</summary>

Write a function called that takes a string of parentheses, and determines if the order of the parentheses is valid. The function should return true if the string is valid, and false if it's invalid.

Examples:

```
"()"              =>  true
")(()))"          =>  false
"("               =>  false
"(())((()())())"  =>  true
```

Constraints:

`0 <= input.length <= 100`

#### Solution

```js
function validParentheses(p){
  let sum = 0;
  
  for(let i = 0; i < p.length; i++) {
    if(p[i] === ")") sum--;
    if(p[i] === "(") sum++;
    if(sum < 0) return false;
  }
  if(sum > 0) return false;
  return true;
}
```

</details>

<details>
<summary><strong>Rot13</strong> (click to show)</summary>

ROT13 is a simple letter substitution cipher that replaces a letter with the letter 13 letters after it in the alphabet. ROT13 is an example of the Caesar cipher.

Create a function that takes a string and returns the string ciphered with Rot13. If there are numbers or special characters included in the string, they should be returned as they are. Only letters from the latin/english alphabet should be shifted, like in the original Rot13 "implementation".

### Solution

```js
function rot13(m) {
  let cipher = "";
  let upr = "ABCDEFGHIJKLMNOPQRSTUVWXYZABCDEFGHIJKLM";
  let lwr = "abcdefghijklmnopqrstuvwxyzabcdefghijklm";
  
  for (let i = 0; i < m.length; i++) {
    let u = upr.indexOf(m[i]);
    let l = lwr.indexOf(m[i]);
    if (u > -1) cipher += upr[u + 13];
    else if (l > -1) cipher += lwr[l + 13];
    else cipher += m[i];
  }

  return cipher;
}
```

</details>


<details>
<summary><strong>ISBN-10 Validation</strong> (click to show)</summary>

ISBN-10 identifiers are ten digits. The first nine digits are on the range 0 to 9. The last digit can be either on the range 0 to 9 or the letter 'X' used to indicate a value of 10.

For an ISBN-10 to be valid, the sum of the digits multiplied by their position has to equal zero modulo 11. For example, the ISBN-10: 1112223339 is valid because:

```
(((1*1)+(1*2)+(1*3)+(2*4)+(2*5)+(2*6)+(3*7)+(3*8)+(3*9)+(9*10)) % 11) == 0
```
Complete the validISBN10() function.

```
validISBN10('1112223339') ; should return true
validISBN10('1234554321') ; should return true
validISBN10('1234512345') ; should return false
```

#### Solution

```js
function validISBN10(isbn) {
  let sum = 0;

  for(let i = 0; i < isbn.length - 1; i++) {
    sum += (i + 1) * isbn[i];
  }
  sum += (isbn[isbn.length - 1] === 'X') ? 10 * 10 : 10 * isbn[isbn.length - 1];
  
  return sum % 11 === 0; 
}
```

</details>

<details>
<summary><strong>Count IP Addresses</strong> (click to show)</summary>

Implement a function that receives two IPv4 addresses, and returns the number of addresses between them (including the first one, excluding the last one).

All inputs will be valid IPv4 addresses in the form of strings. The last address will always be greater than the first one.

Examples: 
```
ipsBetween("10.0.0.0", "10.0.0.50")  ===   50 
ipsBetween("10.0.0.0", "10.0.1.0")   ===  256 
ipsBetween("20.0.0.10", "20.0.1.0")  ===  246
```

#### Solution

```js
function ipsBetween(start, end){
  let s = start.split('.');
  let e = end.split('.');
  let sum = 0;
  
  for(let i = 0; i < s.length; i++) {
    sum += (e[i] - s[i]) * Math.pow(256, s.length - (i + 1));
  }
  
  return sum;
}
```

</details>

<details>
<summary><strong>Human Readable Time</strong> (click to show)</summary>

Write a function, which takes a non-negative integer (seconds) as input and returns the time in a human-readable format (HH:MM:SS)

`HH` = hours, padded to 2 digits, range: 00 - 99
`MM` = minutes, padded to 2 digits, range: 00 - 59
`SS` = seconds, padded to 2 digits, range: 00 - 59
The maximum time never exceeds 359999 (`99:59:59`)

You can find some examples in the test fixtures.

#### Solution

```js
function humanReadable(seconds) {
  let h = parseInt(seconds / 3600).toString();
  let m = parseInt((seconds % 3600) / 60).toString();
  let s = parseInt((seconds % 3600) % 60).toString();
  
  return `${h.padStart(2, '0')}:${m.padStart(2, '0')}:${s.padStart(2, '0')}`;
}
```

</details>

<details>
<summary><strong>int32 to IPv4</strong> (click to show)</summary>

Take the following IPv4 address: 128.32.10.1

This address has 4 octets where each octet is a single byte (or 8 bits).

1st octet 128 has the binary representation: 10000000
2nd octet 32 has the binary representation: 00100000
3rd octet 10 has the binary representation: 00001010
4th octet 1 has the binary representation: 00000001
So 128.32.10.1 == 10000000.00100000.00001010.00000001

Because the above IP address has 32 bits, we can represent it as the unsigned 32 bit number: 2149583361

Complete the function that takes an unsigned 32 bit number and returns a string representation of its IPv4 address.

Examples 


```
2149583361 ==> "128.32.10.1"
32         ==> "0.0.0.32"
0          ==> "0.0.0.0"
```

#### Solution

```js
function int32ToIp(int32){
  let b = int32.toString(2)
  if(b.length < 32) return "0.0.0.0";
  
  const toDec = (s) => parseInt(b.slice(s, s + 8), 2);
  return `${toDec(0)}.${toDec(8)}.${toDec(16)}.${toDec(24)}`;
}
```

</details>

<details>
<summary><strong>Directions Reduction</strong> (click to show)</summary>

**Once upon a time, on a way through the old wild mountainous west,…**

… a man was given directions to go from one point to another. The directions were "NORTH", "SOUTH", "WEST", "EAST". Clearly "NORTH" and "SOUTH" are opposite, "WEST" and "EAST" too.

Going to one direction and coming back the opposite direction right away is a needless effort. Since this is the wild west, with dreadfull weather and not much water, it's important to save yourself some energy, otherwise you might die of thirst!

**How I crossed a mountain desert the smart way.**

The directions given to the man are, for example, the following (depending on the language):

```
["NORTH", "SOUTH", "SOUTH", "EAST", "WEST", "NORTH", "WEST"].
```
or

```
{ "NORTH", "SOUTH", "SOUTH", "EAST", "WEST", "NORTH", "WEST" };
```
or

```
[North, South, South, East, West, North, West]
```

You can immediatly see that going "NORTH" and immediately "SOUTH" is not reasonable, better stay to the same place! So the task is to give to the man a simplified version of the plan. A better plan in this case is simply:

```
["WEST"]
```
or

```
{ "WEST" }
```
or

```
[West]
```

**Other examples:**

In `["NORTH", "SOUTH", "EAST", "WEST"]`, the direction `"NORTH" + "SOUTH"` is going north and coming back right away.

The path becomes `["EAST", "WEST"]`, now "EAST" and "WEST" annihilate each other, therefore, the final result is `[]` (nil in Clojure).

In ["NORTH", "EAST", "WEST", "SOUTH", "WEST", "WEST"], "NORTH" and "SOUTH" are not directly opposite but they become directly opposite after the reduction of "EAST" and "WEST" so the whole path is reducible to ["WEST", "WEST"].

**Task**

Write a function `dirReduc` which will take an array of strings and returns an array of strings with the needless directions removed (W<->E or S<->N side by side).

The Haskell version takes a list of directions with data Direction = North | East | West | South.
The Clojure version returns nil when the path is reduced to nothing.
The Rust version takes a slice of enum Direction {NORTH, SOUTH, EAST, WEST}.

**See more examples in "Sample Tests:"**

**Notes**

Not all paths can be made simpler. The path ["NORTH", "WEST", "SOUTH", "EAST"] is not reducible. "NORTH" and "WEST", "WEST" and "SOUTH", "SOUTH" and "EAST" are not directly opposite of each other and can't become such. Hence the result path is itself : ["NORTH", "WEST", "SOUTH", "EAST"].
if you want to translate, please ask before translating.

#### Solution

```js
function dirReduc(arr) {
  let len = -1;

  while(len !== arr.length){
    len = arr.length;
    arr.map((d, i) => {
      if((d == 'SOUTH' && arr[i + 1] == 'NORTH' || d == 'NORTH' && arr[i + 1] == 'SOUTH') || (d == 'EAST' && arr[i + 1] == 'WEST' || d == 'WEST' && arr[i + 1] == 'EAST')) {
        arr.splice(i, 2);
      }
    });
  }

  return arr;
}
```

</details>

<details>
<summary><strong>Simple Pig Latin</strong> (click to show)</summary>

Move the first letter of each word to the end of it, then add "ay" to the end of the word. Leave punctuation marks untouched.

**Examples**
```
pigIt('Pig latin is cool'); // igPay atinlay siay oolcay
pigIt('Hello world !');     // elloHay orldway !
```

#### Solution

```js
function pigIt(str){
  return str.replace(/(\w){1}(\w+)?/g, '$2$1ay');
}
```

</details>

<details>
<summary><strong>Weight for Weight</strong> (click to show)</summary>

My friend John and I are members of the "Fat to Fit Club (FFC)". John is worried because each month a list with the weights of members is published and each month he is the last on the list which means he is the heaviest.

I am the one who establishes the list so I told him: "Don't worry any more, I will modify the order of the list". It was decided to attribute a "weight" to numbers. The weight of a number will be from now on the sum of its digits.

For example 99 will have "weight" 18, 100 will have "weight" 1 so in the list 100 will come before 99. Given a string with the weights of FFC members in normal order can you give this string ordered by "weights" of these numbers?

**Example:**

"56 65 74 100 99 68 86 180 90" ordered by numbers weights becomes: "100 180 90 56 65 74 68 86 99"

When two numbers have the same "weight", let us class them as if they were strings (alphabetical ordering) and not numbers: 100 is before 180 because its "weight" (1) is less than the one of 180 (9) and 180 is before 90 since, having the same "weight" (9), it comes before as a string.

All numbers in the list are positive numbers and the list can be empty.

**Notes**

- it may happen that the input string have leading, trailing whitespaces and more than a unique whitespace between two consecutive numbers
- Don't modify the input
- For C: The result is freed.

#### Solution

```js
const digitSum = x => x.split('').reduce((acc, cur) => acc + +cur, 0);

function orderWeight(str) {
  return str = str.split(' ').sort((a, b) => {
    let x = digitSum(a), y = digitSum(b);
    return x < y || x === y && a === [a, b].sort()[0] ? -1 : 1;
  }).join(' ');
}
```

</details>

<details>
<summary><strong>Moving Zeros to the End</strong> (click to show)</summary>

Write an algorithm that takes an array and moves all of the zeros to the end, preserving the order of the other elements.

```
moveZeros([false,1,0,1,2,0,1,3,"a"]) // returns[false,1,1,2,1,3,"a",0,0]
```

#### Solution

```js
var moveZeros = function (arr) {
  let arr2 = [], count = 0;
  for(let i = 0; i < arr.length; i++) {
    if(arr[i] !== 0) arr2.push(arr[i]);
    else count++;
  }
  return arr2.concat(Array(count).fill(0));
}
```

</details>

<details>
<summary><strong>Regex Password Validation</strong> (click to show)</summary>

You need to write regex that will validate a password to make sure it meets the following criteria:

- At least six characters long
- contains a lowercase letter
- contains an uppercase letter
- contains a number

Valid passwords will only be alphanumeric characters.

#### Solution

```js
function validate(password) {
  return /(?!^[0-9]*$)(?!^[A-Za-z]*$)(?!^[A-Z0-9]*$)(?!^[a-z0-9]*$)^([a-zA-Z0-9]{6,})$/.test(password);
}
```

</details>

### [6kyu](#katas)

<details>
<summary><strong>Numericals of a String</strong> (click to show)</summary>

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

</details>

<details>
<summary><strong>Tic-Tac-Toe-like table Generator</strong> (click to show)</summary>

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

</details>

<details>
<summary><strong>Highest Scoring Word</strong> (click to show)</summary>

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

</details>

<details>
<summary><strong>CamelCase Method</strong> (click to show)</summary>

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

</details>

<details>
<summary><strong>Unique In Order</strong> (click to show)</summary>

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

</details>

<details>
<summary><strong>Evil Autocorrect Prank</strong> (click to show)</summary>

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

</details>

<details>
<summary><strong>The Vowel Code</strong> (click to show)</summary>

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

</details>

<details>
<summary><strong>Simple Time Difference</strong> (click to show)</summary>

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

</details>

<details>
<summary><strong>Character with longest consecutive repetition</strong> (click to show)</summary>

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

</details>

<details>
<summary><strong>Encrypt this!</strong> (click to show)</summary>

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

</details>

<details>
<summary><strong>Find within array</strong> (click to show)</summary>

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

</details>

<details>
<summary><strong>Delete occurrences of an element if it occurs more than n times</strong> (click to show)</summary>

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

</details>

<details>
<summary><strong>I need more speed!</strong> (click to show)</summary>

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

</details>

<details>
<summary><strong>Break camelCase</strong> (click to show)</summary>

Complete the solution so that the function will break up camel casing, using a space between words.

Example:
```
solution("camelCasing")  ==  "camel Casing"
```

#### Solution

```js
function solution(string) {
    return string.replace(/([A-Z])/g , " $1");
}

```

</details>

<details>
<summary><strong>Are they the "same"?</strong> (click to show)</summary>

Given two arrays a and b write a function comp(a, b) (compSame(a, b) in Clojure) that checks whether the two arrays have the "same" elements, with the same multiplicities. "Same" means, here, that the elements in b are the elements in a squared, regardless of the order.

Examples:
```
a = [121, 144, 19, 161, 19, 144, 19, 11]  
b = [121, 14641, 20736, 361, 25921, 361, 20736, 361]
```
comp(a, b) returns true because in b 121 is the square of 11, 14641 is the square of 121, 20736 the square of 144, 361 the square of 19, 25921 the square of 161, and so on. 

Remarks:
a or b might be [] (all languages except R, Shell).
a or b might be nil or null or None or nothing (except in Haskell, Elixir, C++, Rust, R, Shell, PureScript).
If a or b are nil (or null or None), the problem doesn't make sense so return false.

Note for C:
The two arrays have the same size (> 0) given as parameter in function comp.

#### Solution

```js
function comp(arr1, arr2){
  if(!arr1 || !arr2) return false;
  
  for(let i = 0; i < arr1.length; i++) {
    let index = arr2.indexOf(arr1[i] * arr1[i]);
    if(index !== -1) arr2.splice(index, 1);
    else return false;
  }
  
  return true;
} 
```

</details>

<details>
<summary><strong>Counting Duplicates</strong> (click to show)</summary>

Write a function that will return the count of distinct case-insensitive alphabetic characters and numeric digits that occur more than once in the input string. The input string can be assumed to contain only alphabets (both uppercase and lowercase) and numeric digits.

Example:  
"abcde" -> 0 # no characters repeats more than once
"aabbcde" -> 2 # 'a' and 'b'
"aabBcde" -> 2 # 'a' occurs twice and 'b' twice (`b` and `B`)
"indivisibility" -> 1 # 'i' occurs six times
"Indivisibilities" -> 2 # 'i' occurs seven times and 's' occurs twice
"aA11" -> 2 # 'a' and '1'
"ABBA" -> 2 # 'A' and 'B' each occur twice

#### Solution

```js
function duplicateCount(text){
  let str = new Set(text.toLowerCase().split(''));
  let count = 0;
  str.forEach(l => {
    if(text.match(new RegExp(l, 'gi')).length > 1) count++; 
  });
  
  return count;
}
```

</details>

<details>
<summary><strong>Equal Sides of an Array</strong> (click to show)</summary>

You are going to be given an array of integers. Your job is to take that array and find an index N where the sum of the integers to the left of N is equal to the sum of the integers to the right of N. If there is no index that would make this happen, return -1.

For example:

Let's say you are given the array {1,2,3,4,3,2,1}: Your function will return the index 3, because at the 3rd position of the array, the sum of left side of the index ({1,2,3}) and the sum of the right side of the index ({3,2,1}) both equal 6.

Let's look at another one.  
You are given the array {1,100,50,-51,1,1}: Your function will return the index 1, because at the 1st position of the array, the sum of left side of the index ({1}) and the sum of the right side of the index ({50,-51,1,1}) both equal 1.

Last one:  
You are given the array {20,10,-80,10,10,15,35}
At index 0 the left side is {}
The right side is {10,-80,10,10,15,35}
They both are equal to 0 when added. (Empty arrays are equal to 0 in this problem)
Index 0 is the place where the left side and right side are equal.

**Note**:  
If you are given an array with multiple answers, return the lowest correct index.

#### Solution

```js
function findEvenIndex(arr) {
  let ls = 0;
  let rs = arr.reduce((acc, cur) => acc + cur, 0);
  for (let i = 0; i < arr.length; i++) {
    rs -= arr[i];
    if (ls === rs) return i;
    ls += arr[i];
  }

  return -1;
}
```

</details>

<details>
<summary><strong>Meeting</strong> (click to show)</summary>

John has invited some friends. His list is:
```
s = "Fred:Corwill;Wilfred:Corwill;Barney:Tornbull;Betty:Tornbull;Bjon:Tornbull;Raphael:Corwill;Alfred:Corwill";
```
Could you make a program that

- makes this string uppercase
- gives it sorted in alphabetical order by last name.

When the last names are the same, sort them by first name. Last name and first name of a guest come in the result between parentheses separated by a comma.

So the result of function `meeting(s)` will be:
```
"(CORWILL, ALFRED)(CORWILL, FRED)(CORWILL, RAPHAEL)(CORWILL, WILFRED)(TORNBULL, BARNEY)(TORNBULL, BETTY)(TORNBULL, BJON)"
```
It can happen that in two distinct families with the same family name two people have the same first name too.

#### Solution

```js
function meeting(s) {
  return s.toUpperCase().split(';').map(name => name.replace(/(\w+):(\w+)/, '($2, $1)')).sort().join('');
}
```

</details>

<details>
<summary><strong>Find the odd int</strong> (click to show)</summary>

Given an array, find the integer that appears an odd number of times.

There will always be only one integer that appears an odd number of times.

#### Solution
```js
function findOdd(arr) {
  for(let i = 0; i < arr.length; i++){
    let count = 0;
    for(let j = 0; j < arr.length; j++){
      if(arr[i] === arr[j]) count++;  
    }
    if(count %2 !== 0) return arr[i]
  }
}
```

</details>

<details>
<summary><strong>Find the Parity Outlier</strong> (click to show)</summary>

You are given an array (which will have a length of at least 3, but could be very large) containing integers. The array is either entirely comprised of odd integers or entirely comprised of even integers except for a single integer N. Write a method that takes the array as an argument and returns this "outlier" N.

Examples:

```
[2, 4, 0, 100, 4, 11, 2602, 36]
Should return: 11 (the only odd number)

[160, 3, 1719, 19, 11, 13, -21]
Should return: 160 (the only even number)
```

#### Solution 

```js
function findOutlier(x){
  if(x[0] % 2 && x[1] % 2 || x[0] % 2 && x[2] % 2 || x[1] % 2 && x[2] % 2)
    return x.find(num => !(num % 2));
  return x.find(num => num % 2);
}
```

</details>

<details>
<summary><strong>Who likes this?</strong> (click to show)</summary>

You probably know the "like" system from Facebook and other pages. People can "like" blog posts, pictures or other items. We want to create the text that should be displayed next to such an item.

Implement a function `likes :: [String] -> String`, which must take in input array, containing the names of people who like an item. It must return the display text as shown in the examples:

```
likes [] -- must be "no one likes this"
likes ["Peter"] -- must be "Peter likes this"
likes ["Jacob", "Alex"] -- must be "Jacob and Alex like this"
likes ["Max", "John", "Mark"] -- must be "Max, John and Mark like this"
likes ["Alex", "Jacob", "Mark", "Max"] -- must be "Alex, Jacob and 2 others like this"
```

For 4 or more names, the number in and 2 others simply increases.

#### Solution

```js
function likes(n) {
  let len = n.length;
  if(len === 0) return 'no one likes this';
  if(len === 1) return `${n[0]} likes this`;
  if(len === 2) return `${n[0]} and ${n[1]} like this`;
  if(len === 3) return `${n[0]}, ${n[1]} and ${n[2]} like this`;
  if(len >= 4) return `${n[0]}, ${n[1]} and ${len - 2} others like this`;
}
```

</details>

<details>
<summary><strong>English beggars</strong> (click to show)</summary>

Born a misinterpretation of [this kata](https://www.codewars.com/kata/simple-fun-number-334-two-beggars-and-gold/), your task here is pretty simple: given an array of values and an amount of beggars, you are supposed to return an array with the sum of what each beggar brings home, assuming they all take regular turns, from the first to the last.

For example: `[1,2,3,4,5]` for `2` beggars will return a result of `[9,6]`, as the first one takes `[1,3,5]`, the second collects `[2,4]`.

The same array with `3` beggars would have in turn have produced a better out come for the second beggar: `[5,7,3]`, as they will respectively take `[1,4]`, `[2,5]` and `[3]`.

Also note that not all beggars have to take the same amount of "offers", meaning that the length of the array is not necessarily a multiple of `n`; length can be even shorter, in which case the last beggars will of course take nothing `(0)`.

Note: in case you don't get why this kata is about English beggars, then you are not familiar on how religiously queues are taken in the kingdom ;)

#### Solution

```js
function beggars(values, n){
  const arr = new Array(n).fill(0);
  let count = 0;
  
  for(let i = 0; i < values.length; i++) {
    if(count < n && count < values.length) {
      arr[count] += values[i];++count;
    }
    if(count >= n) count = 0;
  }
  
  return arr;
}
```

</details>

### [7kyu](#katas)

<details>
<summary><strong>Drying Potatoes</strong> (click to show)</summary>

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

</details>

<details>
<summary><strong>Sum of Odd Numbers</strong> (click to show)</summary>

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

</details>

<details>
<summary><strong>VAPORCODE</strong> (click to show)</summary>

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

</details>

<details>
<summary><strong>The First Non Repeated Character In A String</strong> (click to show)</summary>

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

</details>

<details>
<summary><strong>Fun with lists: length</strong> (click to show)</summary>

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

</details>

<details>
<summary><strong>Sort the Gift Code</strong> (click to show)</summary>

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

</details>

<details>
<summary><strong>My Languages</strong> (click to show)</summary>

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

</details>

<details>
<summary><strong>Largest 5 digit number in a series</strong> (click to show)</summary>

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

</details>

<details>
<summary><strong>Greet Me</strong> (click to show)</summary>

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

</details>

<details>
<summary><strong>Halving Sum</strong> (click to show)</summary>

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

</details>

<details>
<summary><strong>The reject() function</strong> (click to show)</summary>

Implement a function which filters out array values which satisfy the given predicate.

```
reject([1, 2, 3, 4, 5, 6], (n) => n % 2 === 0)  =>  [1, 3, 5]
```

#### Solution

```js
const reject = (a, p) => a.filter(v => !p(v));
```

</details>

<details>
<summary><strong>Largest pair sum in array</strong> (click to show)</summary>

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

</details>

<details>
<summary><strong>Find min and max</strong> (click to show)</summary>

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

</details>

<details>
<summary><strong>Sum even numbers</strong> (click to show)</summary>

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

</details>

<details>
<summary><strong>Round up to the next multiple of 5</strong> (click to show)</summary>

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

</details>

<details>
<summary><strong>Running out of space</strong> (click to show)</summary>

Kevin is noticing his space run out! Write a function that removes the spaces from the values and returns an array showing the space decreasing. For example, running this function on the array ['i', 'have','no','space'] would produce ['i','ihave','ihaveno','ihavenospace'].

#### Solution

```js
function spacey(arr){
  for(let i = 1; i < arr.length; i++)
    arr[i] = arr[i - 1] + arr[i];
  
  return arr;
}
```

</details>