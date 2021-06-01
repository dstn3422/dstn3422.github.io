# Katas:
- ### [A Chain adding function kata](#a-chain-adding-function)
[Codewars link](https://www.codewars.com/kata/539a0e4d85e3425cb0000a88)
- ### [Unary function chainer kata](#unary-function-chainer)
[Codewars link](https://www.codewars.com/kata/54ca3e777120b56cb6000710)
- ### [Calculating with Functions kata](#calculating_with_functions)
[Codewars link](https://www.codewars.com/kata/525f3eda17c7cd9f9e000b39)

### Markdown

Markdown is a lightweight and easy-to-use syntax for styling your writing. It includes conventions for

```markdown
Syntax highlighted code block

# Header 1
## Header 2
### Header 3

- Bulleted
- List

1. Numbered
2. List

**Bold** and _Italic_ and `Code` text
```

## A Chain adding function

The task:
![Image](https://raw.githubusercontent.com/dstn3422/dstn3422.github.io/main/assets/chain.png)

## Unary function chainer

The task:
![Image](https://raw.githubusercontent.com/dstn3422/dstn3422.github.io/main/assets/unary.png)

How to:
1. To get the (input) we need the `chained` function to return a function.
```javascript
function chained(functions){

  return function(input){};
}
```
Now our anonymous function that is returned from the chained function will take the input as parameter.

2. We need to execute each function in the functions array.
```javascript
function chained(functions){

  return function(input){
    for(let i = 0; i < functions.length; i++){
      functions[i](input);
    }
  };
}
```
3. Then we have to update the value of input for each function we execute on it.
```javascript
function chained(functions){

  return function(input){
    for(let i = 0; i < functions.length; i++){
      input = functions[i](input);
    }
  };
}
```
Note: this might make our function impure if the input passed as reference (in case of objects and arrays).

4. Finally we return the final value.
```javascript
function chained(functions){

  return function(input){
    for(let i = 0; i < functions.length; i++){
      input = functions[i](input);
    }
    
    return input;
  };
}
```
## Calculating with Functions

The task:
![Image](https://raw.githubusercontent.com/dstn3422/dstn3422.github.io/main/assets/calc.png)
