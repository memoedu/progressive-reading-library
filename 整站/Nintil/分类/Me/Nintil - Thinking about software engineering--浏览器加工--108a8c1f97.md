Though I'm right now not employed as a software engineer I have been writing code under various hats for the last few years (As a data scientist, ML engineer, and software engineer). Naturally me being me I have not just done the thing but also reflected about the thing. Questions like what's good software, what does being a good software engineer mean, how should meetings be ran, and so on. Here are some thoughts on that.

## Organizational issues

## The cost of disagreement

In many cases a disagreement may be over something that both sides agree is not sizable. I can think something is 80% good whereas you think it's just 75% and I can think the opposite of your solution. But we can agree that the cost to resolve that disagreement can defeat the gains from actually choosing the best solution. So "I disagree but let's go ahead" is something that should be done in these cases, with a coin flip if needed. Consensus-driven decision making may be good at some stages, especially if paired with good processes around meetings but it can lead to gridlock.

I'm of course not the first one in proposing the "[disagree and commit](https://en.wikipedia.org/wiki/Disagree_and_commit)" principle though I only learned this had a name very recently.

## Meetings

Meetings can be terrible, meetings can be great. Bad meetings tend to occur because people show up without having thought much about what is to be discussed in the meeting. Isn't the point of the meeting to do the thinking? That's one way, perhaps the most common way of viewing meetings, and it's a lazy default because they don't impose homework on you. But doing the thinking in real time, with many voices to be sequentially heard and given a constrained time is not great for good decision making. Many times I've seen meetings that dragged on forever where disagreements would keep circling back and forth, or where we would simply be talking past each other.

A better way to run meetings, which I also tried, is to collate whatever is it that the outcome should be before the meeting. If it's an architecture design meeting, do the work, usually the meeting owner can do most of it, even if they know it won't be 100% perfect. Then ask for feedback a week ahead of the meeting. Incorporate the feedback to the document, repeat. Note down disagreements; have a literal "Standing disagreements" section in the document with the key points that are not quite agreed on, plus the reasoning behind them. Having everyone review the disagreements and having time to do so, on (digital) paper sets up a clearer goal for the meeting: Iron out specific points. During the meeting, a moderator would go one by one over those points and ask for a decision, then highlighting in bold which option was chosen. Then we would move to the next point and so on. Everyone liked this kind of meeting.

## Ownership

Lack of ownership is the root of all evil (ok, some exaggeration here). Everyone's problem is no one's problem. In documentation, lack of ownership means finding obsolete documentation and no one person to go to to get it fixed; or what's worse, no system in place to enforce that the documentation is up to date (i.e. every document could disappear 6 months after it's published unless whoever uploaded or created it says otherwise in an email the system sends them after six months, documents shouldn't live rent free). Or having a team dedicated to tirelessly aggregating information so that everything everyone is doing is visible. We had something like this in my first job at the London Electric Vehicle Company (LEVC) and I suspect this may be more common in automotive or aerospace than in software or in biotech.

