### Here is a list of example programs

### fibonacci
```TypeScript

function fibonacci(n: number): number {
    // No need for types as the local assignment 
    // to primitives is self-documenting.
    let sum = 1, previous = 0;

    for (let i = 0; i < n; i++) {
        [previous, sum] = [sum, previous + sum];
    }
    
    return sum;
}
```

### quick sort
```TypeScript

function quicksort<T> (array: Array<T>): Array<T> {
    if (array.length <= 1) {
        return array;
    }

    const arrayCopy = array.slice(0);
    // `arrayCopy.pop()` returns an element of type
    // parameter `T` OR an undefined. Thus pivot
    // must also be unioned with an undefined.
    const pivot: T | undefined = arrayCopy.pop();
    const lesser: Array<T> = [];
    const greater: Array<T> = [];

    arrayCopy.forEach((element) => {
        // Normally, with strict null checking
        // TypeScript will point out an error here.
        // Pivot was typed to potentially be undefined
        // but TypeScript points out we did not handle
        // that case at this point. However, if we are
        // 100% confident that pivot will never be undefined
        // at this point we can use a `!` to tell TypeScript
        // not to worry about it.
        if (element > pivot!) {
            greater.push(element);
        } else {
            lesser.push(element);
        }
    });

    return quicksort(lesser).concat(pivot!, quicksort(greater))
};
```


### interleave
```TypeScript
function interleave<T>(array: Array<T>, ...rest: Array<T>): Array<T> {
    const shorter = array.length > rest.length ? rest : array;
    const longer = array.length > rest.length ? array : rest;
    const answer = [];

    for (let i = 0; i < shorter.length; i += 1) {
        answer.push(array[i]);
        answer.push(rest[i]);
    }

    return answer.concat(longer.slice(shorter.length, longer.length));
}
```

### strip quotes
```TypeScript
function stripQuotes(s: string): string {
  return (str.replace(/"|'/g, ''));
}
```

### infamous say
```TypeScript
type CurriedFunction = (s: string) => (CurriedFunction | string); 

function say(input: string): (CurriedFunction | string) {
    let words = '';

    const chain = (str: string): (CurriedFunction | string) => {
        if (str) {
            words += str + " ";
        }

        return str ? chain : words.trim();
    };

    return chain(input);
}

// usage
console.log(say("we")("did")("it")()); // prints "we did it"
```

### A compiler written in TypeScript
A partial plug, but I ported a majority of a compiler that was previously written in JavaScript. I turned on as many strict flags as I could, and it could be very helpful to see an existing project setup in TypeScript. 
Check out [jenlang's github repository](https://github.com/jtorre39/jenlang).
