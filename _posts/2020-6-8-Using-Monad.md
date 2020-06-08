---
layout: post
comments: true
categories: Functional Programming
---
## How to use Either Monad

* imperative code

```
//imperative
//Returns error or price including tax 
const tax = (tax, price) => {
  if (!_.isNumber(price)) return new Error("Price must be numeric");

  return price + (tax * price);
};

//Returns error or price indluding discount
const discount = (dis, price) => {
  if (!_.isNumber(price)) return (new Error("Price must be numeric"));

  if (price < 10) return new Error("discount cant be applied for items priced below 10");

  return price - (price * dis);
};

const isError = (e) => e && e.name == 'Error';

const getItemPrice = (item) => item.price;

//shows total price after tax and discount. Need to handle multiple errors.
const showTotalPrice = (item, taxPerc, disount) => {
  let price = getItemPrice(item);
  let result = tax(taxPerc, price);
  if (isError(result)) {
    return console.log('Error: ' + result.message);
  }
  result = discount(discount, result);
  if (isError(result)) {
    return console.log('Error: ' + result.message);
  }
  //display result
  console.log('Total Price: ' + result);
}

let tShirt = { name: 't-shirt', price: 11 };
let pant = { name: 't-shirt', price: '10 dollars' };
let chips = { name: 't-shirt', price: 5 }; //less than 10 dollars error

showTotalPrice(tShirt) // Total Is: 9.075
showTotalPrice(pant)   // Error: Price must be numeric
showTotalPrice(chips)  //Error: discount cant be applied for items priced below 10
```

* Remove Exception with Monad : Either 

```
import {Either} from 'ramda-fantasy';
import * as R from 'ramda';
import _ from 'lodash';
var Left = Either.Left
var Right = Either.Right
const curry = R.curry;
const tax = curry((tax, price) => {
  if(!_.isNumber(price)) return Left(new Error("price must be numeric"))

  return Right(price - (tax * price))
});
const discount = curry((dis, price) => {
  if(!_.isNumber(price)) return Left(new Error("Price must be numeric"));
  if(price < 10) return Left(new Error("discount cant be applied for items priced below 10"));
  return Right(price - price * dis);
});
const addCaliTax = tax(0.1);
const apply25PercDisc = discount(0.25);
const getItemPrice = (item) => Right(item.price);
const displayTotal = (total) => { console.log('Total Price: ' + total) }
const logError = (error) => { console.log('Error: ' + error.message); };
const eitherLogOrShow = Either.either(logError, displayTotal);
const showTotalPrice = (item) => eitherLogOrShow(getItemPrice(item).chain(apply25PercDisc).chain(addCaliTax));

let tShirt = { name: 't-shirt', price: 11 };
let pant = { name: 'pant', price: '10 dollars'};
let chips = { name: 't-shirt', price: 5}

showTotalPrice(tShirt)
showTotalPrice(pant)
showTotalPrice(chips)
```

* Maybe Monad

```
// monad is also an Applicative, implemented ap function
import {Maybe} from 'ramda-fantasy'
import * as R from 'ramda'
const curry = R.curry;
/**
 * dealing Null checks with monads
 */

// let user = {
//   name: 'lim',
//   discount: 'aaa'
// }
let user;
let discount = {
  code: 'bbb'
}
const maybeUser = Maybe(user);
const maybeDiscount = Maybe(discount);
var applyDiscount = curry(function (user, discount) {
  user.discount = discount.code;
  return user;
});

const maybeApplyDiscountFunc = maybeUser.map(applyDiscount);
maybeApplyDiscountFunc.ap(maybeDiscount); 
console.log(user)

// using Validation applicative in foltale library , fantasyland doesn't have this applicative

import {validation} from 'folktale';
const Success = validation.Success;
const Failure = validation.Failure;

// Instead of:
function isUserNameValidBad(a) {
  return /^(0|[1-9][0-9]*)$/.test(a) ?
    ["Username cant be a number"] : a
}
// use:
function isUsernameValid(a) {
  return /^(0|[1-9][0-9]*)$/.test(a) ?
    Failure(["Username can't be a number"]) : Success(a)
}

function isPwdLengthCorrect(a) {
  return a.length == 10 ? Success(a) : Failure(["Password must be 10 characters"])
}

function ieEmailValid(a) {
  var re = /^(([^<>()\[\]\\.,;:\s@"]+(\.[^<>()\[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/;

  return re.test(a) ? Success(a) : Failure(["Email is not valid"])
}

const returnSuccess = () => 'success';//simply returns success

function validateForm(username, pwd, email) {
  let success = R.curryN(3, returnSuccess);//3 coz we are calling "ap" 3 times.
  return Success(success)
      .ap(isUsernameValid(username))
      .ap(isPwdLengthCorrect(pwd))
      .ap(ieEmailValid(email))
}


console.log(validateForm('raja', 'pwd1234567', 'r@r.com').value);
//Output: success

console.log(validateForm('raja', 'pwd', 'r@r.com').value);
//Output: ['Password must be 10 characters' ]


console.log(validateForm('raja', 'pwd', 'notAnEmail').value);
//Output: ['Password must be 10 characters', 'Email is not valid']

console.log(validateForm('123', 'pwd', 'notAnEmail').value);
//['Username can\'t be a number', 'Password must be 10 characters', 'Email is not valid']
```

