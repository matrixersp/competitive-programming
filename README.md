# Codewars Kata Solutions on JavaScript

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