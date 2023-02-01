# TestTagBot

[![Stable](https://img.shields.io/badge/docs-stable-blue.svg)](https://okatsn.github.io/TestTagBot.jl/stable/)
[![Dev](https://img.shields.io/badge/docs-dev-blue.svg)](https://okatsn.github.io/TestTagBot.jl/dev/)
[![Build Status](https://github.com/okatsn/TestTagBot.jl/actions/workflows/CI.yml/badge.svg?branch=main)](https://github.com/okatsn/TestTagBot.jl/actions/workflows/CI.yml?query=branch%3Amain)
[![Coverage](https://codecov.io/gh/okatsn/TestTagBot.jl/branch/main/graph/badge.svg)](https://codecov.io/gh/okatsn/TestTagBot.jl)

!!! note
    This is a julia package created using `okatsn`'s preference, and this package is expected to be registered to [okatsn/OkRegistry](https://github.com/okatsn/OkRegistry) for CIs to work properly.

## Adding secrets
!!! warning
    - You have to add `ACCESS_OKREGISTRY` to the secret under the remote repo (e.g., https://github.com/okatsn/TestTagBot.jl).
    - `ACCESS_OKREGISTRY` allows `CI.yml` to automatically register/update this package to [okatsn/OkRegistry](https://github.com/okatsn/OkRegistry).

## Doc test
`pkg> add Documenter` to make doc tests worked.

## Tips for connecting to remote
Connect to remote:
1. Switch to the local directory of this project (TestTagBot)
2. Add an empty repo TestTagBot(.jl) on github (without anything!)
3. `git push origin main`
- It can be quite tricky, see https://discourse.julialang.org/t/upload-new-package-to-github/56783
More reading
Pkg's Artifact that manage an external dataset as a package
- https://pkgdocs.julialang.org/v1/artifacts/
- a provider for reposit data: https://github.com/sdobber/FA_data

## Hints for Documenter

In `docs/make.jl`, add pages for example:
```julia
pages=[
    "Home" => "index.md",
    "Examples" => "examples/examples.md",
    "Exported Functions" => "functions.md",
    "Models" =>
                ["Model 1" => "models/model1.md",
                 "Model 2" => "models/model2.md"],
    "Reference" => "reference.md",
    ]
```

In which,
- the `index.md` is the home page, where
    - [`@index` block](https://documenter.juliadocs.org/stable/man/syntax/#@index-block) makes a list of links to docstrings once generated by `@autodoc` in any of your `.md` files for pages.
    - [`@autodocs` block](https://documenter.juliadocs.org/stable/man/syntax/#@autodocs-block) automatically generates docstrings given
        1. the `Modules` (e.g., `Modules = [Foo, Bar, Bar.Baz]`)
        2. the `Pages` (e.g., `Pages   = ["a.jl", "b.jl"]`)
- [`@contents` block](https://documenter.juliadocs.org/stable/man/syntax/#@contents-block) generates table of contents.
    - Example
      ```@contents
      Pages = ["examples/examples.md"]
      Depth = 5
      ```

!!! note
    - If error occurred in generating the page, try the followings in your local machine:
    ```julia
    makedocs(root=joinpath(dirname(pathof(TestTagBot)), "..", "docs"), sitename="TEMP")
    ```
    - Use `dirname(pathof(TestTagBot))` to locate `TestTagBot/src`!

!!! warning for  `@autodocs`:
    Noted that you cannot generate duplicate doc strings with `@autodocs`, with later ones all ignored with warning messages. For example:
    In `index.md`, I have `@autodocs` block:
    ```@autodocs
    Modules = [SWCForecast]
    Order   = [:function, :type]
    ```
    , which generates docstrings for ALL instances under `SWCForecast`.
    After that, in `"models/model2.md"`, I have `@autodocs` block:
    ```@autodocs
    Modules = [SWCForecast]
    Order   = [:function, :type]
    using FileTools, SWCForecast
    Pages = filelist(r".+\.jl", joinpath(dirname(pathof(SWCForecast)),"mymodels"); join=false)
    # which might be `Pages = [model2.jl, model2a.jl]` in the directory "mymodels", for example
    ```
    , which is intended to generate docstrings for instances (functions, macros, structs...) defined in all `.jl` files in the directory "mymodels".
    However, since the docstrings for an instance cannot be `@autodocs` twice, the corresponding `@autodocs` block in the page `"Model 2" => "models/model2.md"` will be empty.

For more information, see [this](https://documenter.juliadocs.org/stable/man/guide/#Adding-Some-Docstrings)

This package is create on 2023-02-01.
