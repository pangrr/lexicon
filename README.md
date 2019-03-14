# how it works
1. Given a person's words familiarity.
2. Show the person a text where words of low familiarity have explaination.
3. Observe the person's behavior of looking up for words in the text.
4. Update the person's words familiarity by
    - descrease familiarity of the words the person looks up
    - increase familiarity of the rest of the words


## words familiarity data structure
```
let wordsFamiliarity: WordsFamiliarity = {
   nausea: Familiarity.Alien,
   almond: Familiarity.Known
};

enum Familiarity = {
    Alien = 0,
    Maybe = 1,
    Known = 2
};

interface WordsFamiliarity {
    [key: string]: Familiarity;
}
```

## which words should show explaination
```
function shouldShowExplaination(wordsFamiliarity: WordsFamiliarity, word: string): boolean {
    return isKnownWord(wordsFamiliarity, word);
}


function isKnownWord(wordsFamiliarity: WordsFamiliarity, word: string): boolean {
    if (wordsFamiliarity[word] === undefined) {
        return true;
    } else {
        return wordsFamiliarity[word] === Familiarity.Known;
    }
}
```

## update words familiarity
```
function lookedUp(wordsFamiliarity: WordsFamiliarity, ...words: string[]): WordsFamiliarity {
    return descreaseFamiliarity(wordsFamiliarity, words);
}

function notLookedUp(wordsFamiliarity: WordsFamiliarity, ...words): WordsFamiliarity {
    return increaseFamiliarity(wordsFamiliarity, words);
}
```
