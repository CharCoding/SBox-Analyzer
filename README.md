# [SBox-Analyzer](https://charcoding.github.io/SBox-Analyzer/)
This tool performs basic linear and differential analysis on any 8-bit substitution box.  
This tool is only made for educational purposes and is not meant to be used in serious cryptographic applications. (If you can't tell, I've been citing stuff from last century...)  
Feedback and pull requests are welcome. Sorry you had to go through my spaghetti code.

## Update 2025
I've learned a few things about 8-bit substitution boxes, other than the fact that they're becoming less common in favor of ARX and other constructions:
- The affine transformation AES uses should have more degrees of freedom - namely, one can use any 8x8 invertible binary matrix (of which there are $\prod_{i=0}^7(2^8-2^i)$ different matrices). AES just uses mod by `0x101` because it can be easily expressed by bit rotation and XOR.
- Even if we stick to multiplying by a polynomial and then taking the modulus by another polynomial of degree 8, the modulus does not have to be `0x101` or $x^8+1$, and the multiplier does not have to have odd number of terms. They can be anything as long as the GCD of the two polynomials is 1.
- Taking the inverse in $\text{GF}_2^8/P(x)$ isn't the most general way of describing the nonlinear transformation. Taking the inverse is equivalent to taking the field element to the 254-th power, with the convenience that $0^{254}=0$ is already well-defined. The best nonlinearity and differential bias are achieved at $255 - 2^i$ where $i\in\{0,1,...,7\}$, i.e. iff the power has Hamming-weight 7.
  - These powers should be identical in terms of their desired statistical properties. I could be wrong on this.
  - Using any other power causes differential bias (and often linear bias also) to increase.  

However, with this comes new questions:
- When using matrix multiplication for both before and after the Galois Field exponentiation, will there be two configurations that generate the identical SBox?
  - I think every configuration should have unique SBoxes, but I don't know for sure. There are about $1.87\times10^{42}$ different valid configurations now, using 2 matrices and 2 vectors and fixing the power and the irreducible polynomial (only $8\times30$ options in these).
- Are there yet other optimal SBoxes that are unique from all affine transformations of Galois Field exponentiation?
  - From my understanding, Maiorana-McFarland construction cannot be applied to a 8-bit to 8-bit SBox (input needs to be twice as large as output), so I don't know if it can be tweaked for this tool to generate.
  - But, there are $256!\approx8.58\times10^{506}$ permutations, so probably yes.
### Changes
- Nyberg SBoxes now allow toggling between using polynomial or matrix at both affine transformation steps
  - Specifically, when using polynomial mode, the 0-indexed k-th lowest bit of the input is treated as the coefficient for $x^k$, and when using matrix mode, the 1-indexed k-th lowest bit of the input is treated as the k-th entry of a column vector in $(\text{GF}(2))^8$.
- Added visualizations for strict avalanche criterion and bit independence criterion (very arbitrarily drawn)
- Added more functions available to generating a custom SBox
- Removed $\text{GF}(257)$ as an option since it's bad and confusing
- Improved summary description
- Optimized some computation steps
- Probably added more bugs than I removed

And, last but not least, thank you to those that still somehow found this tool useful and even pointed out a typo I made.

## Things I learned while making this
- ~Affine transformation multiplier has to be coprime with `0xFF` in $\text{GF}(2^8)$ field.~ (see above)
  - ~This simply means it has to have an odd number of set bits.~
- Multiplicative inverse in $\text{GF}(257)$ isn't nonlinear enough.
- Completely randomly generated SBoxes are almost always bad. You're more likely to get hit by an asteroid than to generate an APN S-box from random shuffling.
- Lots of papers are useless, full of mistakes, or even just completely wrong.
  - Unfortunately, no one has the time to double-check all of them. You just have to beware when reading papers by undistinguished authors.