Apple had a very clever idea in defining [Directly Responsible Individuals](https://twitter.com/michael_nielsen/status/1279478277585817600) (DRIs) for everything. Having a name accountable instead of a vague "the team" or "the process" makes it easy to make changes. I think many people are reluctant to blame individuals for mistakes they make, but well timed blame (feedback about what mistakes were made, potentially there being consequences for grave mistakes etc) can both help the blamed individual (They can know what to improve) and the team as a whole to succeed.

Something I once thought is that many decisions that involve estimates could be made into bets, either for small amounts of money or some kind of token. If you say X will be finished in a week and it takes longer, then you lose. This could be gamed by overestimating how long things take, but something like this seems like the right way to get better at making decisions involving uncertainty.

## Questions as systemic failures

Every question asked in an internal Slack is a policy failure. It means the existing information systems failed to deliver an answer, and the user falls back to manually asking the hive mind's tacit knowledge. This has various problems: One, it introduces longer delays between question and answer, especially if whoever knows the answer is in another timezone. Two, it embraces tacit knowledge in a distributed and incoherent fashion: If there is no one true answer there can be many answers and that can lead to disagreements and wrong decisions. Instead, ideally there is a centralized repository of information where for each Q there is one and only one A, and a team dedicated to getting owners of various systems to actually codify their knowledge. This should work well with the ownership system above.

## Coding

## Good software

Good software is code that is readable, fast, flexible, and scalable. Out of these only speed is the one that is universally agreed on how to measure. The rest are fuzzy, as are most things in life.

Readable code pretty much depends on who is writing it; [Dyalog](https://github.com/jayfoad/aoc2019apl/blob/master/p12.dyalog) looks like [Brainfuck](https://en.wikipedia.org/wiki/Brainfuck) to me, and the many parenthesis in Lisps can make code hard to read to someone who is not a lisper (I did the experiment; I spent some time learning basic Clojure and while I remain not a Clojurian, the parenthesis become less of an issue). Lifetimes in Rust seem obscure until one know how they work.

Flexible code is code that is easier to extend. This is hard to quantify but anyone that has coded knows it when they see it; a given piece of code can just effortlessly do something new with a two line change, or it may need a thousand line change to work again. The former is more flexible than the latter. Moreover this flexibility shouldn't come at the expense of readability though sometimes this can be the case.

Scalable code is code that works well with small as well as big inputs, this can be achieved in a single machine or many.

The extent to which these matter depend on who is developing it (Readable code depends on individual preferences), and flexibility is not really needed if the end result is more or less fixed; but it's really desired in a startup that is constantly adapting. In that environment, speed may be sacrificed for extra flexibility.

The code that should be written absent any constraints is good code, but real life situations means that the right thing to do is to make tradeoffs and move ahead. Those decisions are at the heart of what experience in software engineering is.

## Static types

Static typing is great. Early on in a codebase in Python back at Aiden.ai, we made the decision of going for static typing using myopia as much as we could. So instead of writing something like

```python
def sum_one(x):
  return x+1
```

We would rather much write

```python
def sum_one(x:int)->int:
  return x+1
```

Or even further, in some cases we would use _newtypes_ to make these type annotations more meaningful, dataclasses to bundle data together, as well as exhaustive enumerations to ensure that all variants of an enum get handled, for example:

```python
class Operation(Enum):
  Multiply="Multiply"
  Add="Add"
Value = Union[int,float]
def assert_never(x: NoReturn) -> NoReturn:
    raise AssertionError(f"Invalid value: {x!r}")
def do_the_op(a:Value,b: Value, op:Operation) -> Value:
  if op is Operation.Multiply:
    return a*b
  else if op is Operation.Add:
    return a+b
  else:
    assert_never(op)   
  
```

So if we ever say remove an operation or add a new one, mypy will force us to handle it. This is enforced dynamically but most importantly also statically so the code won't pass tests if there is a missing variant.

All these typing (Plus a custom pandas typechecker I wrote, but that's another story) made it relatively easy to refactor, and add new features when we needed to do so. It would have been a huge pain to fly blind without the types. Python has a tendency to blow up in your face when you least expect it. Starting with types from day 1 is something I don't regret having gone for, it didn't make the coding any slower and saved a lot of time (Or so we imagine!).

There seems to be a trend now towards types everywhere. Javascript died (for any serious developer) to let Typescript rise and Ruby, while still around with the Sorbet type checker, got Crystal.

As a [wise man](https://en.wikiquote.org/wiki/Theodore_Kaczynski#:%7E:text=Consistent%20failure%20to%20attain%20goals,low%20self%2Desteem%20or%20depression.&text=text%20on%20wikisource-,The%20Industrial%20Revolution%20and%20its%20consequences%20have,disaster%20for%20the%20human%20race.&text=The%20industrial%2Dtechnological%20system%20may%20survive%20or%20it%20may%20break%20down.) once said, _Python and its consequences have been a disaster for the human race_. Python has a tendency to blow up in your face, even with all the typing. The typechecker may be happy but it doesn't guarantee that if it thinks something is type T it's actually so, maybe what you thought was a number is actually a string and because of duck typing it can take a few function calls for the error to manifest itself.

## YOLO programming vs chill programming

I believe there is a tradeoff between writing a lot of code and writing correct code. In a day you could write X lines of code or you could write X/2. Two days of programmer B will produce the same as one day of programmer A but probably the latter will have introduced fewer bugs. You can write more or less tests, you can be more or less sure that something you just wrote is actually correct.

As you can expect from the number of typos here, I'm more of a YOLO programmer than a chill programmer. I'm the guy that was once pushing to prod from a one-handed GitHub hot fix from my phone as I was having dinner in a hotel in Tokyo (That one did work!).

YOLO programming when the programming language doesn't have your back is more problematic and conversely it greatly benefits from strongly typed languages (Like Rust; probably of functional programming as well). While the "if it compiles it works" idea is not quite true, it is more true than "if it typechecks it works" in Python. In the Python case the code may seem "good enough" to you as well as the (insufficient) tests and the (imperfect) type checker, and all it takes for bugs to slip through is that code review is not perfect. In the Rust case no amount of "It seems right to me" will make `rustc` happy. Many errors (Logic errors) can be hard to avoid, but in innumerable times I've seen bugs getting through that would have been caught by proper types.

More code is buggier code (on average) and so another way to do safer YOLO programming is designing the code so that fewer lines of code need to be added over time. How to do this? If you can pull it off, DSLs and [compilers](https://twitter.com/ArtirKel/status/1247257768408633344) for them. A DSL is (if done well) a concise, readable, modular way of describing business logic for the domain of interest, and making as much invalid possibilities unrepresentable. Back at Aiden.ai instead of writing lots of data pipelines, I ended up writing a general data transformation system that would fetch configuration at runtime and transform data arbitrarily. That way we could plug new data sources into a common framework with relatively little work.

## The short vs long function debate

How long is a long function? A program could be a single, very long function. Or it could be lots of tiny functions that each do one absurdly simple thing. What's the best? Should you inline heavily? Should you follow the "Single Responsibility Principle" to its ultimate consequences. I personally like longer functions that can be read top-down; even if a pattern repeats itself within a function, rather than defining a new function outside the main function I would rather either use closures or write this new function inside the original function. The fewer functions you can grab the better, it makes it easier to find what you are looking for. Having lots of smaller functions makes it more difficult, at least to me, to track what the larger body of code is doing, and when deleting old code, it's hard to miss these auxiliary functions. Absent tooling to detect dead code that is not being called, they can linger there and be maintained over time even though they serve no purpose anymore. Shorter functions can also hide what the code is really doing. What if the innocent `getPersonFromId` is doing a database call? Are you going to map over and spam the database with calls? Or... write a new function that does a single call? With fewer layers of abstraction this is easier to see.

But short function defenders will argue that they make the code more readable. I can see how that could be in some cases, and that made me realize that it probably has to do with cognitive differences and values at various levels. How much working memory do you have, or preference for concreteness vs abstraction, or how much do you trust the code you are reading? Ultimately the answer is that the right function length is a function of the team working on a given codebase.

## Premature optimization

Premature optimization is said to be the root of all evil. But late optimization can also be harmful. Imagine you get to a point where everything just feels slow. You profile the causes and turns out it's everywhere: The database is not properly indexed, the queries are not optimized, queries are made repeatedly, or slow algorithms or functions were written. It can be tempting at that stage to just throw clouds at the problem and replicate the database and spin up 4 more servers. Now you have to pay for all that and deal with database synchronization. I think there is a reasonable middle ground of designing with future performance needs in mind, instead of making it a complete afterthought.

## Programming books & resources

I've read many books about software. A [philosophy](https://www.amazon.com/Philosophy-Software-Design-John-Ousterhout/dp/1732102201) of software design, [clean code](https://www.amazon.com/Clean-Code-Handbook-Software-Craftsmanship/dp/0132350882/ref=sr_1_1?dchild=1&keywords=clean+code&qid=1609459157&s=books&sr=1-1) and so on. I've also watched lots of talks about software. For some reason I ended up appreciating the points of view of game developers ([Jonathan](https://www.youtube.com/watch?v=ZSRHeXYDLko) [Blow](https://www.youtube.com/watch?v=21JlBOxgGwY), [Casey](https://caseymuratori.com/blog_0015) Muratori, John [Carmack](http://number-none.com/blow/john_carmack_on_inlined_code.html)) a lot. I guess part of it is their "It depends" approach to many of these problems. One can recognize the advantages of functional programming or of having classes here and there, or of writing unit tests without making everything into a pure function, going full OOP, or spending days testing code that could do with a shorter test suite. If you take say [SOLID](https://en.wikipedia.org/wiki/SOLID) which sounds kind of reasonable, the Single Responsibility Principle in an OCD way can only be met by [unary functions](https://en.wikipedia.org/wiki/Unary_function). Everything else must be doing more than "one thing". Things that seem like hard rules in programming run afoul of the [nebulosity](https://meaningness.com/nebulosity) of the art of programming.

What do we really know about programming? Well, evidence-based software engineering is a thing. There are [books](http://www.knosof.co.uk/ESEUR/) about it. There are [talks](https://www.youtube.com/watch?v=WELBnE33dpY) about it. But it's not a huge thing. Software engineering is like education in that is a huge part of modern society yet there is little resources devoted to making it better, unlike for the life sciences say. Twitter was originally written in Ruby and then there was a transition away into Scala. Many people these days use Kubernetes-backed micro service architectures rather than monoliths. Is that a good thing? Who knows. My guess is that probably monoliths are underrated.