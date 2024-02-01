# RxJava
Super easy interface to handle concurrency and thread management

- RxJava is the Observable type that represents a stream of data or events
- It is intended for push (reactive) but can also be used for pull (interactive).
- It is lazy rather than eager.
- It can be used asynchronously or synchronously.
- Reactive Programming
- Uses Rx Operators
- Rx operator are a function that define the observable, how and when it should emit the data stream.
- Most operators operate on an Observable and return an Observable.


1. just()

```
Observable.just(1, 2, 3, 4, 5)
```

2. filter operators like filter(), skip(), skipLast(), take(), takeLast(), takeFirst()

```
// Filtter

 Observable
    .just(1, 2, 3, 4, 5)
    .filter(new Func1<Integer, Boolean>() {
        @Override
        public Boolean call(Integer integer) {
            //check if the number is odd? If the number is odd return true, to emmit that object.
            return integer % 2 != 0;
        }
    });

```

```
// skip

Observable<Integer> observable = Observable.from(new Integer[]{1,2,3,4,5})  //emit 1 to 5
        .skip(2);   //Skip first two elements

observable
        .subscribeOn(Schedulers.newThread())
        .observeOn(Schedulers.io())
        .subscribe(new Action1<Integer>() {
            @Override
            public void call(Integer integer) {
                Log.d("Observer", "Output:" + integer);
            }
        });
//Output will be : 3, 4, 5

```

3. combining operators: Combines more than 2 data stream emitted by different observable and emit single data stream.
Ex. concate().merge(),zip()


```
// concate

Observable<Integer> observer1 = Observable.from(new Integer[]{1, 2, 3, 4, 5});  //Emit 1 to 5
Observable<Integer> observer2 = Observable.from(new Integer[]{6, 7, 8, 9, 10}); //Emit 6 to 10

Observable.concat(observer1, observer2) //Concat the output of both the operators.
        .subscribeOn(Schedulers.newThread())
        .observeOn(Schedulers.io())
        .subscribe(new Action1<Integer>() {
            @Override
            public void call(Integer integer) {
                Log.d("Observer", "Output:" + integer);
            }
        });

```


```
// Zip : Class that combines both data streams
class ZipObject {
    int number; 
    String alphabet;
}

Observable<Integer> observable1 = Observable.from(new Integer[]{1, 2, 3, 4, 5});  //Emits integers
Observable<String> observable2 = Observable.from(new String[]{"A", "B", "C", "D", "F"});  //Emits alphabets
Observable<ZipObject> observable = Observable.zip(observable1, observable2,   
    //Function that define how to zip outputs of both the stream into single object.
    new Func2<Integer, String, ZipObject>() { 
        @Override
        public ZipObject call(Integer integer, String s) {
            ZipObject zipObject = new ZipObject();
            zipObject.alphabet = s;
            zipObject.number = integer;
            return zipObject;
        }
    });

observable
    .subscribeOn(Schedulers.newThread())
    .observeOn(Schedulers.io())
    .subscribe(new Action1<ZipObject>() {
        @Override
        public void call(ZipObject zipObject) {
            Log.d("Observer", "Output:" + zipObject.number + " " + zipObject.alphabet);
        }
    });
}
```

*Refference*
https://medium.com/@kevalpatel2106/what-should-you-know-about-rx-operators-54716a16b310
