[![](https://michaelnotebook.com/assets/home.png)](https://michaelnotebook.com/index.html)

/Working notes by Michael Nielsen, released September 30, 2021. Rough and incomplete. Requires a basic background in general relativity. My own understanding of general relativity is very patchy, so correction of misapprehensions is appreciated. There's also a lot I don't understand about the Alcubierre solution, so these notes contain many questions, as a placeholder for future work./

Intuitively, we think of space and time as fixed – an arena for action, but not the action itself. But in general relativity this is not the case: spacetime changes in response to mass and energy and motion; it is a mutable, dynamic quantity. It's as though the action of the players changes the arena in which they play.

In this viewpoint, fundamental questions are: what spacetimes are possible? What spacetimes can we, in principle, engineer? Or, from a different point of view: what spacetimes can we design and then realize[1](https://michaelnotebook.com/wn/warp_drive.html#fn1)?

I do not know the answer to these questions. However, in these brief working notes I review a stimulating 1994 proposal for a striking spacetime, from the physicist Miguel Alcubierre[2](https://michaelnotebook.com/wn/warp_drive.html#fn2).

It is well known that in special relativity the ability to travel faster than light leads to paradoxes – the ability to send information backward in time, to violate causality, and so on. For this reason, faster-than-light travel is usually regarded as forbidden.

Of course, special relativity is only an approximate description of reality. A better description of the universe is provided by general relativity, and there the situation is more complicated. It is true that locally it's not possible to travel faster than light, in general relativity. However, the mutable nature of spacetime in general relativity means that spacetime itself may expand and contract. As Alcubierre notes in motivating background discussion, this effect can be used to achieve what seems (from a global point of view) like faster-than-light travel:

> The basic idea can be more easily understood if we think for a moment in the inflationary phase of the early Universe, and consider the relative speed of separation of two comoving observers. It is easy to convince oneself that, if we define this relative speed as the rate of change of proper spatial distance over proper time, we will obtain a value that is much larger than the speed of light. This doesn't mean that our observers will be travelling faster than light: they always move inside their local light-cones. The enormous speed of separation comes from the expansion of spacetime itself.
> 
> The previous example shows how one can use an expansion of spacetime to move away from some object at an arbitrarily large speed. In the same way, one can use a contraction of spacetime to approach an object at any speed. This is the basis of the model for hyperfast space travel that I wish to present here: create a local distortion of spacetime that will produce an expansion behind the spaceship, and an opposite contraction ahead of it. In this way, the spaceship will be pushed away from the Earth and pulled towards a distant star by spacetime itself. One can then invert the process to come back to Earth, taking an arbitrarily small time to complete the round trip.

In particular, Alcubierre constructs a spacetime which has a little moving “bubble” of space inside it. To restate the above in my own terms: this bubble kind of “pulls” on the surrounding region of space, contracting the region in front of its motion, and expanding the region behind. The result, Alcubierre showed, was that the bubble (and its contents) can follow essentially any trajectory you want, including trajectories faster-than-light.

It should be noted up-front: Alcubierre's solution is very likely not realizable in our universe. We'll discuss the reasons below. However, it's become something of a crank magnet: any time someone says something about faster than light travel being impossible, someone will say: “But what about the Alcubierre drive?” And, so far as we can currently tell the correct response is: “Yes, so what about the Alcubierre drive? It requires exotic matter with properties we believe are likely to be unphysical”. But with that said: I learned a lot by studying the Alcubierre solution as an example of spacetime design.

Alcubierre imagines a spaceship moving along the x axis of some Cartesian co-ordinate system. In that co-ordinate system the position of the spaceship at time t is given by x\_s(t), and the velocity is v\_s := dx\_s/dt. It's convenient to define the Euclidean distance r\_s from the spaceship to an arbitrary point (x, y, z): r\_s := \\sqrt{(x-x\_s)^2+y^2+z^2)}

We're going to enclose the spaceship in a little bubble of size R > 0. Alcubierre then writes down a metric to describe his spacetime (G = c = 1): ds^2 = -dt^2 + (dx-v\_s f(r\_s)dt)^2 + dy^2 + dz^2.

In this metric the only bit we haven't yet specified is the function f(\\cdot). It's chosen so that f(r\_s) \\approx 1 when |r\_s| < R and f(r\_s) \\approx 0 when |r\_s| > R. That is, it switches on or off depending on whether we're inside or outside the bubble. The exact form of f (probably) doesn't much matter, provided it has these properties[3](https://michaelnotebook.com/wn/warp_drive.html#fn3). Alcubierre chose the particular form: f(r\_s) := \\frac{\\tanh(k(r\_s-R))+\\tanh(k(r\_s+R))}{2 \\tanh(kR)}, where k is some large positive constant. Roughly speaking, 1/k is the range of values of r\_s near R where the function f(\\cdot) switches from 0 to 1.

How to think about the Alcubierre metric?

-   Outside the bubble, this is just flat spacetime.
    
-   Inside the bubble the metric is approximately: \-dt^2 + (dx-v\_s dt)^2 + dy^2+dz^2. I don't understand how to think about this very well. At first, I wondered about the situation when v\_s > 1, i.e., the situation when the bubble is moving faster than light. In particular, we see terms like (v\_s^2-1)dt^2 in the metric, which made me wonder if the metric even has a Lorentzian signature. Fortunately, direct computation shows that this is fine – the cross-terms between dx and dt keep the signature Lorentzian.
    
-   Along the spaceship trajectory the co-ordinate time t is (approximately) equal to the proper time \\tau. One way to see this is to consider a small piece of the spaceship trajectory inside such a bubble, and do a direct computation – the reason is that the (dx-v\_s dt) term will vanish, and the dy and dz terms of course vanish along a trajectory on the x axis.
    
-   Alcubierre claims, though I have not checked, that the trajectory x\_s(t) of the spaceship is a geodesic in this spacetime[4](https://michaelnotebook.com/wn/warp_drive.html#fn4). That is, if we had a test particle in this spacetime, it would freely fall along this trajectory. This is true, even if the velocity v\_s exceeds the velocity (c = 1) of light.
    
-   Outside the bubble the co-ordinate time t is _also_ equal to the proper time \\tau for observers stationary with respect to the Cartesian co-ordinates. This has a nice consequence: the freely falling spaceship is experiencing no time dilation at all with respect to such stationary observers!
    
-   Both inside and outside the bubble the tidal forces are near-vanishing. On the boundary of the bubble, well, something pretty peculiar is going on, and one would expect there to be very strong forces ripping apart any test particle that attempted to cross the boundary.
    

All that said, I don't understand this form of metric very well. I don't know how Alcubierre could have dreamt it up, and I don't know how to think very well about it. But that set of properties is certainly very striking!

Lots of followup questions and observations:

-   The metric automatically implies a matter distribution – use the metric to compute the Einstein tensor, then the field equations tell you the stress-energy tensor. Alcubierre does this computation to find an energy density: \-\\frac{1}{8\\pi} \\frac{v\_s^2 \\rho^2}{4 r\_s^2} \\left( \\frac{df}{dr\_s} \\right)^2 where \\rho := \\sqrt{y^2+z^2}. This energy density is evidently _negative_, and so must require some exotic form of matter.
    
-   Because the energy density is negative, it doesn't correspond to any known form of matter. This is a (major!) blow to anyone hoping to use these ideas to achieve effectively faster-than-light travel. Reducing the requirement for negative energy appears to have been a fun theme of followup papers. I do not know if it can be proved that negative energy density is required for a scheme like this to work. Alcubierre's inflationary example gives some hope that it is not.
    
-   Is there any way of finding similar solutions with physically realistic matter?
    

## On the freedom to choose spacetime

Something which bothered me a lot upon a first read of the paper: Alcubierre starts by specifying his spacetime and studying its properties. Only later does he compute the stress-energy tensor. I found this rather surprising – usually in general relativity I'd think in terms of some distribution of matter and energy, and try to use it to figure out the structure of spacetime.

Further reflection bothered me even more. In particular, it seemed to me that there was something of a conflict between an initial value formulation of general relativity, and the ability to simply specify a spacetime metric everywhere (as Alcubierre does).

In particular, I assumed that if you specify the metric and stress-energy tensor (and derivatives) on some initial spacelike hypersurface, that would determine the future of both metric and matter. However, if that's true, it's _not consistent_ with having the ability to freely specify the metric everywhere. The reason is that if you know the metric everywhere, then you can use the Einstein field equations to figure out the stress-energy tensor on a spacelike hypersurface. That would then determine the future of both the metric and of the stress-energy tensor. In other words: if the initial value problem is well-formed, then it destroys later freedom to choose the metric.

Put in more physical terms: if you know the metric and matter (and how they are changing) at some initial time, doesn't that tell you everything about the metric and matter at later times? And doesn't that mean you're not free to choose the metric later on?

I asked [on Twitter](https://twitter.com/michael_nielsen/status/1443375896329527296) about these concerns, and received illuminating and helpful replies from John Baez, Sean Carroll, Geraint Lewis, Markus Pössel, Steinn Sigurðsson, and several other people.

Some things I learned from them (mistakes are almost certainly mine), and some questions I still have:

-   It is, indeed, possible to simply specify a spacetime. The distribution of mass-energy then follows from the Einstein field equations.
    
-   The Bianchi identities plus the Einstein field equations imply conservation of mass-energy.
    
-   My formulation of the initial value problem above is _not correct_. In particular, they state that you need additional information to say how matter will behave. In particular, Baez stated: “Einstein's equation _together with the extra equations describing matter fields_ should have a unique solution given initial data. But here we don't have any equation describing matter fields. The matter is just some mysterious gunk that does whatever it's told!”
    

Markus Pössel gives a nice example of figuring out “whether some star is stable under its own gravity, you need to specify the stabilizing pressure within the star - and that pressure will be a source of gravity that you need to take into account.” In particular, there will be some equation of state relating pressure and density within the star, and you need that equation of state (on top of the Einstein field equations) to understand the behaviour. Different equations of state will then give rise to different types of behaviour! So the Einstein equations alone are not enough to determine the behaviour of the (potentially) collapsing star.

Of course, what's going on here is that really there's a (hidden) force underlying that equation of state. It could be electromagnetic, for instance, or perhaps Fermi pressure. The point is: the Einstein equations remain true, even in the presence of other fields, though those fields may contribute to the stress-energy tensor. I'm sure this is a point made repeatedly in every introductory general relativity class, but I must admit it's a point I hadn't really understood before the kind people of Twitter helped me understand.

Having cleared up one confusion, let us move on to more advanced confusions. I think of general relativity as a replacement for Newton's theory of gravitation. And in Newton's theory, if you're given a set of purely gravitating bodies, you can write down a force law for each, and then (in principle) integrate forward in time to find the future behaviour. In practice, you usually can't do this – you'll find very sensitive dependence on initial conditions. But it is in principle possible.

(It's tangential, but realizing this was one of the most astounding moments ever for humanity. I can understand why Newton named the final volume of _Principia_ “The System of the World”.)

Given all this, my thinking was that there should be something analogous in general relativity, some sort of system of “purely gravitating bodies”, like a set of point masses in Newtonian gravity. And for such a set of bodies it should be possible to uniquely solve the initial value problem. But as nearly as I can see, there is no obviously analogous statement in general relativity. Let me just formalize that a bit:

**Question:** In pure Newtonian gravity, given a set of point masses, positions and velocities, it's possible to integrate forward in time to find the future state of the system. Is there an analogous statement true in general relativity, without needing to introduce additional equations beyond the Einstein field equations?

I believe the answer is no – you need to say more about the nature of the masses. So that's an interesting change in my understanding of general relativity!

There's another point of confusion for me that may well be related. In general relativity you really can deduce from the field equations that test particles travel along geodesics of spacetime. (If I recall correctly this is done in Dirac's book about general relativity, for instance.) You don't need extra assumptions about the nature of the particle, or any kind of equations governing the matter. This is very useful in practice: you can understand things like the perihelion effect, for instance, this way, or (more generally) much about the motion of the planets around the sun.

In a similar vein, I don't understand how to square all this with Wheeler's famous summary of general relativity: “spacetime tells matter how to move; matter tells spacetime how to curve”. In fact, it sounds as though the situation is really more like: matter tells spacetime how to curve, and spacetime plus a bunch of other contingent things tells matter how to move. Not as pithy! But has the benefit that it seems to actually be accurate for things which are not test particles.

(It's tempting to think the Wheelerism might be true for test particles, at least. But the point of test particles is that they aren't affecting spacetime. All that's happening with the test particles is that spacetime is telling matter how to move; the matter isn't telling spacetime anything at all. The situation is rather frustrating, since with non test particles, matter _is_ telling spacetime how to curve, through the field equations. So the Wheelerism is never really true.)

There are several natural followup questions to all this:

-   What spacetimes can be realized? Intuitively, it seems that most spacetimes and stress-energy tensors require extremely unnatural and bespoke interactions – a kind of programmable matter-and-interactions, indeed, effectively, a kind of programmable spacetime. My conjecture is that with a little work you can probably figure out a programmable design for matter which leads to any spacetime and stress-energy tensor consistent with relatively mild constraints on the stress-energy tensor (alas, including non-negative energy density) and the Einstein field equations.
    
-   In the particular case of the Alcubierre metric, what additional equations for matter would be needed to realize the Alcubierre stress-energy tensor?
    
-   Returning to the general case of an arbitrary spacetime: given the stress-energy tensor, how much freedom is there in the spacetimes giving rise to that stress-energy tensor? There must be quite a bit of freedom, given that there are many vacuum solutions to the field equations.
    

All told, then, my understanding is certainly quite limited. Still, it's very clever of Alcubierre to have constructed this little bubble that can move in an arbitrary way on the x axis, while: (a) cocooning a geodesic; (b) having arbitrary speed; and (c) where proper time along the geodesic matches proper time for external observers. But even given a source of exotic matter with negative mass it's not obvious to me how you set this up, how you maintain it (i.e., does it need special interactions to main), or how you enter or exit the bubble.

## Further Questions and possible TODOs

-   Could we write discovery fiction for the Alcubierre spacetime?
    
-   How should I think about a metric with off-diagonal terms mixing time and space? This is a very elementary question, but it's been years since I thought about general relativity – I need to recover intuition on this point.
    
-   Is there an easy way to see that the trajectory of the spaceship is a geodesic in the Alcubierre spacetime?
    
-   How to create the bubble in the first place? It's perhaps something of a moot point, since we don't have negative mass! But if we did, how would you do it? Presumably one would need to use other fields to get the correct initial (negative) matter distribution, using electromagnetic (or other) fields to “push” the negative mass into the correct configuration[5](https://michaelnotebook.com/wn/warp_drive.html#fn5). But I don't understand the details at all. Stein Sigurðsson said on Twitter [that as far as he knows](https://twitter.com/steinly0/status/1443384929602228233), it's still unsolved how to set up the bubble.
    
-   How to get the spaceship in and out of the bubble, without being torn apart? Or is it, perhaps, possible to build the bubble around the spaceship – perhaps gradually building up negative mass nearby? Presumably the reverse process could be used to get the spaceship out, though that would (likely) require a high-tech civilization at the destination.
    
-   In what sense is it necessary that spacetime be expanded behind the spaceship? There's a [standard image](https://commons.wikimedia.org/wiki/File:Alcubierre.png) used in discussions of the Alcubierre spacetime, but I don't understand in any detail how to connect it to the actual metric.
    
-   Is negative energy density required for a “warp drive”-like scheme to work? Related: Baez pointed out several nice papers, including an interesting-looking paper by [Chris Van Den Broeck](https://arxiv.org/abs/gr-qc/9906050) on various constraints that make warp bubbles unlikely to be realized.
    
-   Alcubierre worked within – and seems to have been strongly motivated by, and used intuitions based on – the [ADM formalism](https://en.wikipedia.org/wiki/ADM_formalism). This isn't a formalism I have used. How does it work? What does it make easy to understand that are not so easy when the ADM formalism is not used?
    

* * *

1.  An [article](https://ui.adsabs.harvard.edu/abs/1985nuas.conf..176Y/abstract) with the title “Spacetime Engineering” was written in 1985 by the mathematical physicist James York. Unfortunately I haven't been able to locate a copy of the article. If anyone can send it to me, I'd appreciate it (mn@michaelnielsen.org).[↩︎](https://michaelnotebook.com/wn/warp_drive.html#fnref1)
    
2.  Miguel Alcubierre, [The warp drive: hyper-fast travel within general relativity](https://arxiv.org/abs/gr-qc/0009013) (1994).[↩︎](https://michaelnotebook.com/wn/warp_drive.html#fnref2)
    
3.  We'll see later that this form implies the need for substantial negative masses, and large tidal forces when crossing in or out of the bubble. It's interesting to ponder whether this may be ameliorated by choosing some other form for f(\\cdot).[↩︎](https://michaelnotebook.com/wn/warp_drive.html#fnref3)
    
4.  This point is apparently trivial to see in the particular formalism Alcubierre uses (the ADM formalism). But since this formalism is unfamiliar to me, it doesn't much help to know this! Still, it should be easy enough to prove through a direct computation, even without any slick idea.[↩︎](https://michaelnotebook.com/wn/warp_drive.html#fnref4)
    
5.  Note that negative masses naturally _repel_ one another gravitationally. This makes agglomerating them somewhat challenging – they don't naturally fall together, the way ordinary mass does. To get them to clump would require other forces to overcome the gravitational repulsion.[↩︎](https://michaelnotebook.com/wn/warp_drive.html#fnref5)
    

## Comments