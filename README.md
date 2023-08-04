# Foundry custom remappings priority issue reproduction

This repository reproduces an issue with Foundry (tested with `forge 0.2.0 (2d87c0c 2023-08-04T00:24:28.478338727Z)`).

This project defines a custom remapping to an npm package, and forge prioritizes the automatically generated one. This leads to a compilation error.


## How to reproduce it

1. Clone this repository
2. Run `npm i`
3. Run `forge build`, which will fail.

## Explanation of the behavior

This repository has a remapping from `@prb/math` to `node_modules/@prb/math/src/`.

`forge` automatically maps `@prb` to `node_modules/@prb`.

As `forge` prioritizes shorter remappings (as per [this comment](https://github.com/foundry-rs/foundry/blob/4ebac29412e5fbec8806e8cf6e762d08eea9bc8c/config/src/providers/remappings.rs#L48-L50)), so the automatically generated one takes presedence, mapping `@prb/math` to `node_modules/@prb/math`, instead of the desired `node_modules/@prb/math/src/`.


## Alternative behavior

`forge` could give lower priority to automatically generated remappers, prioritizing the user's.
