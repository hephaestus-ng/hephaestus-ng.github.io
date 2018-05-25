---

layout: page
title: Assets
permalink: /assets/

---


#### [hephaestus-assets](https://github.com/hephaestus-ng/hephaestus-assets)

This module represents our resource abstractions that we'll be able to manipulate through Hephaestus. It can be any abstraction that you wish to work with as a product line, and have access to our SPL semantics and functionalities described in our code base.

A new asset is a data type, usually a record, that describes its properties and characteristics, in which we manipulate through our transformations. A transformation is a set of functions used to alter our final builded Product, and their usage is described in our Configuration Knowledge DSL file.

In our implementation, an Asset is a Typeclass in which we describe how the functions must be implemented by our signatures, and fully work with Hephaestus. Our typeclass is defined as follows:

```haskell
      class Asset a where
        initialize :: Product a
        parserT    :: Parsec String () (Transformation a)
        export     :: Source -> Target -> Product a -> IO ()
```


Our other main SPL types are defined in [here](https://github.com/hephaestus-ng/hephaestus-spl/blob/master/src/Data/SPL.hs)

Any asset instance must implement those three behaviours.

*   **initialize** \- used to define a base Asset instantiation, such as initializing all it's properties with empty values
*   **parserT** \- refers to the parser that will recognize the transformations described in our Configuration Knowledge
*   **export** \- logic to export a final builded product



### Creating a new Asset

We have to follow some architectural designs to create a new asset. Our structure to implement one takes this form:

```
Data/
   Assets.hs

   HelloWorld/                    <- example
      Main.hs
      Data/
        Asset.hs
        ParserT.hs
        Types.hs
      Readme.md
      CKExample.ck

   SourceCode/
    ...

   Simulink/
    ...
```


The **Data.Assets** module is where we export our assets implementations to be used elsewhere.

A directory holds each individual asset module, which also follows a structre. The standardization of this repository allows Hephaestus to provide all of its functionalities to every asset, independent of how they are implemented.

We have a simple example asset, HelloWorld, which shows clearly what has to be defined, and we'll use it as reference to explain what each file
is supposed to do.

The purpose of each module is:

*   **Data.Types**  

    The module where we define our new data type that represents our asset, usually in a record format. In this same module, we specify the functions that will represent the transformations we can do with such asset.

*   **Data.ParserT**  

    This is the transformation parser implementation. In Hephaestus we use the [Parsec](http://hackage.haskell.org/package/parsec) library to write our parsers.

    The parser must read and interpret the transformation part of a Confuration Knowledge file. In the HelloWorld example, we have a setMessage transformation, and in a .ck file, it would take the form of

    ```
    ... => [setMessage("HelloWorld!"), setMessage("Overrding"), ...)]
    ```

    Our ParserT module is responsible for parsing **only** the function part, the *setMessage("HelloWorld!")*, and accordingly, return a transformation that corresponds to the function defined on **Data.Types**

*   **Data.Asset**

    In this module, we instantiate our asset defined on **Data.Types** to the Asset typeclass, and implement the three necessary functions for it.

    The initialize is trivial, only initializing an empty product.

    The parserT is direct, referencing the parser defined in **Data.ParserT**.

    The export function is the logic to export a builded product and do what you
    desire with it.

*   **Main.hs**    

    The main module where we export the implementation of our asset, and make it available to be imported by **Data.Assets**

It's important to pay attention to module names, and make them correspond to their folder structure in the repository.

Happy coding, and let's enrich Hephaestus environment with new assets. Cheers !
