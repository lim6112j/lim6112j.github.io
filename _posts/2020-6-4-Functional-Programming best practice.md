---
layout: post
comments: true
categories: development
---
## Quicksort
```
const quicksort = ([head, ...tail]: any[]):any => head === undefined ? [] : 
  [...quicksort([...tail.filter(a => a <= head)]), head, ...quicksort([...tail.filter(a => a > head)])];

console.log(quicksort([2,4,1,7,3]))
```
## Try-Catch custom monad
```
import {Try, Success, Failure} from './try_monad';
const processTry = (replacement: any = null) => (p: any) => {
  return p.then((v: any) => new Success(v))
          .catch((err:any) => {
            console.log("### Error, if exists, replacement will be used ######" ,err);
            return new Failure(replacement ? replacement : err)});
};
let record = Try.of(() => findById("92375c2-be1d-415b-a55a-f93d82705b6e"))
.map(processTry(new User("jane doe", 0, Date.now())))
.flatten();

record.then((v:any) => console.log(v))

```
* try-catch monad
```
export class Try {
  _val: any;
  constructor(val: any){
    this._val = val;
  }
  static of(fn:any): (Success | Failure) {
    try {
      return new Success(fn());
    } catch (error) {
      return new Failure(error);
    }
  }
  flatten() {
    return this._val;
  }
  map(fn: any) {
    return Try.of(() => fn(this._val));
  }
}
export class Success extends Try {
  getOrElse(anotherVal: any) {
    return this._val;
  };
  getOrElseThrow() {
    return this._val;
  }
}
export class Failure extends Try {
  map(fn: any) {
    return this;
  }
  getOrElse(anotherVal: any) {
    return anotherVal;
  }
  getOrElseThrow() {
    if(this._val !== null) {
      throw this._val;
    }
  }
}
```
