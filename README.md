# Codewars Kata Solutions on JavaScript

## Katas
* [5kyu](#5kyu)
  * [Did you mean ...?](#did-you-mean-...?)
* [6kyu](#6kyu)
  * [Numericals of a String](#numericals-of-a-string)
  * [Tic-Tac-Toe-like table Generator](#tic-tac-toe-like-table-generator)

## 5kyu

### Did you mean ...?

I'm sure, you know Google's "Did you mean ...?", when you entered a search term and mistyped a word. In this kata we want to implement something similar.

You'll get an entered term (lowercase string) and an array of known words (also lowercase strings). Your task is to find out, which word from the dictionary is most similar to the entered one. The similarity is described by the minimum number of letters you have to add, remove or replace in order to get from the entered word to one of the dictionary. The lower the number of required changes, the higher the similarity between each two words.

Same words are obviously the most similar ones. A word that needs one letter to be changed is more similar to another word that needs 2 (or more) letters to be changed. E.g. the mistyped term berr is more similar to beer (1 letter to be replaced) than to barrel (3 letters to be changed in total).

Extend the dictionary in a way, that it is able to return you the most similar word from the list of known words.

#### Examples

```js
fruits = new Dictionary(['cherry', 'pineapple', 'melon', 'strawberry', 'raspberry']);
fruits.findMostSimilar('strawbery'); // must return "strawberry"
fruits.findMostSimilar('berry'); // must return "cherry"

things = new Dictionary(['stars', 'mars', 'wars', 'codec', 'codewars']);
things.findMostSimilar('coddwars'); // must return "codewars"

languages = new Dictionary(['javascript', 'java', 'ruby', 'php', 'python', 'coffeescript']);
languages.findMostSimilar('heaven'); // must return "java"
languages.findMostSimilar('javascript'); // must return "javascript" (same words are obviously the most similar ones)
```


#### Solution

```js
function Dictionary(words) {
  this.words = words;
}

Dictionary.prototype.findMostSimilar = function(term) {
  let arr = [];
  for(let i = 0; i < this.words.length; i++) {
    let count = 0, nextIdx = 0;
    this.words[i].split('').forEach(l => {
      if(term.indexOf(l, nextIdx) > -1) {
        count++;
        nextIdx = term.indexOf(l, nextIdx);
      } else count--;
    })
    arr.push(count)
  }
  return this.words[arr.indexOf(Math.max(...arr))];
}
```

## 6kyu

### Numericals of a String

You are given an input string.

For each symbol in the string if it's the first character occurence, replace it with a '1', else replace it with the amount of times you've already seen it...

But will your code be performant enough?

#### Examples

```js
input   =  "Hello, World!"
result  =  "1112111121311"

input   =  "aaaaaaaaaaaa"
result  =  "123456789101112"
```

#### Solution

```js
function numericals(s){
  let res = '', chars = {};
  for(let i = 0; i < s.length; i++) {
    let cur = s[i];
    if(chars.hasOwnProperty(cur)) {
      chars[cur] += 1;
    } else {
      chars[cur] = 1;
    }
    res += chars[cur];
  }
  return res;
}
```

### Tic-Tac-Toe-like table Generator

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
function displayBoard(board, width){
  let result = '';
  for(let i = 0; i < board.length; i++) {
    if( i > 0 && i % width === 0) {
      result += '---'.repeat(width) + '-'.repeat(width - 1) + '\n';
    }
    
    result += ' ' + board[i] + ' ';
    
    if(i + 1 < board.length) {
      if((i + 1) % width === 0) result += '\n';
      else result += '|' 
    }
  }
  return result;
}
```

