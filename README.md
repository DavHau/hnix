# hnix

Haskell parser, evaluator and type checker for the Nix language.

<!-- TOC generates automatically, do not bother editing any further TOC text -->
<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
## Contents

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

---

| [![Chat](https://badges.gitter.im/Join%20Chat.svg)](https://gitter.im/haskell-nix/Lobby)                                                                                  | CI                                                                                                                                                                                                                                         |
| :---                                                                                                                                                                     |  :---                                                                                                                                                                                                                                      |
| [![Hackage](https://img.shields.io/hackage/v/hnix?color=%235e5086&label=Latest%20release)](https://hackage.haskell.org/package/hnix)                                      | [![Hackage, Cabal, Linux](https://github.com/haskell-nix/hnix/workflows/Hackage,%20Cabal,%20Linux/badge.svg)](https://github.com/haskell-nix/hnix/actions?query=workflow%3A"Hackage%2C+Cabal%2C+Linux"+branch%3Amaster)                    |
| [![Hackage Matrix Builder](https://img.shields.io/badge/Hackage%20Matrix-Builder-%235e5086)](https://matrix.hackage.haskell.org/package/hnix)                             | [![Nixpkgs, Linux, main](https://github.com/haskell-nix/hnix/workflows/Nixpkgs,%20Linux,%20main/badge.svg)](https://github.com/haskell-nix/hnix/actions?query=workflow%3A%22Nixpkgs%2C+Linux%2C+main%22+branch%3Amaster)                   |
| [![Nixpkgs Hydra CI](https://img.shields.io/badge/Nixpkgs%20Hydra-CI-%234f72bb)](https://hydra.nixos.org/job/nixpkgs/trunk/haskellPackages.hnix.x86_64-linux#tabs-status) | [![Nixpkgs, Linux, additional](https://github.com/haskell-nix/hnix/workflows/Nixpkgs,%20Linux,%20additional/badge.svg)](https://github.com/haskell-nix/hnix/actions?query=workflow%3A%22Nixpkgs%2C+Linux%2C+additional%22+branch%3Amaster) |
| [![Release dependencies](https://img.shields.io/hackage-deps/v/hnix?label=Release%20dependencies)](https://packdeps.haskellers.com/feed?needle=hnix)                      | [![Nixpkgs, macOS](https://github.com/haskell-nix/hnix/workflows/Nixpkgs,%20macOS/badge.svg)](https://github.com/haskell-nix/hnix/actions?query=workflow%3A%22Nixpkgs%2C+macOS%22+branch%3Amaster)                                         |
| [![Repology page](https://img.shields.io/badge/Repology-page-%23005500)](https://repology.org/project/haskell:hnix/versions)                                              | |

---

## Prerequisites
Until `hnix-store` is ready, `nix-store` is still used for interacting with the store paths, so Nix is still required installed and available through `$PATH`.

## Getting Started

```
git clone --recursive https://github.com/haskell-nix/hnix.git
cd hnix
nix-shell
cabal v2-configure --enable-tests
cabal v2-build
```

Run testing:
  * Default:
    ```
    cabal v2-test
    ```

  * All:
    ```
    env ALL_TESTS=yes cabal v2-test
    ```

  * Selected (list of tests is in `tests/Main.hs`):
    ```
    env NIXPKGS_TESTS=yes PRETTY_TESTS=1 cabal v2-test
    ```

Run built binary with Cabal (`--` is for separation between `cabal` & `hnix` args):
```
cabal v2-run hnix -- --help
```

## Using the REPL

Enter REPL:
```
hnix --repl
```

To evaluate an expression and make it available in the REPL as the `input` variable use:
```
hnix --eval -E '(import <nixpkgs> {}).pkgs.hello' --repl
```

Use the `:help` command for a list of all available REPL commands.


## Cachix prebuild binary caches

To autoload prebuild project dependencies - please, enable the official HNix Cachix binary cache:

1. Go through https://cachix.org/ and set it up.

2. Run: `cachix use hnix`


## Building

### With benchmarks

To build `hnix` with benchmarks enabled:

```
nix-shell
cabal v2-configure --enable-tests --enable-benchmarks
cabal v2-build
cabal v2-bench
```

### With profiling

To build `hnix` with profiling enabled:

```
nix-shell
cabal v2-configure --enable-tests --enable-profiling --flags=profiling
cabal v2-build
cabal v2-run hnix -- <args> +RTS -p
```

### With full debug info

To build `hnix` for debugging, with full tracing output and stack traces:

```
nix-shell
cabal v2-configure --enable-tests --enable-profiling --flags=profiling --flags=tracing
cabal v2-build
cabal v2-run hnix -- -v5 --trace <args> +RTS -xc
```

Note that this will run quite slowly, but will give the most information as to what might potentially be going wrong during parsing or evaluation.


## Contributing

1. If something in the [quests](https://github.com/haskell-nix/hnix/issues?q=is%3Aissue+is%3Aopen+label%3A%22help+wanted%22+no%3Aassignee) looks interesting, look through the thread and leave a comment taking it, to let others know you're working on it.

2. You are free to chat with everyone on [Gitter](https://gitter.im/haskell-nix/Lobby).

3. When pull request is ready to be submitted, to save time - please, test it with:

```
git submodule update --init --recursive
nix-shell --run "LANGUAGE_TESTS=yes cabal v2-test"
```

Please, check that all tests that were passing prior (most probably all tests mentioned in the command) are still passing for the PR, it is faster to chech that locally then through CI. It's OK if no new tests are passing.

### Evaluating Nixpkgs with HNix

Currently the main high-level goal is to be able to evaluate all of Nixpkgs. To run this yourself, first build `hnix` with `nix-build`, then run the following command:

```
./result/bin/hnix --eval -E "import <nixpkgs> {}" --find
```
