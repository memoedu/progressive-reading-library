At my work I’ve been writing a lot of machine learning code. Some of it is machine learning code responsible for moving around a whole lot of money, so it behooves us to be _really careful_ when writing and testing it to make sure no bugs make it into our production systems. Unfortunately, machine learning bugs are often quite hard to catch, for a couple reasons.

## Language design

First, machine-learning code often depends on complex data-structure manipulation and linear algebra. That means that doing ML in a less-expressive programming language is a huge drag—you end up writing a lot of boilerplate `for` loops, null-value checks, etc. On the other hand, more-expressive languages tend to be less safe to work with, because it’s harder to shoehorn good expressivity into a robust type system.

First, it’s very hard to use [statically typed](http://en.wikipedia.org/wiki/Static_typing#STATIC) languages for machine learning, because your algorithms need to be generic across many different data types, which you might not know until runtime. Try imagining a type-safe version of R’s [Data Frame](http://www.r-tutor.com/r-introduction/data-frame) (or the [Python equivalent](http://pandas.pydata.org/pandas-docs/dev/generated/pandas.DataFrame.html)) to see what I mean.

Relatedly, machine learning involves converting so frequently between different data types (probabilities to generic floating-point numbers, enums to integers, booleans to probabilities, etc.) that one must sacrifice [strong typing](http://en.wikipedia.org/wiki/Strong_typing) even if it is dynamic. For instance, although Python is usually quite strongly-typed, all of the data science libraries I’ve used ([numpy](http://www.numpy.org/), [scikit-learn](http://scikit-learn.org/stable/) and [pandas](http://pandas.pydata.org/)) do an unsettling amount of type coercion.

Finally, even if we could use strong or static typing to catch bugs, machine learning algorithms often involve using a lot of objects that are the same data type (i.e. floating-point numbers), so large classes of errors can get past any sort of type-checking.

## Testing

The typical answer to such errors is to catch them with [unit testing](http://en.wikipedia.org/wiki/Unit_testing), but this too is unusually difficult in machine learning. For one thing, it’s difficult to concoct test data to run your code on. For instance, if I were writing medical imaging diagnostics, I might want to test that my software classified certain “obvious” tumors as cancerous, but I’d have to construct such images first and assure myself that they were really so obvious that _no possible algorithm_ should misclassify them.

Even once you have this test data, it’s sometimes even difficult to determine what exactly counts as a “bug”. If my [support vector machine](http://en.wikipedia.org/wiki/Support_vector_machine) has an [area under the receiver operating characteristic curve](http://en.wikipedia.org/wiki/Receiver_operating_characteristic) of 0.58 instead of 0.63 like I thought it should, maybe it’s just because SVMs won’t work very well on my dataset—but maybe it’s because I messed up my data pipeline somehow and the SVM is getting the wrong features.

## Debugging

Finally, even once you’ve identified a bug in your code it can be hard to figure out exactly what’s going on. Typically programmers find thorny bugs by pausing the code while it’s running and inspecting the value of various variables to see if they make sense. But in an ML algorithm, it’s quite hard to inspect the values like this, both because there are too many of them to just read through and because it’s hard to tell what “makes sense” or not to the naked eye. It essentially becomes a data science/engineering problem just to understand what’s going on inside your algorithm well-enough to debug it.

## Conclusion

Because of these problems, it’s very hard to get the defect rate in machine learning code satisfactorily low with the typical tools for writing high-assurance software. So I’m trying to think of other, less conventional techniques and processes to catch bugs before they make it to production. In a future post, I’ll talk about some of the many exciting types of bugs I’ve experienced so far, and speculate about what can be done to mitigate (some of) them.