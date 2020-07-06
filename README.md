# SBox-Analyzer
This tool performs basic linear and differential analysis on any 8-bit substitution box.
This tool is only made for educational purposes and is not meant to be used in serious cryptographic applications.
Feedback and pull requests are welcome. Sorry you had to go through my spaghetti code.

## Things I learned while making this
- Affine transformation multiplier has to be coprime with `0xFF` in GF(2<sup>8</sup>) field.
  - This simply means it has to have an odd number of set bits.
- Multiplicative inverse in GF(257) isn't nonlinear enough.
- Completely randomly generated SBoxes are always bad.
- CSS Flexbox is amazing.

## Things I hope to learn in the future
- Apparently there's another method of making perfect nonlinear SBoxes? I didn't really understand that part (or, most parts) of Nyberg's paper (Maiorana-McFarland method).
  - SBoxes generated this way is unique from all affine transformations of Galois Field inverse SBoxes, so it would be interesting to analyze those.
  - Not sure if it can be modified from Z<sup>n</sup> -> Z<sup>(n/2)</sup> to Z<sup>n</sup> -> Z<sup>n</sup> without loss of good nonlinear properties.
