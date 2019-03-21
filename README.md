# how the lexicon helps reading foreign laguange
- The lexicon contains words with familiarity.
```ts
interface Lexicon {
    [word: string]: Familiarity;
}
enum Familiarity {
    unknown = 0;
    learning = 1;
    known = 2;
}
```
- The lexicon can suggest which words in the given text should be translated in order to approximate the goal where the reader:
        - doesn't have to look up unknown words which interrupts reading
        - is not bothered by translation of known words
```ts
function shouldTranslateWord(word: string, lexicon: Lexicon): boolean {
    return lexicon[words] < Familiarity.known;
}
```
- The lexicon gets updated given which words the reader looks up and which words the reader doesn't. Words been looked up should be unknown while words not been looked up should get increased familiarity. Note that for words not in the lexicon, those been looked up should be marked as unknown in the lexicon, those not been looked up should be marked as known in the lexicon.
```ts
function lookedUp(word: string, lexicon: Lexicon): void {
    if (lexicon[word] === undefined) lexicon[word] = Familiarity.unknown;
    else lexicon[word] = Familiarity.unknown;
}
function notLookedUp(word: string, lexicon: Lexicon): void {
    if (lexicon[word] === undefined) lexicon[word] = Familiarity.known;
    lexicon[word] = Math.min(lexicon[word] + 1, Familiarity.known);
}
```



# how to display hint for unfamiliar words
![](https://github.com/pangrr/reading-assistant/blob/master/hint.png)
```html demo.html #sampleParagraph
<p #selectHook style="line-height: 2.3em">
Call me Ishmael. Some years ago—never mind how long precisely—having little or no money in my purse, and nothing particular to interest me on shore, I thought I would sail about a little and see the watery part of the world. It is a way I have of driving off the <span hint="脾">spleen</span> and regulating the circulation. Whenever I find myself growing <span hint="严峻">grim</span> about the mouth; whenever it is a <span hint="潮湿">damp</span>, <span hint="蒙蒙">drizzly</span> November in my soul; whenever I find myself involuntarily pausing before <span hint="棺材">coffin</span> warehouses, and bringing up the rear of every funeral I meet; and especially whenever my <span hint="狂躁">hypos</span> <span hint="
得到这样的优势">get such an upper hand of</span> me, that it requires a strong moral principle to prevent me from deliberately stepping into the street, and methodically knocking people’s hats off—then, I account it high time to get to sea as soon as I can. This is my substitute for <span hint="手枪">pistol</span> and ball. With a <span hint="哲学上">philosophical</span> <span hint="繁荣">flourish</span> Cato throws himself upon his sword; I quietly take to the ship. There is nothing surprising in this. If they but knew it, almost all men in their degree, some time or other, <span hint="珍爱">cherish</span> very nearly the same feelings towards the ocean with me.
</p>
```
```html demo.html #style
<style>
[hint] {
  position: relative;
  z-index: 2;
  cursor: pointer;
}
[hint]:after {
  position: absolute;
  bottom: -12px;
  left: 0;
  width: 100%;
  color: silver;
  content: attr(hint);
  text-align: center;
  font-size: 11px;
  line-height: 1;
}
</style>
```



# how to display translation for user selected text
## how to get user selected text
```js demo.html #script
function getSelectedText() {
  if (window.getSelection) {
    return window.getSelection().toString();
  } else if (document.selection && document.selection.type !== "Control") {
    return document.selection.createRange().text;
  }
}
function doSomethingWithSelectedText() {}
```
``` demo.html #selectHook
onmouseup="doSomethingWithSelectedText()"
```


# how to get translation
- An API that to return simple explaination (hint below text) (Google traslation?).
- Another API to get full explaination (tooltip on selection) (Youdao?).


# biolerplate
```html demo.html
<html>
<head>
#style
<script>
#script
</script>
</head>
<body>
  <div style="margin: auto; margin-top: 100px; width: 70%;">
    #sampleParagraph
  </div>
</body>
</html>
```
