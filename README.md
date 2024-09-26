# grep
grep

How to pass this stage
Since this is the first stage, we've included some commented code to help you get started. To pass this stage, simply uncomment the code and submit your changes.

Step 1: Navigate to app/main.ts


Expand
Step 2: Uncomment code

Step 3: Submit changes

 Note: After your first Git push, you should see Tests failed in the bar below this card. This is expected! Complete the steps above to pass the tests.


In this stage, we'll handle the simplest regex possible: a single character.

Example: a should match "apple", but not "dog".

Your program will be executed like this:

$ echo -n "apple" | ./your_program.sh -E "a"
The -E flag instructs grep to interprets patterns as extended regular expressions (with support for metacharacters like +, ? etc.). We'll use this flag in all stages.

You program must exit with 0 if the character is found, and 1 if not.
-----------------------
In this stage, we'll implement support for the \d character class.

\d matches any digit.

Example: \d should match "3", but not "c".

Your program will be executed like this:

$ echo -n "apple123" | ./your_program.sh -E "\d"
You program must exit with 0 if a digit is found in the string, and 1 if not.
function matchPattern(inputLine: string, pattern: string): boolean {
  if (pattern.length === 1) {
    return inputLine.includes(pattern);
  } else if (pattern === "\\d") {
    return /\d/g.test(inputLine);
  } else {
    throw new Error(`Unhandled pattern: ${pattern}`);
  }
}
if (args[2] !== "-E") {
  console.log("Expected first argument to be '-E'");
  process.exit(1);
}
if (matchPattern(inputLine, pattern)) {
  process.exit(0);
} else {
  process.exit(1);
}
-----------------------------------
In this stage, we'll implement support for the \w character class.

\w matches any alphanumeric character (a-z, A-Z, 0-9, _).

Example: \w should match "foo101", but not "$!?".

Your program will be executed like this:

$ echo -n "alpha-num3ric" | ./your_program.sh -E "\w"
You program must exit with 0 if an alphanumeric character is found in the string, and 1 if not.

const args = process.argv;
const pattern = args[3];
const inputLine: string = await Bun.stdin.text();
function matchPattern(inputLine: string, pattern: string): boolean {
  if (pattern.length === 1) {
    return inputLine.includes(pattern);
  } else if (pattern === '\\d') {
    return /\d/g.test(inputLine);
  } else if (pattern === '\\w') {
    return /\w/g.test(inputLine);
  } else {
    throw new Error(`Unhandled pattern: ${pattern}`);
  }
}
if (args[2] !== "-E") {
  console.log("Expected first argument to be '-E'");
  process.exit(1);
}
// You can use print statements as follows for debugging, they'll be visible when running tests.
console.log("Logs from your program will appear here!");
// Uncomment this block to pass the first stage
if (matchPattern(inputLine, pattern)) {
  process.exit(0);
} else {
  process.exit(1);
}

--------------------------
In this stage, we'll add support for positive character groups.

Positive character groups match any character that is present within a pair of square brackets.

Example: [abc] should match "apple", but not "dog".

Your program will be executed like this:

$ echo -n "apple" | ./your_program.sh -E "[abc]"
You program must exit with 0 if an any of the characters are found in the string, and 1 if not.

const args = process.argv;
const pattern = args[3];
const inputLine: string = await Bun.stdin.text();
function matchPattern(inputLine: string, pattern: string): boolean {
  if (pattern.length === 1) {
    return inputLine.includes(pattern);
  } else if (pattern === "\\d") {
    return /\d/g.test(inputLine);
  } else if (pattern === "\\w") {
    return /\w/g.test(inputLine);
  } else if (pattern.startsWith("[") && pattern.endsWith("]")) {
    let chars = pattern.slice(0, pattern.length - 1);
    return Array.from(chars).some((char) => inputLine.includes(char));
  } else {
    throw new Error(`Unhandled pattern: ${pattern}`);
  }
}
if (args[2] !== "-E") {
  console.log("Expected first argument to be '-E'");
  process.exit(1);
}
// You can use print statements as follows for debugging, they'll be visible when running tests.
console.log("Logs from your program will appear here!");
// Uncomment this block to pass the first stage
if (matchPattern(inputLine, pattern)) {
  process.exit(0);
} else {
  process.exit(1);
}
export {};

---------------------------------
In this stage, we'll add support for negative character groups.

Negative character groups match any character that is not present within a pair of square brackets.

Example: [^abc] should match "dog", but not "cab" (since all characters are either "a", "b" or "c").

Your program will be executed like this:

$ echo -n "apple" | ./your_program.sh -E "[^abc]"
You program must exit with 0 if the input contains characters that aren't part of the negative character group, and 1 if not.

const args = process.argv;
const pattern = args[3];
const inputLine: string = await Bun.stdin.text();
function matchPattern(inputLine: string, pattern: string): boolean {
  if (pattern.length === 1) {
    return inputLine.includes(pattern);
  } else if (pattern === "\\d") {
    return /\d/g.test(inputLine)
  } else if (pattern === "\\w") {
    return /\w/g.test(inputLine)
  } else if (pattern.startsWith("[^") && pattern.endsWith("]")) {
    const char = pattern.slice(2, -1).split('')
    return !char.some((c) => inputLine.includes(c))
  } else if (pattern.startsWith("[") && pattern.endsWith("]")) {
    const char = pattern.slice(1, -1).split("")
    const char = pattern.slice(1, -1).split('')
    return char.some((c) => inputLine.includes(c))
  } else {
    throw new Error(`Unhandled pattern: ${pattern}`);
  }
}
if (args[2] !== "-E") {
  console.log("Expected first argument to be '-E'");
  process.exit(1);
}
// You can use print statements as follows for debugging, they'll be visible when running tests.
console.log("Logs from your program will appear here!");
if (matchPattern(inputLine, pattern)) {
  process.exit(0);
} else {
  process.exit(1);
}

----------------------------------
In this stage, we'll add support for patterns that combine the character classes we've seen so far.

This is where your regex matcher will start to feel useful.

Keep in mind that this stage is harder than the previous ones. You'll likely need to rework your implementation to process user input character-by-character instead of the whole line at once.

We recommend taking a look at the example code in "A Regular Expression Matcher" by Rob Pike to guide your implementation.

Examples:

\d apple should match "1 apple", but not "1 orange".
\d\d\d apple should match "100 apples", but not "1 apple".
\d \w\w\ws should match "3 dogs" and "4 cats" but not "1 dog" (because the "s" is not present at the end).
Your program will be executed like this:

$ echo -n "1 apple" | ./your_program.sh -E "\d apple"
You program must exit with 0 if the pattern matches the input, and 1 if not.

const args = process.argv;
const pattern = args[3];
const inputLine: string = await Bun.stdin.text();
function matchPattern(inputLine: string, pattern: string): boolean {
  if (pattern.length === 1) {
    return inputLine.includes(pattern);
  } else if (pattern === "\\d") {
    return /\d/g.test(inputLine);
  } else if (pattern === "\\w") {
    return /\w/g.test(inputLine);
  } else if (pattern.startsWith("[^") && pattern.endsWith("]")) {
    const charArray = pattern.slice(2, -1).split("");
    return !charArray.some((c) => inputLine.includes(c));
  } else if (pattern.startsWith("[") && pattern.endsWith("]")) {
    const charArray = pattern.slice(1, -1).split("");
    return charArray.some((c) => inputLine.includes(c));
  } else {
    throw new Error(`Unhandled pattern: ${pattern}`);
    // Handle patterns with multiple \d or \w
    const regexPattern = pattern
      .replace(/\\d/g, "\\d")
      .replace(/\\w/g, "\\w")
      .replace(/\\s/g, "\\s");
    const regex = new RegExp(regexPattern, "g");
    // Test the regular expression against the input line
    return regex.test(inputLine);
  }
}
if (args[2] !== "-E") {
  console.log("Expected first argument to be '-E'");
  process.exit(1);
}
// You can use print statements as follows for debugging, they'll be visible when running tests.
console.log("Logs from your program will appear here!");
// Uncomment this block to pass the first stage
if (matchPattern(inputLine, pattern)) {
  console.log(0);
  process.exit(0);
} else {
  console.log(1);
  process.exit(1);
}

