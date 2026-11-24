[![](https://michaelnotebook.com/assets/home.png)](https://michaelnotebook.com/index.html)

_Very quickly written rough working notes. Written by someone nonexpert on the_ _topic, filling in a gap in their physics education. Release and last revision: 11-19-2021._

> Feynman: I am Feynman.
> 
> Dirac: I am Dirac. (Silence.)
> 
> Feynman: It must be wonderful to be the discoverer of that equation.
> 
> Dirac: That was a long time ago. (Pause.) What are you working on?
> 
> Feynman: Mesons.
> 
> Dirac: Are you trying to discover an equation for them?
> 
> Feynman: It is very hard.
> 
> Dirac: One must try.
> 
> – _From James Gleick's biography of Feynman, "Genius"_

One big gap in my education as a physicist was that I never understood the Dirac equation – the fundamental equation we use to understand the electron(!) This is, to put it mildly, a pretty big gap. So yesterday afternoon I decided to fill the gap in.

One reason I struggled as a student was that in the accounts I saw the Dirac equation was presented as a mish-mash of historical, deductive, and motivating reasoning. Mixed in was a bundle of important new physical and mathematical ideas. I found the resulting combination difficult to internalize, and over the years gradually forgot it all.

In these notes I thought it would be fun to present the Dirac equation in the form of what I call [discovery fiction](https://cognitivemedium.com/srs-mathematics). As I say at that link: "Discovery fiction starts with the question \`how would I have discovered this result?' And then you try to make up a story about how you might have come to discover it, following simple, almost-obvious steps." Of course, for something like the Dirac equation this typically requires some considerable background (or the discovery fiction gets very long). In this case, I'll assume you're comfortable with basic non-relativistic quantum mechanics. I'll also use a little quantum information at one point.

In the event, I don't think what I've written below is that much different than conventional accounts, and it's certainly not entirely successful as discovery fiction. But it was fun to try, and I certainly improved my understanding!

(Experts familiar with the Dirac equation can expect to learn exactly nothing from these notes! In the event they for some reason perservere they may, however, spot some misunderstandings or errors in my account, in which case they are welcome to comment below.)

So, the problem the Dirac equation addresses is: how to give a quantum mechanical description of the electron? In particular: how to give a quantum mechanical description consistent with special relativity?

The Dirac equation was discovered in 1928, a few years after Schroedinger and Heisenberg invented quantum mechanics. Unfortunately, while Schroedinger and Heisenberg had achieved great success in describing non-relativistic systems, their work hadn't yet achieved similar success in a relativistic setting. Dirac remedied this situation.

Schroedinger had, of course, obtained the famous Schroedinger equation describing the time evolution of the quantum state,

i \\frac{d|\\psi\\rangle}{dt} = H |\\psi\\rangle,

where H is a quantum mechanical operator playing a role analogous to the energy (or Hamiltonian) function in classical mechanics. The Hamiltonian function for a free particle in non-relativistic (classical!) mechanics is given by

H = \\frac{p^2}{2m},

where p is the momentum of the particle, and m is the mass. Schroedinger guessed – for reasons we won't go into! – that if you work in the position basis, p^2|\\psi\\rangle becomes \-\\nabla^2 \\psi(x,t). And so the Schroedinger equation for a free (non-relativistic) particle of mass m is:

i \\frac{\\partial \\psi(x,t)}{\\partial t} = -\\frac{1}{2m} \\nabla^2 \\psi(x,t).

In 1905 Einstein had showed that the non-relativistic equation for energy, H = p^2/2m, needs to be modified in special relativity. In particular, in special relativity, we should use (units where c = 1):

H = \\sqrt{p^2+m^2}.

Note that when |p| \\ll |m| this is just H = m+p^2/2m, i.e., it reduces to the non-relativistic case, with an extra constant m that has no physical impact on the equations of motion. If you simply plug Einstein's expression into the Schroedinger equation, you get:

i \\frac{d|\\psi\\rangle}{dt} = \\sqrt{p^2+m^2}|\\psi\\rangle. \\,\\,\\,\\, (A)

It's not immediately obvious how to work with this in the position representation – how to take the square root of things like \\nabla^2? But if we work in the momentum representation, \\phi(p,t) := \\langle p|\\psi(t)\\rangle this is a perfectly respectable equation:

i \\frac{\\partial \\phi(p,t)}{\\partial t} = \\sqrt{p^2+m^2} \\phi(p,t).

Now, I asked on Twitter why Dirac didn't use this equation. Several people replied that they thought the equation probably generated non-local solutions. Matħeus Velloso pointed to [this paper](https://aip.scitation.org/doi/abs/10.1063/1.530015?journalCode=jmp) which explores the issue in detail. And John Baez pointed out that in fact the equation is used to describe massive spin-zero particles – so, a perfectly good equation, but not suitable for the electron.

(Aside: quite a few people made statements like "this is just the Klein-Gordon equation". But it's not really. For one thing, this equation only has non-negative energy solutions. For another, the Klein-Gordon equation has two time derivatives, and needs \\partial \\phi/\\partial t specified as initial data, unlike the above equation. So while it's certainly related – any solution to the above will, for instance, automatically satisfy the Klein-Gordon equation, though not vice versa – it's actually not the same equation.)

I don't know exactly what Dirac realized of all this – for reasons perverse I've done no more than glance at Dirac's papers[1](https://michaelnotebook.com/wn/dirac.html#fn1). But let's assuume he wasn't happy with this solution to equation (A) for some reason. What else might you do?

Dirac's solution is incredibly audacious and imaginative. Let's look at equation (A) again:

i \\frac{d|\\psi\\rangle}{dt} = \\sqrt{p^2+m^2}|\\psi\\rangle. \\,\\,\\,\\, (A)

Of course, when you're dealing with matrices there's more than one way to take a square root. To jump into a quantum information context for a moment, suppose you're dealing with two qubits. Then, for instance, X \\otimes Y is a perfectly good square root for I \\otimes I. The point being: the matrix degree of freedom gives you a lot of different possible square roots.

Dirac simply _guessed_ that maybe he should add in an additional degree of freedom to his system[2](https://michaelnotebook.com/wn/dirac.html#fn2), and this might help him construct a square root with helpful physical properties in equation (A). In modern terms, we'd say that he should describe an electron's state as the tensor product of the usual (position-space) Hilbert space with some other state space. We'll eventually see that it'll be useful to choose the other state space to be four-dimensional, though for now we'll leave the choice of state space up in the air. And then Dirac hoped that he could find matrices \\gamma^0, \\gamma^1, \\gamma^2, \\gamma^3 acting on the new state space so that:

\\left( m I \\otimes \\gamma^0 + p\_x \\otimes \\gamma^1 + p\_y \\otimes \\gamma^2 + p\_z \\otimes \\gamma^3 \\right)^2 = m^2 + p^2.

By squaring the left-hand side you see that you need (\\gamma^j)^2 = I for all the \\gamma matrices. You also need that \\gamma^j anticommutes with \\gamma^k whenever j != k. If we could find such a set of matrices, then you could choose the square root in equation (A) so that the Schroedinger equation would become:

i\\frac{d|\\psi\\rangle}{dt} = \\left( m I \\otimes \\gamma^0 + p\_x \\otimes \\gamma^1 + p\_y \\otimes \\gamma^2 + p\_z \\otimes \\gamma^3 \\right) |\\psi\\rangle.

This is, in fact, the Dirac equation(!) We just need to find suitable matrices. We have the constraints (\\gamma^j)^2 = I and \\{\\gamma^j, \\gamma^k \\} = 0 when j != k. It also makes sense to impose the constraint that \\gamma^j is Hermitian, to ensure the Dirac Hamiltonian is Hermitian.

Unfortunately, it's not possible to find 2 \\times 2 matrices satisfying these criteria. It's an easy exercise to show the anticommuting matrices we seek must be linearly independent and traceless. The space of traceless 2 \\times 2 Hermitian matrices is only 3\-dimensional, so it's not possible to find four 2 \\times 2 matrices satisfying these conditions.

What about 3 \\times 3 solutions? There, I don't see a really quick argument that it's not possible to find suitable matrices, though I gather that it it is not. In any case, the standard solution is to choose the following 4 \\times 4 matrices, known as the Dirac algebra:

\\gamma^0 = Z \\otimes I; \\,\\, \\gamma^1 = iY \\otimes X; \\,\\, \\gamma^2 = iY \\otimes Y; \\,\\, \\gamma^3 = iY \\otimes Z.

Other solutions may easily be found as well. Apparently, in four dimensions, these can be shown to be unique up to a similarity transformation. I do not know the significance of higher-dimensional solutions, though of course such are easily constructed.

I will leave the notes there. That was, in fact, really very simple (and makes me wonder why I had trouble as a student). Of course, there's far more that could be said – finding solutions to the equation, understanding the physical significance of the solutions, and so on. But I feel pleased just to have internalized the basic equation.

* * *

1.  The obvious thing to do is to read the papers in detail. I typically try to avoid that while writing discovery fiction, at least until as late as possible. In this case, I should probably just bite the bullet.[↩︎](https://michaelnotebook.com/wn/dirac.html#fnref1)
    
2.  This didn't come completely out of nowhere. The previous year Pauli had guessed an equation for an electron which also added in an extra degree of freedom.[↩︎](https://michaelnotebook.com/wn/dirac.html#fnref2)
    

## Comments