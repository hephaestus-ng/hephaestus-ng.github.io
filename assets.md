---

layout: page
title: Assets
permalink: /assets/

---


#### [hephaestus-assets](https://github.com/hephaestus-ng/hephaestus-assets)

This module represents our resource abstractions that we'll be able to manipulate through Hephaestus. It can be any abstraction that you wish to work with as a product line, and have access to our SPL semantics and functionalities described in our code base.

A new asset is a data type, usually a record, that describes its properties and characteristics, in which we manipulate through our transformations. A transformation is a set of functions used to alter our final builded Product, and their usage is described in our Configuration Knowledge DSL file.

In our implementation, an Asset is a Typeclass in which we describe how the functions must be implemented by our signatures, and fully work with Hephaestus. Our typeclass is defined as follows:

                    `class Asset a where
        initialize :: Product a
        parserT    :: Parsec String () (Transformation a)
        export     :: Source -> Target -> Product a -> IO ()`


Our other main SPL types are defined in [here](https://github.com/hephaestus-ng/hephaestus-spl/blob/master/src/Data/SPL.hs)

Any asset instance **must** implement those three methods.

*   **initialize** \- used to define a base Asset instantiation, such as initializing all it's properties with empty values
*   **parserT** \- refers to the parser that will recognize the transformations described in our Configuration Knowledge
*   **export** \- logic to export a final builded product



### Creating a new Asset

We have to follow some architectural designs to create a new asset. Our structure to implement one takes this form:

[Data/](https://github.com/hephaestus-ng/hephaestus-assets/tree/master/src/Data)

[Assets.hs](https://github.com/hephaestus-ng/hephaestus-assets/blob/master/src/Data/Assets.hs)

SourceCode/

CustomAsset/

[HelloWorld/](https://github.com/hephaestus-ng/hephaestus-assets/tree/master/src/Data/HelloWorld)_example_ [Data/](https://github.com/hephaestus-ng/hephaestus-assets/tree/master/src/Data/HelloWorld/Data)

[Asset.hs](https://github.com/hephaestus-ng/hephaestus-assets/blob/master/src/Data/HelloWorld/Data/Asset.hs)

[ParserT.hs](https://github.com/hephaestus-ng/hephaestus-assets/blob/master/src/Data/HelloWorld/Data/ParserT.hs)

[Types.hs](https://github.com/hephaestus-ng/hephaestus-assets/blob/master/src/Data/HelloWorld/Data/Types.hs)

[Main.hs](https://github.com/hephaestus-ng/hephaestus-assets/blob/master/src/Data/HelloWorld/Main.hs)

README.md

CKExample.ck

The **Data.Assets** module is where we export our assets implementations to be used elsewhere.

A directory holds each individual asset module, which also follows a structre. The standardization of this repository allows Hephaestus to provide all of its functionalities to every asset, independent of how they are implemented.

We have a simple example asset, HelloWorld, which shows clearly what has to be defined.

The purpose of each module below:

*   **Data.Types**  

    The

*   **Data.ParserT**  

    This is the parser implementation. In Hephaestus we use the [Parsec](http://hackage.haskell.org/package/parsec) library to write our parsers.

*   **Data.Asset** \- logic to export a final builded product
