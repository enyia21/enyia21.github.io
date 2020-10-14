---
layout: post
title:      "Promises Explained"
date:       2020-10-14 15:01:51 -0400
permalink:  promises_explained
---


According to [mdn] (https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise) a promise is a proxy for a value that isn't known when the promise is created.  An example of a promise would be the act of going to the ice cream shop and ordering vanilla ice cream.  You as the customer are passing information(i.e. the type of ice cream you want) to the employee, who will go in the back and check if they have the icream you ordered and bring it out.  While they check if your ice cream is available your promise to recieve the ice cream is *pending* because  you don't know if they have it or not.  You are operating on the promise that they will come back with it or they will inform you that they don't have it.  The strength of promises lies in the fact that they are asynchronous. This means you can do other things while you wait for the worker to return.  So while you check your phone or look at other items on their menu; the workers promise of vanilla ice cream will remain pending.  When the ice cream worker returns they will either have your favorite flavor or they won't.  The promise will either be *fullfilled* or *rejected*.   If he/she/they fail to bring your ice cream then the promise is rejected and if they bring out your favorite flavor then the promise has been *resolved*. 

The Promise allows programs to perform other actions will it awaits conformation that a promise has been fullfilled or rejected. The  `promise.then()`, `promise.catch()`, `promise.finally` are utilized to perform asychronous actions while resolving a request.  The `.then()` method allows for further actions to be taken on the returned promise object.   Nesting can be used to chain .then() methods together, or to execute an action on the returned object.  In the event of an error, .catch() can be utilized to catch the rejected error communication.   

A simple Promise:
```
		 let getIceCream = new Promise(grabVanillaIceCream)
		 getIceCream.then(vanilia icream => decide what to do with vallla icream)
		 getIceCream.catch(no_vanilla_ice_cream_message => we have no_vanilla_ice_cream_message)
```

A nested Promise:
```
     let getIceCream = new Promise(buyVanillaIceCream)
		 buyVanillaIceCream.then(vanilia icream => decide what to do with vallla icream)
		 buyVanillaIceCream.then(change and ice cream => give child change and ice cream)
		 buyVanillaIceCream.catch(not_enough_money_or_no_icecream => not_enough_money_or_no_icecream)
```

The nesting concept of promise is important to its utilization in fuctions such as fetch.  As humans we nest promises in our everyday lives.  To extend the ice cream shop example, we as users could hand over money which would act as a nested promise.  If the money is enough and vanilla is available we will recieve ice cream.  Our promise of recieving icecream involves nesting the promise to check for the flavors availability then returning that promise with the promise to provide the ice cream if the monetary condition is met.  
Otherwise we will not.   Catch acts a tool to capture the promise's rejected state.  A promise sends its rejected state and that is caught by the .catch() method.  This state is equivalent to saying that the ice cream is unavailable or the custumer failed to provide adequate payment.  The catch action acts like a .then method except it receives the rejected input.  `.then() `doesn't throw errors it catches resolved promises.  The `.catch()` method catches rejected/revoked promises.  
This was a breif overview of promises.  A promise allows the user to request an action and that promise will either be accepted or rejected.   This action happens asynchronously, meaning that other work can be done while the promise is being  settled.
