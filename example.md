### Here is a list of example programs


### fibonacci
```TypeScript

function fibonacci(x: number): number {
    if (x <= 1) {
        return x;
    }

    return fibonacci(x - 1) + fibonacci(x - 2);
}
```

### quick sort
```TypeScript

function quicksort<T> (array: Array<T>): Array<T> {
    if (array.length <= 1) {
        return array;
    }

    const arrayCopy = array.slice(0);
    const pivot: T | undefined = arrayCopy.pop();
    const lesser: Array<T> = [];
    const greater: Array<T> = [];

    arrayCopy.forEach((element) => {
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

```