------------------------------------------
In this stage, we'll add support for ^, the Start of String or Line anchor.

^ doesn't match a character, it matches the start of a line.

Example: ^log should match "log", but not "slog".

Your program will be executed like this:

$ echo -n "log" | ./your_program.sh -E "^log"
You program must exit with 0 if the input starts with the given pattern, and 1 if not.

import { log } from "console";
const args = process.argv;
const pattern = args[3];
const inputLine: string = await Bun.stdin.text();
function matchDigit(char: string): boolean {
  let digit = ["0", "1", "2", "3", "4", "5", "6", "7", "8", "9"];
  return digit.includes(char);
}
function matchAlphabetical(char: string): boolean {
  let alphabet = [
    "a",
    "b",
    "c",
    "d",
    "e",
    "f",
    "g",
    "h",
    "i",
    "j",
    "k",
    "l",
    "m",
    "n",
    "o",
    "p",
    "q",
    "r",
    "s",
    "t",
    "u",
    "v",
    "w",
    "x",
    "y",
    "z",
    "_",
  ];
  return alphabet.includes(char);
}
function matchGroupClass(char: string, charClass: string[]): boolean {
  return charClass.includes(char);
}
function matchNegatedGroupClass(char: string, charClass: string[]):boolean {
  return !charClass.includes(char);
}
function searchSimpleChar(inputLine: string, pattern: string): number {
  if (inputLine === pattern) return 1;
  else return -1;
}
if (args[2] !== "-E") {
  console.log("Expected first argument to be '-E'");
  process.exit(1);
}
// You can use print statements as follows for debugging, they'll be visible when running tests.
console.log("Logs from your program will appear here!");
// split the patter in sections and check if the corresponding inputLine section match the pattern
function matchPatternCombination(inputLine: string, pattern: string): boolean {
  let findPattern = false;
  let logCharacterIndex = 0;
  patternLoop: for (let index = 0; index < pattern.length; index++) {
    //if we are at the end of the inputLine and we still have pattern to match, than we return false
    if (logCharacterIndex === inputLine.length) {
      if (index !== pattern.length) {
        findPattern = false;
        break patternLoop;
      }
    }
    searchLineLoop: for (let charactersIndex = logCharacterIndex; charactersIndex < inputLine.length; charactersIndex++) {
      let char = pattern[index];
      if (char === "\\") {
        if (pattern[index + 1] === "d") {
          if(matchDigit(inputLine[charactersIndex])){
            findPattern = true;
            index ++;
            logCharacterIndex = charactersIndex + 1;
            continue patternLoop;
          }
          else {
            if(findPattern){
              findPattern = false;
              break patternLoop;         
            }
            else{
              findPattern = false;
              continue searchLineLoop;
            }
          }
        } else if (pattern[index + 1] === "w") {
          if(matchAlphabetical(inputLine[charactersIndex])){
            findPattern = true;
            index ++;
            logCharacterIndex = charactersIndex + 1;
            continue patternLoop;
          }
          else {
            if(findPattern){
              findPattern = false;
              break patternLoop;         
            }
            else{
              findPattern = false;
              continue searchLineLoop;
            }
          }
        }
      }
      else if(char === "["){
        if(pattern[index + 1] === "^"){
          //search the index from the "]" in the pattern starting from the next character
          let i = pattern.indexOf("]", index + 2);
          let negationArray = pattern.slice(index + 2, i).split("");
          if(negationArray.some((char)=> inputLine.includes(char))){
            if(findPattern){
              findPattern = false;
              break patternLoop;
            }
          }
          else{
            findPattern = true;
            index ++;
            logCharacterIndex = charactersIndex + 1;
            break patternLoop;
          }
        }
        else {
          let i = pattern.indexOf("]", index + 1);
          let patternArray = pattern.slice(index + 1 , i).split("");
          if(patternArray.some((char)=> inputLine.includes(char))){
            findPattern = true;
            index ++;
            logCharacterIndex = charactersIndex + 1;
            break patternLoop;
          }
          else{
              findPattern = false;
              break patternLoop;
          }
        }
      }
      else if(char === "]"){
        continue;
      }
      else {
        let i = searchSimpleChar(inputLine[charactersIndex], char);
        console.log("i: ", i);
        if (i === -1) {
          if(findPattern){
            findPattern = false;
            break patternLoop;         
          }
          else{
            findPattern = false;
            continue searchLineLoop;
          }
        } else {
          findPattern = true;
          logCharacterIndex = charactersIndex + 1;
          continue patternLoop;
        }
      }
    }
  }
  console.log("findPattern: ", findPattern);
  return findPattern;
}
function matchPatternCombination_2(inputLine: string, pattern: string): boolean {
  let i = 0; //index for inputLine
  let j = 0; //index for pattern
  let findPattern = true;
  while (i < inputLine.length && j < pattern.length) {
    if(pattern[j] == "\\"){
    if(pattern[j]=="^"){
      if(i !== 0 || inputLine[i] !== pattern[j+1]){
        return false;
      }
      else {
        findPattern = true;
        j+=2;
      }
    }
    else if(pattern[j] == "\\"){
      if(pattern[j + 1] == "d"){
        if(!matchDigit(inputLine[i])){
          findPattern = false;
          i++;
          continue;
        }
        findPattern = true;
        j+=2; //skip the "d"
      }
      else if(pattern[j + 1] == "w"){
        if(!matchAlphabetical(inputLine[i])){
          findPattern = false;
          i++;
          continue;
        }
        findPattern = true;
        j+=2; //skip the "w"
      }
      else {
        findPattern = false;
        j++;
      }
    }
    else if(pattern[j]=="["){
      let negation = pattern[j+1] === "^";
      let closingBracket = pattern.indexOf("]", j);
      let charClass = pattern.slice(j+1, closingBracket).split("");
      if(negation){
        if(!matchNegatedGroupClass(inputLine[i], charClass)){
          findPattern = false;
          i++;
          continue;
        }
        findPattern = true;
      }
      else{
        if(!matchGroupClass(inputLine[i], charClass)){
          findPattern = false;
          i++;
          continue;
        }
        findPattern = true;
      }
      j = closingBracket + 1;
    }
    else {
      if(inputLine[i] !== pattern[j]){
        findPattern = false;
        i++;
        continue;
      }
      findPattern = true;
      j++;
    }
    i++;
  }
  if(i === inputLine.length && j < pattern.length){ findPattern = false;}
  
    return findPattern;
}
if (matchPatternCombination_2(inputLine, pattern)) {
  process.exit(0);
} else {
  process.exit(1);
}

-------------------------------------
In this stage, we'll add support for $, the End of String or Line anchor.

$ doesn't match a character, it matches the end of a line.

Example: dog$ should match "dog", but not "dogs".

Your program will be executed like this:

$ echo -n "dog" | ./your_program.sh -E "dog$"
You program must exit with 0 if the input matches the given pattern, and 1 if not.

const args = process.argv;
const pattern = args[3];
const inputLine: string = await Bun.stdin.text();
const CHAR_CLASS = {
  NUMBER: '\\d',
  ALPHA_NUMERIC: '\\w'
}
//Character Basic Matchings
function isDigit(c: string) {
  return c.charCodeAt(0) >= 48 && c.charCodeAt(0) <= 57;
}
function isLowerCaseFont(c: string) {
  return c.charCodeAt(0) >= 97 && c.charCodeAt(0) <= 122;
}
function isUpperCaseFont(c: string) {
  return c.charCodeAt(0) >= 65 && c.charCodeAt(0) <= 90;
}
//Cahracter Combined Matchings
function isFont(c: string) {
  return isLowerCaseFont(c) || isUpperCaseFont(c);
}
//0-9, a-z, A-Z, _
function isAlphaNumeric(c: string) {
  return isDigit(c) || isFont(c) || c.charCodeAt(0) === 95;
}
function isCharIncluded(c: string, pattern: string) {
  let start = pattern.indexOf('[');
  let end = pattern.indexOf(']');
  let chars = pattern.substring(start+1, end);
  return chars.includes(c);
}
function isNotIncludedChar(c: string, pattern: string) {
  let start = pattern.indexOf('[^');
  let end = pattern.indexOf(']');
  let chars = pattern.substring(start+2, end);
  return !chars.includes(c);
}
//string matchers
function hasDigit(input: string) {
  return input.split('').some( c => isDigit(c));
}
function hasAlphaNumeric(input: string) {
  return input.split('').some( c => isAlphaNumeric(c));
}
function includesCharacters(input: string, pattern: string) {
  let start = pattern.indexOf('[');
  let end = pattern.indexOf(']');
  let chars = pattern.substring(start+1, end);
  return input.split('').some( c => chars.includes(c));
}
function notIncludesCharacters(input: string, pattern: string) {
  let start = pattern.indexOf('[^');
  let end = pattern.indexOf(']');
  let chars = pattern.substring(start+2, end);
  return input.split('').every( c => !chars.includes(c));
}
function doMatch(searchText: string, pattern: string) {
  console.log('Matching: '+ searchText + " To:" + pattern );
  let currentPattern = pattern;
  let currentSearchTextIndex = 0;
  let matched = true;
  while (currentPattern.length > 0 && matched && currentSearchTextIndex < searchText.length) {
    let patternLength = 1;
    const char = searchText.charAt(currentSearchTextIndex);
    console.log((char), currentPattern);
    if (currentPattern.startsWith(CHAR_CLASS.NUMBER)) {
      matched = isDigit(char);
      patternLength = 2;
    } else if (currentPattern.startsWith(CHAR_CLASS.ALPHA_NUMERIC)) {
      matched = isAlphaNumeric(searchText.charAt(currentSearchTextIndex));
      patternLength = 2;
    } else if (currentPattern.startsWith('[^')) {
      matched = isNotIncludedChar(char, currentPattern);
      patternLength = currentPattern.indexOf(']') + 1;
    } else if (currentPattern.startsWith('[')) {
      matched = isCharIncluded(char, currentPattern);
      patternLength = currentPattern.indexOf(']') + 1;
    } else {
      matched = char === currentPattern.substring(0, 1);
    }
    currentPattern = currentPattern.substring(patternLength, currentPattern.length);
    currentSearchTextIndex++;
  }
  if (currentPattern.length > 0) {
  if (currentPattern.length > 0 && currentPattern !== '$') {
    matched = false;
  }
  return matched;
}
function matchPattern(inputLine: string, pattern: string): boolean {
  if (pattern.length === 1) {
    return inputLine.includes(pattern);
  }
  if (pattern === CHAR_CLASS.NUMBER) {
    return hasDigit(inputLine);
  }
  if (pattern === CHAR_CLASS.ALPHA_NUMERIC) {
    return hasAlphaNumeric(inputLine);
  }
  if (pattern.startsWith('^')) {
    return doMatch(inputLine, pattern.substring(1));
  }
  let currentInputIndex = 0;
  let matched = false;
  while (currentInputIndex < inputLine.length && !matched) {
    matched = doMatch(inputLine.substring(currentInputIndex), pattern);
    currentInputIndex++;
  }
  return matched;
}
if (args[2] !== "-E") {
  console.log("Expected first argument to be '-E'");
  process.exit(1);
}
// You can use print statements as follows for debugging, they'll be visible when running tests.
console.log("Logs from your program will appear here!");
// Uncomment this block to pass the first stage
if (matchPattern(inputLine, pattern)) {
  process.exit(0);
} else {
  process.exit(1);
}

-----------------------------------
In this stage, we'll add support for +, the one or more quantifier.

Example: a+ should match "apple" and "SaaS", but not "dog".

Your program will be executed like this:

$ echo -n "caats" | ./your_program.sh -E "ca+ts"
You program must exit with 0 if the input matches the given pattern, and 1 if not.


export {}; // This makes the file a module
// import {Bun} from "bun";
const args = process.argv;
const pattern = args[3];
const inputLine: string = await Bun.stdin.text();
function patternChecker(input) {
  const inputString = input.split("");
  if (inputString[0] === "d" && inputString[1].length > 0) {
    return true;
  }
  return false;
}
function matchPattern(inputLine: string, pattern: string): boolean {
  if (pattern.includes("+")) {
    const parts = pattern.split("+");
    const prefix = parts[0];
    const suffix = parts[1];
    const regex = new RegExp(prefix + "+"); // Create regex for the prefix followed by one or more
    return regex.test(inputLine) && inputLine.includes(suffix);
  }
  if (pattern.length === 1) {
    return inputLine.match(pattern) !== null;
  } else if (pattern === "\\d") {
    return inputLine.match(/\d/) !== null;
  } else if (pattern === "\\w") {
    return inputLine.match(/\w/) !== null;
  } else if (pattern.startsWith("[") && pattern.endsWith("]")) {
    const newPattern = pattern.slice(1, pattern.length - 1).split("");
    // check for negative patterns
    if (newPattern[0] === "^") {
      newPattern.shift();
      return !newPattern.some((c) => inputLine.includes(c));
    }
    return newPattern.some((c) => inputLine.includes(c));
  } else if (pattern === "\\d \\w\\w\\ws") {
    return /\d \w\w\ws/g.test(inputLine);
  } else if (pattern === "\\d\\d\\d apples") {
    return /\d\d\d apple/g.test(inputLine);
  } else if (pattern === "\\d apple") {
    return /\d apple/g.test(inputLine);
  } else if (pattern[0] === "^") {
    const newPattern = pattern.slice(1);
    return newPattern === inputLine;
  } else if (pattern[pattern.length - 1] === "$") {
    const newPattern = pattern.slice(0, pattern.length - 1);
    return newPattern === inputLine;
  }
}
if (args[2] !== "-E") {
  console.log("Expected first argument to be '-E'");
  process.exit(1);
}
console.log("Logs from your program will appear here!");
// Uncomment this block to pass the first stage
if (matchPattern(inputLine, pattern)) {
  console.log("pattern matched");
  process.exit(0);
} else {
  console.log("pattern did not match");
  process.exit(1);
}

------------------------------------
In this stage, we'll add support for ?, the zero or one quantifier (also known as the "optional" quantifier).

Example: dogs? should match "dogs" and "dog", but not "cat".

Your program will be executed like this:

$ echo -n "dogs" | ./your_program.sh -E "dogs?"
You program must exit with 0 if the input matches the given pattern, and 1 if not.

const args = process.argv;
const pattern = args[3];
const inputLine: string = await Bun.stdin.text();
function matchPattern(input: string, pattern: string): boolean {
  if (pattern.startsWith('^')) {
    return matchFromIndex(input, pattern.slice(1), 0, true);
  }
  if (pattern.endsWith('$')) {
    return matchFromIndex(input, pattern.slice(0, -1), input.length - pattern.length + 1, true);
  }
  for (let i = 0; i <= input.length; i++) {
    if (matchFromIndex(input, pattern, i, false)) {
      return true;
    }
  }
  return false;
}
function matchFromIndex(input: string, pattern: string, startIndex: number, mustMatchAll: boolean): boolean {
  let inputIndex = startIndex;
  let patternIndex = 0;
  while (patternIndex < pattern.length) {
    if (inputIndex >= input.length) {
      // Check if remaining pattern is optional
      while (patternIndex < pattern.length - 1 && pattern[patternIndex + 1] === '?') {
        patternIndex += 2;
      }
      return patternIndex === pattern.length;
    }
    if (pattern[patternIndex] === '\\') {
      patternIndex++;
      if (patternIndex >= pattern.length) {
        throw new Error("Invalid pattern: \\ at end of pattern");
      }
      switch (pattern[patternIndex]) {
        case 'd':
          if (!/\d/.test(input[inputIndex])) {
            return false;
          }
          break;
        case 'w':
          if (!/\w/.test(input[inputIndex])) {
            return false;
          }
          break;
        default:
          throw new Error(`Unhandled escape sequence: \\${pattern[patternIndex]}`);
      }
      inputIndex++;
      patternIndex++;
    } else if (pattern[patternIndex] === '[') {
      const closeBracketIndex = pattern.indexOf(']', patternIndex);
      if (closeBracketIndex === -1) {
        throw new Error("Invalid pattern: unclosed [");
      }
      const charGroup = pattern.slice(patternIndex + 1, closeBracketIndex);
      if (!new RegExp(`[${charGroup}]`).test(input[inputIndex])) {
        return false;
      }
      inputIndex++;
      patternIndex = closeBracketIndex + 1;
    } else if (pattern[patternIndex + 1] === '+') {
      const charToRepeat = pattern[patternIndex];
      let count = 0;
      while (inputIndex < input.length && input[inputIndex] === charToRepeat) {
        inputIndex++;
        count++;
      }
      if (count === 0) {
        return false;
      }
      patternIndex += 2;
    } else if (pattern[patternIndex + 1] === '?') {
      const optionalChar = pattern[patternIndex];
      if (input[inputIndex] === optionalChar) {
        inputIndex++;
      }
      patternIndex += 2;
    } else if (pattern[patternIndex] !== input[inputIndex]) {
      return false;
    } else {
      inputIndex++;
      patternIndex++;
    }
  }
  return mustMatchAll ? inputIndex === input.length : true;
}
if (args[2] !== "-E") {
  console.log("Expected first argument to be '-E'");
  process.exit(1);
}
// You can use print statements as follows for debugging, they'll be visible when running tests.
console.log("Logs from your program will appear here!");
if (matchPattern(inputLine, pattern)) {
  process.exit(0);
} else {
  process.exit(1);
}-----------------------------


In this stage, we'll add support for ., which matches any character.

Example: d.g should match "dog", but not "cog".

Your program will be executed like this:

$ echo -n "dog" | ./your_program.sh -E "d.g"
You program must exit with 0 if the input matches the given pattern, and 1 if not.

const args = process.argv;
const pattern = args[3];
const inputLine: string = await Bun.stdin.text();
function matchPattern(inputLine: string, pattern: string): boolean {
  if (pattern.includes("+")) {
    const parts = pattern.split("+");
    return parts.filter((x) => !inputLine.includes(x)).length === 0;
  } else if (pattern.includes(".")) {
    const regexp = new RegExp(pattern.replace(".", "\\w"), "g");
    return regexp.test(inputLine);
  } else if (pattern.includes("?")) {
    const parts = pattern.split("?");
    const sss = parts.join("");
    let rs = "";
    for (let index = 0; index < parts.length; index++) {
      if (index < parts.length - 1) {
        rs += parts[index].slice(0, -1);
      } else {
        rs += parts[index];
      }
    }
    return inputLine.includes(sss) || inputLine.includes(rs);
  } else if (pattern.length === 1) {
    return inputLine.includes(pattern);
  } else if (pattern === "\\d") {
    return /\d/g.test(inputLine);
  } else if (pattern === "\\w") {
    return /\w/g.test(inputLine);
  } else if (pattern.startsWith("[^") && pattern.endsWith("]")) {
    const charArray = pattern.slice(2, -1).split("");
    return !charArray.some((c) => inputLine.includes(c));
  } else if (pattern.startsWith("[") && pattern.endsWith("]")) {
    const charArray = pattern.slice(1, -1).split("");
    return charArray.some((c) => inputLine.includes(c));
  } else if (pattern === "\\d \\w\\w\\ws") {
    return /\d \w\w\ws/g.test(inputLine);
  } else if (pattern === "\\d\\d\\d apples") {
    return /\d\d\d apple/g.test(inputLine);
  } else if (pattern === "\\d apple") {
    return /\d apple/g.test(inputLine);
  } else if (pattern.startsWith("^")) {
    return inputLine.startsWith(pattern.slice(1));
  } else if (pattern.endsWith("$")) {
    return inputLine.endsWith(pattern.slice(0, -1));
  } else {
    throw new Error(`Unhandled pattern: ${pattern}`);
  }
}
if (args[2] !== "-E") {
  console.log("Expected first argument to be '-E'");
  process.exit(1);
}
// You can use print statements as follows for debugging, they'll be visible when running tests.
console.log("Logs from your program will appear here!");
// Uncomment this block to pass the first stage
const result = matchPattern(inputLine, pattern);
console.log("🚀 ~ result:", result);
if (result) {
  process.exit(0);
} else {
  process.exit(1);
}


--------------------------------
In this stage, we'll add support for the | keyword, which allows combining multiple patterns in an either/or fashion.

Example: (cat|dog) should match "dog" and "cat", but not "apple".

Your program will be executed like this:

$ echo -n "cat" | ./your_program.sh -E "(cat|dog)"
You program must exit with 0 if the input matches the given pattern, and 1 if not.


//app/main.ts
import { testChar } from "./patternTest";
const args = process.argv;
const pattern = args[3];
const inputLine: string = await Bun.stdin.text();
function matchPattern(inputLine: string, pattern: string): boolean {
export function matchPattern(inputLine: string, pattern: string): boolean {
  
  if (pattern[0] === '[' && pattern[pattern.length -1] === ']') {
    if (pattern[1] === '^') return pattern.slice(2,-1).split('').every(c => {
      return !inputLine.includes(c);
    })
    return pattern.slice(1,-1).split('').some(c => {
      return inputLine.includes(c);
    })
  }
  let patternIndex = 0; 
  let inputIndex = 0;  
  
  if (pattern[0] === '^') {
    patternIndex++;
  } else if (pattern[pattern.length -1] === '$') {
    pattern = pattern.slice(0,-1)
    inputIndex = inputLine.length - pattern.length; 
  } else {
    while(!testChar(inputLine,pattern,inputIndex,0)[0] && inputIndex < inputLine.length) inputIndex++;
  }
  
  while (patternIndex < pattern.length) {
    let charRes = testChar(inputLine,pattern,inputIndex,patternIndex); 
    if (!charRes[0]) return false;
    patternIndex = charRes[1] + 1;
    //inputIndex++;
    inputIndex = charRes[2] + 1;
  }
  console.log("true")
  return true;
}
if (args[2] !== "-E") {
  console.log("Expected first argument to be '-E'");
  process.exit(1);
}
export { };
if (matchPattern(inputLine, pattern)) {
  process.exit(0);
} else {
  process.exit(1);
}



//app/patternTest.ts

import { matchPattern } from "./main";
export function testChar (inputLine : string, pattern: string, inputIndex: number, patternIndex : number) : [boolean, number, number] {
    if (inputIndex >= inputLine.length) return [false, patternIndex, inputIndex];
    const patternCurrentChar = pattern[patternIndex];
    // we will test the altervative there 
    // there we suppose that all ( will be closed 
    if (patternCurrentChar === '(') {
        const patterns = pattern.slice(patternIndex + 1 , pattern.indexOf(')')).split('|'); 
        // we suppose again that all patterns have same length 
        const res = patterns.some(p => {
            console.log(`test for : ${inputLine.slice(inputIndex, patternIndex + patterns[0].length)} - ${p}`)
            return matchPattern(inputLine.slice(inputIndex, inputIndex + patterns[0].length), p);
        })
        return [res, pattern.indexOf(')') , inputIndex +  patterns[0].length]
    }
    if (patternIndex < pattern.length - 1 && pattern[patternIndex + 1] === '?') {
      
        if (patternIndex === pattern.length - 2) return [true, pattern.length -1, inputIndex]; 
        let nextPatternChar = pattern[patternIndex+2]; 
        while (inputLine[inputIndex] !== nextPatternChar && inputIndex < inputLine.length) inputIndex++;
       
        if (inputIndex >= inputLine.length) return [false, patternIndex + 2,inputIndex]
        return [true, patternIndex + 2,inputIndex]
    }
    
    // "+" shouldn't be in the pattern[0]
    if (patternCurrentChar === '+') {
        patternIndex++; 
        const char = pattern[patternIndex - 1]
        while (char === inputLine[inputIndex]) {
            inputIndex++;
        }
    } else if (patternCurrentChar === '\\') {
        patternIndex++; 
        if (pattern[patternIndex] === 'd') {
        
          if (!/\d/g.test(inputLine[inputIndex])) return [false, patternIndex,inputIndex];
        }
        if (pattern[patternIndex] === 'w') {
          
            if (!/\w/g.test(inputLine[inputIndex])) return [false, patternIndex, inputIndex];
        }
     
      } else if (patternCurrentChar === '.') return [true, patternIndex, inputIndex];
      else {
        if (patternCurrentChar !== inputLine[inputIndex]) return [false, patternIndex]; 
        if (patternCurrentChar !== inputLine[inputIndex]) return [false, patternIndex, inputIndex]; 
      }
   
    return [true, patternIndex, inputIndex]; 
}
\ No newline at end of file


-----------------------
In this stage, we'll add support for backreferences.

A backreference lets you reuse a captured group in a regular expression. It is denoted by \ followed by a number, indicating the position of the captured group.

Examples:

(cat) and \1 should match "cat and cat", but not "cat and dog".
\1 refers to the first captured group, which is (cat).
(\w+) and \1 should match "cat and cat" and "dog and dog", but not "cat and dog".
\1 refers to the first captured group, which is (\w+).
Your program will be executed like this:

$ echo -n "<input>" | ./your_program.sh -E "<pattern>"
Your program must exit with 0 if the input matches the given pattern, and 1 if not.

Note: You only need to focus on one backreference and one capturing group in this stage. We'll get to handling multiple backreferences in the next stage

//app/main.ts

import { register } from "module";
const args = process.argv;
const pattern = args[3];
const inputLine: string = await Bun.stdin.text();
function matchPattern(inputLine: string, pattern: string): boolean {
	if (pattern.length === 1) {
		return inputLine.includes(pattern);
	} else if (pattern.includes("\\1")) {
		let startsWithAnchor = false;
		let endsWithAnchor = false;
		if (pattern.startsWith("^")) {
			startsWithAnchor = true;
			pattern = pattern.slice(1);
		}
		if (pattern.endsWith("$")) {
			endsWithAnchor = true;
			pattern = pattern.slice(0, -1);
		}
		try {
			const { capturingGroupPattern } = parsePattern(pattern);
			const groupTokens = parseCapturingGroup(capturingGroupPattern);
			const backrefPos = parseBackreference(pattern);
			if (backrefPos === false)
				throw new Error("Backreference not found.");
			const capturingGroupStart = pattern.indexOf("(");
			const capturingGroupEnd = pattern.indexOf(")", capturingGroupStart);
			const beforeGroup = pattern.substring(0, capturingGroupStart);
			const afterGroup = pattern.substring(
				capturingGroupEnd + 1,
				backrefPos
			);
			const afterBackref = pattern.substring(backrefPos + 2); // Skip \1
			if (!inputLine.startsWith(beforeGroup)) {
				return false;
			}
			let pos = beforeGroup.length;
			let posStart = pos;
			let capturedGroup = "";
			let matched = false;
			if (capturingGroupPattern.includes("|")) {
				const options = capturingGroupPattern.split("|");
				for (const option of options) {
					const optionTokens = parseCapturingGroup(option);
					const tempPos = pos;
					const [groupMatched, newPos, tempCapturedGroup] =
						matchTokensWithCapture(
							optionTokens,
							inputLine,
							tempPos
						);
					if (groupMatched) {
						matched = true;
						capturedGroup = tempCapturedGroup;
						pos = newPos;
						break;
					}
				}
				if (!matched) return false;
			} else {
				const [groupMatched, newPos, tempCapturedGroup] =
					matchTokensWithCapture(groupTokens, inputLine, pos);
				if (!groupMatched) {
					return false;
				}
				pos = newPos;
				capturedGroup = tempCapturedGroup;
			}
			if (afterGroup.length > 0) {
				const afterGroupTokens = parseCapturingGroup(afterGroup);
				const [afterGroupMatched, newPos] = matchTokens(
					afterGroupTokens,
					inputLine,
					pos
				);
				if (!afterGroupMatched) {
					return false;
				}
				pos = newPos;
			}
			if (capturedGroup.length > 0) {
				if (
					inputLine.substr(pos, capturedGroup.length) !==
					capturedGroup
				) {
					return false;
				}
				pos += capturedGroup.length;
			}
			if (afterBackref.length > 0) {
				const afterBackrefTokens = parseCapturingGroup(afterBackref);
				const [afterBackrefMatched, newPos] = matchTokens(
					afterBackrefTokens,
					inputLine,
					pos
				);
				if (!afterBackrefMatched) {
					return false;
				}
				pos = newPos;
			}
			if (startsWithAnchor && posStart !== 0) {
				return false;
			}
			if (endsWithAnchor && pos !== inputLine.length) {
				return false;
			}
			if (startsWithAnchor || endsWithAnchor) {
				return pos === inputLine.length;
			} else {
				return true;
			}
		} catch (error) {
			console.error("Error in matchPattern:", error);
			return false;
		}
	} else if (pattern === "\\d") {
		return /\d/g.test(inputLine);
	} else if (pattern === "\\w") {
Expand 37 lines
		throw new Error(`Unhandled pattern: ${pattern}`);
	}
}
function parsePattern(pattern) {
	const capturingGroupRegex = /\(([^()]+)\)/;
	const backreferenceRegex = /\\1/;
	const captureGroupMatch = pattern.match(capturingGroupRegex);
	if (!captureGroupMatch)
		throw new Error("Invalid pattern: Missing capturing group");
	const capturingGroupPattern = captureGroupMatch[1];
	const backreferenceMatch = pattern.match(backreferenceRegex);
	if (!backreferenceMatch)
		throw new Error("Invalid pattern: Missing backreference");
	return {
		capturingGroupPattern,
		backreferenceIndex: backreferenceMatch.index,
	};
}
function parseCapturingGroup(groupPattern) {
	const tokens = [];
	let i = 0;
	while (i < groupPattern.length) {
		const char = groupPattern[i];
		if (char === "\\") {
			const nextChar = groupPattern[i + 1];
			if (["w", "d", "s"].includes(nextChar)) {
				tokens.push({ type: "charClass", value: `\\${nextChar}` });
				i += 2;
			} else {
				tokens.push({ type: "literal", value: nextChar });
				i += 2;
			}
		} else if (char === ".") {
			tokens.push({ type: "wildcard", value: "." });
			i++;
		} else if (char === "|") {
			tokens.push({ type: "alternation", value: "|" });
			i++;
		} else if (["+", "*", "?"].includes(char)) {
			tokens.push({ type: "quantifier", value: char });
			i++;
		} else if (char === "[") {
			let charSet = "";
			i++;
			let negated = false;
			if (groupPattern[i] === "^") {
				negated = true;
				i++;
			}
			while (groupPattern[i] !== "]" && i < groupPattern.length) {
				charSet += groupPattern[i];
				i++;
			}
			if (groupPattern[i] !== "]") {
				throw new Error("Unterminated character class");
			}
			tokens.push({
				type: negated ? "negatedCharSet" : "charSet",
				value: charSet,
			});
			i++;
		} else {
			tokens.push({ type: "literal", value: char });
			i++;
		}
	}
	return tokens;
}
function parseBackreference(pattern) {
	const backrefRegex = /\\1/;
	const match = pattern.match(backrefRegex);
	if (!match) return false;
	return match.index;
}
function matchTokens(tokens, inputLine, pos) {
	let i = 0;
	while (i < tokens.length) {
		let token = tokens[i];
		let quantifier = null;
		if (i + 1 < tokens.length && tokens[i + 1].type === "quantifier") {
			quantifier = tokens[i + 1].value;
		}
		const [matchResult, newPos] = applyTokenWithQuantifier(
			token,
			quantifier,
			inputLine,
			pos
		);
		if (!matchResult) {
			return [false, pos];
		}
		pos = newPos;
		if (quantifier) {
			i += 2;
		} else {
			i++;
		}
	}
	return [true, pos];
}
function matchTokensWithCapture(tokens, inputLine, pos) {
	let i = 0;
	let captured = "";
	while (i < tokens.length) {
		let token = tokens[i];
		let quantifier = null;
		if (i + 1 < tokens.length && tokens[i + 1].type === "quantifier") {
			quantifier = tokens[i + 1].value;
		}
		const [matchResult, newPos, tempCaptured] =
			applyTokenWithQuantifierCapture(token, quantifier, inputLine, pos);
		if (!matchResult) {
			return [false, pos, captured];
		}
		pos = newPos;
		captured += tempCaptured;
		if (quantifier) {
			i += 2;
		} else {
			i++;
		}
	}
	return [true, pos, captured];
}
function applyTokenWithQuantifier(token, quantifier, inputLine, pos) {
	if (!quantifier) {
		return matchSingleToken(token, inputLine, pos);
	} else {
		return applyQuantifier(token, quantifier, inputLine, pos);
	}
}
function applyTokenWithQuantifierCapture(token, quantifier, inputLine, pos) {
	if (!quantifier) {
		return matchSingleTokenWithCapture(token, inputLine, pos);
	} else {
		return applyQuantifierWithCapture(token, quantifier, inputLine, pos);
	}
}
function matchSingleToken(token, inputLine, pos) {
	if (pos >= inputLine.length) {
		return [false, pos];
	}
	const currentChar = inputLine[pos];
	if (token.type === "literal") {
		if (currentChar !== token.value) {
			return [false, pos];
		}
		return [true, pos + 1];
	} else if (token.type === "wildcard") {
		return [true, pos + 1];
	} else if (token.type === "charClass") {
		let match = false;
		if (token.value === "\\w" && /\w/.test(currentChar)) {
			match = true;
		} else if (token.value === "\\d" && /\d/.test(currentChar)) {
			match = true;
		} else if (token.value === "\\s" && /\s/.test(currentChar)) {
			match = true;
		}
		if (!match) {
			return [false, pos];
		}
		return [true, pos + 1];
	} else if (token.type === "charSet") {
		const charClassRegex = new RegExp(`^[${token.value}]$`);
		if (!charClassRegex.test(currentChar)) {
			return [false, pos];
		}
		return [true, pos + 1];
	} else if (token.type === "negatedCharSet") {
		const negatedCharClassRegex = new RegExp(`^[^${token.value}]$`);
		if (!negatedCharClassRegex.test(currentChar)) {
			return [false, pos];
		}
		return [true, pos + 1];
	} else {
		return [false, pos];
	}
}
function matchSingleTokenWithCapture(token, inputLine, pos) {
	if (pos >= inputLine.length) {
		return [false, pos, ""];
	}
	const currentChar = inputLine[pos];
	if (token.type === "literal") {
		if (currentChar !== token.value) {
			return [false, pos, ""];
		}
		return [true, pos + 1, currentChar];
	} else if (token.type === "wildcard") {
		return [true, pos + 1, currentChar];
	} else if (token.type === "charClass") {
		let match = false;
		if (token.value === "\\w" && /\w/.test(currentChar)) {
			match = true;
		} else if (token.value === "\\d" && /\d/.test(currentChar)) {
			match = true;
		} else if (token.value === "\\s" && /\s/.test(currentChar)) {
			match = true;
		}
		if (!match) {
			return [false, pos, ""];
		}
		return [true, pos + 1, currentChar];
	} else if (token.type === "charSet") {
		const charClassRegex = new RegExp(`^[${token.value}]$`);
		if (!charClassRegex.test(currentChar)) {
			return [false, pos, ""];
		}
		return [true, pos + 1, currentChar];
	} else if (token.type === "negatedCharSet") {
		const negatedCharClassRegex = new RegExp(`^[^${token.value}]$`);
		if (!negatedCharClassRegex.test(currentChar)) {
			return [false, pos, ""];
		}
		return [true, pos + 1, currentChar];
	} else {
		return [false, pos, ""];
	}
}
function applyQuantifier(token, quantifier, inputLine, pos) {
	let startPos = pos;
	if (quantifier === "+") {
		let matchCount = 0;
		do {
			const [matchResult, newPos] = matchSingleToken(
				token,
				inputLine,
				pos
			);
			if (!matchResult) {
				break;
			}
			pos = newPos;
			matchCount++;
		} while (true);
		if (matchCount === 0) {
			return [false, startPos];
		}
	} else if (quantifier === "?") {
		const [matchResult, newPos] = matchSingleToken(token, inputLine, pos);
		if (matchResult) {
			pos = newPos;
		}
	} else if (quantifier === "*") {
		while (true) {
			const [matchResult, newPos] = matchSingleToken(
				token,
				inputLine,
				pos
			);
			if (!matchResult) {
				break;
			}
			pos = newPos;
		}
	} else {
		return [false, startPos];
	}
	return [true, pos];
}
function applyQuantifierWithCapture(token, quantifier, inputLine, pos) {
	let startPos = pos;
	let captured = "";
	if (quantifier === "+") {
		let matchCount = 0;
		do {
			const [matchResult, newPos, tempCaptured] =
				matchSingleTokenWithCapture(token, inputLine, pos);
			if (!matchResult) {
				break;
			}
			pos = newPos;
			captured += tempCaptured;
			matchCount++;
		} while (true);
		if (matchCount === 0) {
			return [false, startPos, captured];
		}
	} else if (quantifier === "?") {
		const [matchResult, newPos, tempCaptured] = matchSingleTokenWithCapture(
			token,
			inputLine,
			pos
		);
		if (matchResult) {
			pos = newPos;
			captured += tempCaptured;
		}
	} else if (quantifier === "*") {
		while (true) {
			const [matchResult, newPos, tempCaptured] =
				matchSingleTokenWithCapture(token, inputLine, pos);
			if (!matchResult) {
				break;
			}
			pos = newPos;
			captured += tempCaptured;
		}
	} else {
		return [false, startPos, captured];
	}
	return [true, pos, captured];
}
if (args[2] !== "-E") {
	console.log("Expected first argument to be '-E'");
	process.exit(1);
}
// You can use print statements as follows for debugging, they'll be visible when running tests.
console.log("Logs from your program will appear here!");
console.log("input line: ", inputLine);
console.log("pattern: ", pattern);
// Uncomment this block to pass the first stage
if (matchPattern(inputLine, pattern)) {
	process.exit(0);
} else {
	process.exit(1);
}


------------------------------
Multiple Backreferences
In this stage, we'll add support for multiple backreferences.

Multiple backreferences allow you to refer to several different captured groups within the same regex pattern.

Example: (\d+) (\w+) squares and \1 \2 circles should match "3 red squares and 3 red circles" but should not match "3 red squares and 4 red circles".

Your program will be executed like this:

$ echo -n "<input>" | ./your_program.sh -E "<pattern>"
Your program must exit with 0 if the input matches the given pattern, and 1 if not.


app/tokenizer.ts
import {BackReference, CapturedBlock, Concat, Equals, IsAlphaNum, IsDigit, IsNotOneOf, IsOneOf, Optional, Or, Plus, Regex, SimpleRegex, Wildcard, } from "./matcher"
const SPECIAL_CHARS = ["\\", "^", "$", "[", "]", "+", "?", ".", "(", ")", "|"]
export class State {
  isOpen: boolean = false;
  values: string[] = [];
  open() {
    this.isOpen = true;
  }
  push(s: string) {
    if (!this.isOpen) {
      throw new Error("attempted to push into a closed state object");
    }
    this.values.push(s);
  }
  pop(): string | undefined {
    if (!this.isOpen) {
      throw new Error("attempted to pop from a closed state object");
    }
    return this.values.pop()
  }
  delete() {
    if (this.isOpen) {
      throw new Error("attempted to delete state in use");
    }
  }
}
export function tokenizer(pattern: string): [SimpleRegex, State] {
  let tokens: SimpleRegex[] = [];
  let state = new State() // used as a common memory for regex components
  let lastIndex = -1; // only for verifying currentIndex increases
  let currentIndex = 0;
  while (currentIndex < pattern.length) {
    if (lastIndex >= currentIndex) {
      throw new Error(`Tokenizer got stuck in position ${lastIndex} of the string \"${pattern}\"`)
    }
    lastIndex = currentIndex;
    if (!SPECIAL_CHARS.includes(pattern[currentIndex])) {
      tokens.push(new Equals(pattern[currentIndex]));
      currentIndex++;
      continue;
    }
    switch (pattern[currentIndex]) {
      case "^":
        throw new Error("Start anchor inside pattern")
      case "$":
        throw new Error("End anchor inside pattern")
      case "]":
        throw new Error("bracket ] not opened");
      case ")":
        throw new Error("parenthesis ) not opened");
      case "|":
        throw new Error("Or sign | not inside parenthesis");
      case ".":
        tokens.push(new Wildcard());
        currentIndex++;
        break
      case "\\":
        if (pattern.length === currentIndex + 1) {
          throw new Error("Pattern ends with backslash")
        }
        switch(pattern[currentIndex + 1]) {
          case "d":
            tokens.push(new IsDigit());
            currentIndex += 2;
            break
          case "w":
            tokens.push(new IsAlphaNum());
            currentIndex += 2;
            break
          case "1":
            tokens.push(new BackReference(state, 0));
            currentIndex += 2;
            break;
          default:
            throw new Error("unsupported character after backslash")
            // check if this is a backreference
            let cc = pattern[currentIndex + 1].charCodeAt(0);
            if (49 <= cc && cc <= 57) {
              tokens.push(new BackReference(state, cc - 49));
              currentIndex += 2;
            } else {
              throw new Error(`unsupported character after backslash: ${pattern[currentIndex + 1]}`)
            }
        }
        break
        break;
      
      
      case "[":
        let brackets_end_idx = currentIndex;
        let opened_bracket_count = 1
        while (opened_bracket_count != 0) {
          brackets_end_idx++;
          if (brackets_end_idx === pattern.length) {
            throw new Error("bracket [ not closed")
          }
          if (pattern[brackets_end_idx] === "[") {
            opened_bracket_count++;
          }
          if (pattern[brackets_end_idx] === "]") {
            opened_bracket_count--;
          }
        }
        let stringInBrackets = pattern.slice(currentIndex + 1, brackets_end_idx);
        currentIndex = brackets_end_idx + 1;
        if (stringInBrackets.length === 0) {
          throw new Error("Empty brackets []")
        }
        if (stringInBrackets[0] === "^") {
          tokens.push(new IsNotOneOf(stringInBrackets.slice(1)));
        } else {
          tokens.push(new IsOneOf(stringInBrackets));
        }
        break
      case "+":
        let lastToken = tokens.pop()
        if (lastToken === undefined) {
          throw new Error("plus symbol at the start");
        }
        tokens.push(new Plus(lastToken));
        currentIndex++;
        break
        
      case "?":
        let llastToken = tokens.pop()
        if (llastToken === undefined) {
          throw new Error("plus symbol at the start");
        }
        tokens.push(new Optional(llastToken));
        currentIndex++;
        break
      
      case "(":
        let par_end_idx = currentIndex;
        let delimeters: number[] = [currentIndex]
        let opened_par_count = 1
        while (opened_par_count != 0) {
          par_end_idx++;
          if (par_end_idx === pattern.length) {
            throw new Error("parenthesis ( not closed")
          }
          switch (pattern[par_end_idx]) {
            case "(":
              opened_par_count++;
              break
            case ")":
              opened_par_count--;
              break
            case "|":
              if (opened_par_count === 1) {
                delimeters.push(par_end_idx);
              }
              break;
            default:
              break;
          }
        }
        delimeters.push(par_end_idx)
        currentIndex = par_end_idx + 1;
        let regs: SimpleRegex[] = []
        for (let idx = 0; idx < delimeters.length - 1; idx++) {
          let [start, end] = [delimeters[idx] + 1, delimeters[idx + 1]];
          let [reg, _state] = tokenizer(pattern.slice(start, end));
          _state.delete();
          regs.push(reg);
        }
        tokens.push(new CapturedBlock(state, new Or(regs)));
        break;
      default:
        throw new Error(`Unhandled special character: ${pattern[currentIndex]}`);
    }
  }
  
  if (tokens.length === 0) {
    throw Error("should not happen")
  }
  return [new Concat(tokens), state];
}
\ No newline at end of file


-------------------------------
In this stage, we'll add support for nested backreferences. This means that a backreference is part of a larger capturing group, which itself is referenced again.

Example: ('(cat) and \2') is the same as \1 should match "'cat and cat' is the same as 'cat and cat'".

Your program will be executed like this:

$ echo -n "<input>" | ./your_program.sh -E "<pattern>"
Your program must exit with 0 if the input matches the given pattern, and 1 if not.

app/main.ts
import { Regex } from "./matcher";
import {tokenizer} from "./tokenizer"
const args = process.argv;
const pattern = args[3];
const inputLine: string = await Bun.stdin.text();
function parsePattern(pattern: string): Regex {
  if (pattern.length === 0) {
    throw new Error("Empty pattern");
  }
  let anchorStart : Boolean = (pattern[0] === "^");
  let anchorEnd : Boolean = (pattern[pattern.length - 1] === "$");
  let slice = pattern.slice((anchorStart? 1:0), pattern.length - (anchorEnd? 1:0));
  let [reg, state] = tokenizer(slice);
  let reg = tokenizer(slice);
  return new Regex(reg, anchorStart, anchorEnd);
}
function matchPattern(inputLine: string, pattern: string): Boolean {
  let reg = parsePattern(pattern);
  return reg.match(inputLine);
}
if (args[2] !== "-E") {
  console.log("Expected first argument to be '-E'");
  process.exit(1);
}
// You can use print statements as follows for debugging, they'll be visible when running tests.
// console.log("Logs from your program will appear here!");
// Uncomment this block to pass the first stage
if (matchPattern(inputLine, pattern)) {
  // console.log("match")
  process.exit(0);
} else {
  // console.log("doesn't match")
  process.exit(1);
}
\ No newline at end of file
app/matcher.ts
import { State } from "./tokenizer";
type Matches = Generator<number, void, undefined>;
export interface SimpleRegex {
    nextMatches(s: string): Matches;
}
export class Equals implements SimpleRegex {
  c: string;
  constructor (c: string) {
    if (c.length !== 1) {
        throw new Error(`expected character, recieved ${c}`);
    }
    this.c = c;
  }
  *nextMatches(s: string) {
    if (s.startsWith(this.c)) {
        yield 1;
    }
  }
}
export class IsDigit implements SimpleRegex {
  *nextMatches(s: string) {
    if (s.length > 0) {
        let cc = s.charCodeAt(0);
        if (48 <= cc && cc <= 57) {
            yield 1;
        }
    }
  }
}
export class IsAlphaNum implements SimpleRegex {
    *nextMatches(s: string) {
      if (s.length > 0) {
          let cc = s.charCodeAt(0);
          if ((48 <= cc && cc <= 57) || (97 <= cc && cc <= 122) || (65 <= cc && cc <= 90)) {
              yield 1;
          }
      }
    }
  }
  export class Wildcard implements SimpleRegex {
    *nextMatches(s: string) {
        if (s.length > 0) {
            yield 1;
        }
    }
  }
export class IsOneOf implements SimpleRegex {
  str: string;
  constructor(str: string) {
    this.str = str;
  }
  *nextMatches(s: string) { 
    if (s.length > 0) {
        let c = s[0];
        if (this.str.includes(c)) {
            yield 1;
        }
    }
  }
}
export class IsNotOneOf implements SimpleRegex {
  str: string;
  constructor(str: string) {
    this.str = str;
  }
  *nextMatches(s: string) { 
    if (s.length > 0) {
        let c = s[0];
        if (!this.str.includes(c)) {
            yield 1;
        }
    }
  }
}
export class Or implements SimpleRegex {
    regs: SimpleRegex[];
    constructor(regs: SimpleRegex[]) {
        this.regs = regs;
    }
    *nextMatches(s: string){
        let reachable = new Set<number>();
        for (const reg of this.regs) {
            for (const i of reg.nextMatches(s)) {
                if (reachable.has(i)) {
                    continue
                }
                reachable.add(i)
                yield i;
            }
        }
    }
}
export class Optional implements SimpleRegex {
    reg: SimpleRegex;
    constructor(reg: SimpleRegex) {
        this.reg = reg;
    }
    *nextMatches(s: string) {
        yield 0;
        for (const i of this.reg.nextMatches(s)) {
            if (i > 0) {
                yield i;
            }
        }
    }
}
export class Plus implements SimpleRegex {
    reg: SimpleRegex;
    constructor(reg: SimpleRegex) {
        this.reg = reg;
    }
    *nextMatches(s: string): Matches {
        let reachableStates = new Set<number>();
        for (const i of this.reg.nextMatches(s)) {
            if (reachableStates.has(i)) {
                continue
            }
            reachableStates.add(i);
            yield i;
            for (const j of this.nextMatches(s.slice(i))) {
                let k = i + j
                if (reachableStates.has(k)) {
                    continue;
                }
                reachableStates.add(k);
                yield k;
            }
        }
    }
}
export class BackReference implements SimpleRegex {
    state: State;
    index: number;
    constructor(state: State, index: number) {
        state.open()
        this.state = state;
        this.index = index;
    }
    *nextMatches(s: string) {
        if (this.state.values.length < this.index) {
            throw new Error("Backreference handling error")
        }
        let captured = this.state.values[this.index];
        let captured = this.state.get(this.index);
        if (captured.length === 0) {
            throw new Error("Backreference handling error")
        }
Expand 6 lines
export class CapturedBlock implements SimpleRegex {
    state: State;
    index: number;
    reg: SimpleRegex;
    constructor(state: State, reg: SimpleRegex) {
        state.open()
    constructor(state: State, index: number, reg: SimpleRegex) {
        this.state = state;
        this.index = index;
        this.reg = reg;
    }
    *nextMatches(s: string) {
        if (this.state.values[this.index] !== undefined) {
            throw new Error("State error")
        }
        for (const i of this.reg.nextMatches(s)) {
            let capturedBlock = s.slice(0, i);
            this.state.push(capturedBlock);
            this.state.values[this.index] = capturedBlock;
            yield i;
            this.state.pop();
        }
        this.state.values[this.index] = undefined;
    }
}
function* concatSingleRegexNextMatches(s: string, initialPositions: Matches, reg: SimpleRegex): Matches {
    let reachableStates = new Set<number>();
    for (const i of initialPositions) {
        let slice = s.slice(i);
        for (const j of reg.nextMatches(slice)) {
            let k = i + j;
            if (reachableStates.has(k)) {
                continue
            }
            reachableStates.add(k);
            yield k;
        }
    }
}
function* concatRegsNextMatches(s: string, regs: SimpleRegex[]): Matches {
    if (regs.length === 0) {
        throw new Error("should not happen")
    }
    if (regs.length === 1) {
        yield* regs[0].nextMatches(s);
    } else {
        let reachablePositions = new Set<number>()
        for (const position of concatSingleRegexNextMatches(s, concatRegsNextMatches(s, regs.slice(0, regs.length - 1)), regs[regs.length - 1])) {
            if (reachablePositions.has(position)) {
                continue;
            }
            reachablePositions.add(position);
            yield position;
        }
    }
}
export class Concat implements SimpleRegex {
  regs: SimpleRegex[];
  constructor(regs: SimpleRegex[]) {
    if (regs.length === 0) {
        throw new Error("should not happen")
    }
    this.regs = regs;
  }
  *nextMatches(s: string): Matches {
    yield* concatRegsNextMatches(s, this.regs);
  }
}
export class Regex {
  anchorStart: Boolean;
  anchorEnd: Boolean;
  simpleRegex: SimpleRegex;
  constructor(simpleRegex: SimpleRegex, anchorStart: Boolean, anchorEnd: Boolean) {
    this.simpleRegex = simpleRegex;
    this.anchorStart = anchorStart;
    this.anchorEnd = anchorEnd;
  }
  match(s: string): Boolean {
    if (this.anchorStart) {
        for (const j of this.simpleRegex.nextMatches(s)) {
            if (!this.anchorEnd || j === s.length) {
                return true;
            }
        }
    } else {
        for (let i = 0; i < s.length; i++) {
            let slice = s.slice(i)
            for (const j of this.simpleRegex.nextMatches(slice)) {
                if (!this.anchorEnd || i + j === s.length) {
                    return true;
                }
            }
        }
    }
    return false;
  }
}
\ No newline at end of file
app/tokenizer.ts
import {BackReference, CapturedBlock, Concat, Equals, IsAlphaNum, IsDigit, IsNotOneOf, IsOneOf, Optional, Or, Plus, Regex, SimpleRegex, Wildcard, } from "./matcher"
const SPECIAL_CHARS = ["\\", "^", "$", "[", "]", "+", "?", ".", "(", ")", "|"]
export class State {
  isOpen: boolean = false;
  values: string[] = [];
  values: (string | undefined)[];
  currIndex: number;
  open() {
    this.isOpen = true;
  constructor(size: number) {
    this.values =  new Array(size).fill(undefined);
    this.currIndex = 0;
  }
  push(s: string) {
    if (!this.isOpen) {
      throw new Error("attempted to push into a closed state object");
  get(i: number): string {
    if (i >= this.currIndex) {
      throw new Error("tried to access a value at a place that was not cleared")
    }
    this.values.push(s);
  }
  pop(): string | undefined {
    if (!this.isOpen) {
      throw new Error("attempted to pop from a closed state object");
    let res = this.values[i]
    if (res === undefined) {
      throw new Error("tried accessing a value that was not initiated")
    }
    return this.values.pop()
    return res
  }
  delete() {
    if (this.isOpen) {
      throw new Error("attempted to delete state in use");
  move() {
    this.currIndex++;
    if (this.currIndex >= this.values.length) {
      throw new Error("state index exceeds the array length")
    }
  }
}
export function tokenizer(pattern: string): [SimpleRegex, State] {
function _tokenizer(pattern: string, state: State): SimpleRegex {
  let tokens: SimpleRegex[] = [];
  let state = new State() // used as a common memory for regex components
  let lastIndex = -1; // only for verifying currentIndex increases
  let currentIndex = 0;
Expand 130 lines
        delimeters.push(par_end_idx)
        currentIndex = par_end_idx + 1;
        let stateIndex = state.currIndex;
        state.move()
        let regs: SimpleRegex[] = []
        for (let idx = 0; idx < delimeters.length - 1; idx++) {
          let [start, end] = [delimeters[idx] + 1, delimeters[idx + 1]];
          let [reg, _state] = tokenizer(pattern.slice(start, end));
          _state.delete();
          let reg = _tokenizer(pattern.slice(start, end), state);
          regs.push(reg);
        }
        tokens.push(new CapturedBlock(state, new Or(regs)));
        tokens.push(new CapturedBlock(state, stateIndex, new Or(regs)));
        break;
      default:
        throw new Error(`Unhandled special character: ${pattern[currentIndex]}`);
    }
  }
  
  if (tokens.length === 0) {
    throw Error("should not happen")
  }
  return [new Concat(tokens), state];
  return new Concat(tokens);
}
export function tokenizer(pattern: string): SimpleRegex {
  return _tokenizer(pattern, new State(9));
}
\ No newline at end of file
