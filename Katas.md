# Katas:
- ### [A Chain adding function kata](#a-chain-adding-function)
[Codewars link](https://www.codewars.com/kata/539a0e4d85e3425cb0000a88)
- ### [Unary function chainer kata](#unary-function-chainer)
[Codewars link](https://www.codewars.com/kata/54ca3e777120b56cb6000710)
- ### [Calculating with Functions kata](#calculating-with-functions)
[Codewars link](https://www.codewars.com/kata/525f3eda17c7cd9f9e000b39)

## A Chain adding function

The task:
![Image](https://raw.githubusercontent.com/dstn3422/dstn3422.github.io/main/assets/chain.png)

How to:

First of all we need our `add` function to return a function that takes a single argument so that we can take all the variables and values inside the parenthesis as arguments. The technique is called currying. So let's do that:
```javascript
const add = (n) => {
  const fx = (x) => ;
  
  return fx;
}
```
Now our `add` function will return a function, however this will only take care of two parenthesis and we need it to take all of them. And what do we do when we don't know the number of times we need to call a function to get our desired result? That's right, recursion!
```javascript
const add = (n) => {
  const fx = (x) => add(n + x);
  
  return fx;
}
```
We only got one thing to take care of now, the last call of our recursive function.

Let's take a look of what happens so far with this example:

`add(1)(2)(3)`

The `add` function will be called with the number 1 as an argument, then it will call the `fx` function which will take the next number, 2, add 1 and 2 and call the add function, now with 3 as a sole argument. After the first call of the `add` function we will get:

`add(3)(3)`

Now same thing happens again and we will get:

`add(6)`

But this time the `fx` function won't get any arguments, since there are no more left. So the `add` will return a function, `fx`. But we need the value of `n` in this case. 

Luckily for us functions are objects under the hood in JavaScript. Objects have a method called valueOf and if we create a valueOf method for the `fx` function it will return that value:
```javascript
const add = (n) => {
  const fx = (x) => add(n + x);
  fx.valueOf = () => n;
  
  return fx;
}
```
And voila, it's working!
## Unary function chainer

The task:
![Image](https://raw.githubusercontent.com/dstn3422/dstn3422.github.io/main/assets/unary.png)

How to:

To get the (input) we need the `chained` function to return a function.
```javascript
function chained(functions){

  return function(input){};
}
```
Now our anonymous function that is returned from the `chained` function will take the `input` as an argument.

We need to execute each function in the `functions` array.
```javascript
function chained(functions){

  return function(input){
    for(let i = 0; i < functions.length; i++){
      functions[i](input);
    }
  };
}
```
Then we have to update the value of `input` for each function we execute on it.
```javascript
function chained(functions){

  return function(input){
    for(let i = 0; i < functions.length; i++){
      input = functions[i](input);
    }
  };
}
```
Note: this might make our function impure if the `input` is passed as reference (in case of objects and arrays this function will modify the original variable we are passing to it).

Finally we return the final value.
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
We can refactor it and make it look more professional by using arrow functions and the array reduce method. And don't forget to rename every identifier to a single letter so that the next person reading it will have no idea wtf is going on but hey, at least it's a one liner and that's the most important thing in a clean code, right?
```javascript
const chained = (f) => (i) => f.reduce((r,c) => c(r), i);
```
## Calculating with Functions

The task:
![Image](https://raw.githubusercontent.com/dstn3422/dstn3422.github.io/main/assets/calc.png)

How to:

First let's break down what we want our functions to do for easier understanding.

We want our digit functions to be able to take either 0 arguments, in which case it should simply return the digit or 1 argument in which case it should pass it's value to the operator function that is passed to the digit function as an argument.

Well, the first case is simple enough:
```javascript
const zero = (op) => op ? : 0;
```
We declare the zero function as a function that takes an op argument then we check if the argument is truthy or falsy. If we call the zero function without an argument we will get a falsy result as op will be undefined, however if we pass a function to the zero function then the op will be a function and the result will be truthy.

Now what should we return if the result is truthy?

Let's leave that question hanging for now and see what can we do with our operator functions.

We want our operator functions to take a left operand and a right operand and calculate the result of the expression.

But our order is as follows: digit(operator(digit)).

As you may see operator only gets one argument. We have to fix that somehow.

That's where the truthy return statement from the digit function come in. So now we can say that if our digit function does take an operator function as argument we want to return the operator function with that digit. That way digit(operator(digit)) will become operator(digit)(digit). So our zero function will look like this:
```javascript
const zero = (op) => op ? op(0) : 0;
```
Okay but now that we have a operator(digit)(digit) how do we convert that to the desired result?

Well we'll do it so that our operator function returns a function so that it can take the value in the second parentheses as an argument.
```javascript
const plus = (r) => (l) => r + l;
```
So now our plus function will take an argument and return a function that will also take an argument and return the sum of those arguments.

And that's it! We just have to do the same for every digit and every operator function but I'll leave that for you. 
Of course this will violate the DRY rule so instead of creating 10 digit functions with different digits we can make one general function that will take a digit as an argument and return a function with the correct digit and assign that function with the correct argument to each digit function.
