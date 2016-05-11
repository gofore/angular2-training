# RxJS demo

Create an observable sequence that produces a value after each period and subscribe to it:
```javascript
var observable = Rx.Observable.interval(500).take(5);
var subscription = observable.subscribe(
    x => console.log('onNext: ' + x),
    e => console.log('onError: ' + e.message),
    () => console.log('onCompleted'));

// => "onNext: 0"
// => "onNext: 1"
// => "onNext: 2"
// => "onNext: 3"
// => "onNext: 4"
// => "onCompleted"
```
Unsubscribe the subscription:
```javascript
subscription.unsubscribe();
```

Add filtering for uneven values:
```javascript
var observable = Rx.Observable.interval(500).take(5)
    .filter(x => x % 2 === 0);
observable.subscribe(x => console.log(x));
```

Use map to add 10 to every value:
```javascript
var observable = Rx.Observable.interval(500).take(5)
    .filter(x => x % 2 === 0)
    .map(x => x + 10);
```

Accumulate the result:
```javascript
var o = Rx.Observable.interval(500).take(5)
    .filter(x => x % 2 === 0)
    .map(x => x + 10)
    .reduce((acc, x) => acc + x)
```

Buffer values:
```javascript
var o = Rx.Observable.interval(100).take(50)
    .filter(x => x % 2 === 0)
    .map(x => x + 10)
    .bufferCount(5);
```

Concatenate and merge Observables:
```javascript
var o = Rx.Observable.from([1,2,3,4,5]);
var o2 = Rx.Observable.from([6,7,8,9]);
o.concat(o2).subscribe(x => console.log(x));
o.merge(o2).subscribe(x => console.log(x));
```

Flatmap example:
```javascript
var o = Rx.Observable.range(1,10);
var o2 = o.flatMap(x => Rx.Observable.range(x,2));
o2.subscribe(x => console.log(x));
```

Observable from event: Create Observable from mousemove event and take value emitted every 500 ms:
```javascript
var pos = Rx.Observable.fromEvent(document, 'mousemove')
    .map(e => {return {x:e.clientX, y:e.clientY}}).throttleTime(500);
var s = pos.subscribe(pos => {console.log('x: ' + pos.x + ' y: ' + pos.y)});    
```