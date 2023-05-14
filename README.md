# Nix gripes

Complaints about the _technical_ design of Nix, not including documentation/shallow UX issues.

PRs welcome 😉

## The good parts

Nix is a build system that is a unique combination both _reproducible_ and _composable_. It (theoretically) unifies the concepts of build system, package management, and deployment.

## The bad parts


- Nix uses a programming language to configure builds. I want to configure my build files from a configuration file, not a programming language.

  Whilst developers just want to tell the build system what they want and have it get out of their way, Nix insists in combining the _what_ of configuration with the _how_. This both obscures the programmer's intent and leads to unnecesary verbosity. Compare a typical `flake.nix` with an NPM `package.json`.
  
  - Getting anything done in this language is immensely difficult. [Blame for this can be assigned to various places](https://www.haskellforall.com/2022/08/stop-calling-everything-nix.html), but the point remains that Nix as it stands has a severe usability problem

  - The build conventions for different languages etc. is inconsistent

- Despite having a custom programming language, performing functions basic to Nix use is not well supported, requiring strange hacks in the nixpgs and Flakes APIs/EDSLs, further obscuring understandability.

- The Nix implementation is extremely buggy. A lot of functionality simply doesn't work as documented for all but the most straightforward use cases.

- Evaluation of Nix is slow. This prevents it being used as a module-level build system.

- Integration with existing build systems is poor and inconsistent. This often seems to require IFD (I don't know if this is an intrinsic requirement, or simply the easiest/current best way in certain situations), which brings a whole host of problems - IFD hides dependencies, and actions unpredictably become slow, or can even [fail entirely](https://github.com/NixOS/nix/issues/4265).

- Conventional build system UX is that the user specifies the name/location of a dependency and possibly a version, and the build system works out the exact revision to use. Since Flakes, there is a conventional way to _store_ revision pins, but how to _instantiate_ these pins is still an unsolved problem (Seeing as this action is inherently unreproducible). In other words, in my opinion Flakes does not fully solve the problem it is intended to.

  Making sure dependency versions are in agreement is a known problem of dependency management (The 'diamond problem') and different build systems take different approaches - constraint solving, package sets, 'follows' - so the ideal solution would provide a _mechanism_ compatible with different _policies_.
