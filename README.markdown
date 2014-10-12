# cabal2nix4dev

A wrapper round `cabal2nix` for use when developing Haskell packages.

It writes a `default.nix` and `shell.nix` to the current directory,
based on the `*.cabal` found in the current directory.  The idea is
that you shouldn't have to manually update your `*.nix` files when you
update your `*.cabal` file.


## Usage

 *  Test using the default version of ghc

    `cabal2nix4dev && nix-shell --command 'cabal test'`

 *  Use a specific version of ghc

    `cabal2nix4dev && nix-shell --command 'cabal test' --arg haskellPackages '(import <nixpkgs> {}).haskellPackages_ghc763'`

 *  Use profiling libraries

    `cabal2nix4dev && nix-shell --command 'cabal test' --arg haskellPackages '(import <nixpkgs> {}).haskellPackages_ghc783_profiling'`

 *  Install into local Nix store

    `cabal2nix4dev && nix-build`


## Limitations

 *  `cabal bench` doesn't work.
    (Benchmark-specific packages aren't mentioned in `default.nix`,
    so don't get installed.)

 *  Only works when you are developing a single package.

    If you are developing two packages (call them `foo` and `bar`), and `foo`
    depends on `bar`, and you want to ensure your new versions of `foo` and
    `bar` work with each other, then you will want the environment in which you
    are developing `foo` to include your local version of `bar`, not the latest
    version of `bar` from Hackage.  You will need to edit the files generated
    by `cabal2nix4dev` in this case.

 *  I'm a Nix newbie and don't totally understand how Nix packages/expressions
    work.  Please be gentle if I've made any silly mistakes :-)


## Installation

    $ git clone https://github.com/dave4420/cabal2nix4dev.git
    $ ln -s "$PWD/cabal2nix4dev/cabal2nix4dev" /somewhere/on/your/PATH/


## Implementation

A bash script, calling out to

 *  grep
 *  sed
 *  cut
 *  perl
 *  cabal2nix

Ideally I would rewrite it in Haskell as a patch to cabal2nix.


## Acknowledgements

 *  `shell.nix` and modifications to `default.nix` based on [instructions from
    Oliver Charles][ack ocharles].


[ack ocharles]: http://wiki.ocharles.org.uk/Nix#how-do-i-use-cabal2nix-for-local-projects
