# About

LSS (Lexical Style Sheets) is a library to help writing CSS. It has
two main goals / opinions:

1. CSS should be scoped statically, not globally. Styles should only
   apply within the lexical context that they are declared to
   apply. This is VERY different from how CSS normally works, where
   often styles apply to a given element, regardless of where it is,
   and in less extreme cases, if a class-name is re-used, it can pick
   up styles from different parts of a site (provided they share the
   CSS files, of course).

2. Styling should be written in semantic, re-usable blocks. What this
   means is that rather than talking about the styling of a type of
   paragraph (say, `p.article`), and the heading style that goes with
   it (for example, `h3.article`), and the way that links should
   appear (`a.article`), you should rather put all the styles for an
   `article` together within a named context.

   In plain CSS, this would be accomplished with some container (often
   a `<div>`) with class `article`, and then styles of form `.article
   p`, `.article h3`, and `.article a`. In one of the various CSS
   preprocessors that supports nesting, it would be more concisely
   represented by something like:

   ```scss
   .article {
       p { ... }
       h3 { ... }
       a { ... }
   }
   ```

   LSS both restricts and expands this. Semantic blocks are not CSS
   selectors, and arbitrary nesting is not permitted. Our experience
   has been that heavy nesting actually makes organizing and re-using
   portions of CSS more difficult. On the other hand, semantic blocks
   can have parameters, to allow them to be more flexible / general.

3. This isn't really a goal or opinion, but something that it seems
   bizare that CSS does not support: LSS has constants. They can be
   either global or inside semantic blocks, and are defined with `=`,
   with semicolons at the end of line, like:

   ```
   acme-gray = #efefef;
   ```

   These can then be used anywhere a CSS identifier is legal, which is
   most places where you would want them.

# Examples

```scss
store(highlight-color) {
  .featured {
    width: 100%;
    height: 200px;
    margin: 10px;
    border-bottom: 10px solid transparent;
  }
  .featured:hover {
    border-bottom: 10px solid highlight-color;
    cursor: pointer;
  }

  .item {
    width: 30%;
    height: 200px;
    display: inline-block;
    vertical-align: top;
  }
}
```

# Building

This depends on the (unreleased as of now) library that has attoparsec
parsers for the `language-css` library. I have a fork of that library at:
https://github.com/dbp/language-css-attoparsec.

You should install that locally, or `cabal sandbox add-source` a
checkout of it.

# Using

If you are using the Snap Web Framework, you can use the provided
`snaplet-lss` package. It's important to modify the development reloader
in the generated `Main.hs` file in your project so that it will reload
when `lss` files change. The line in question looks like:

    ["snaplets/heist/templates"]

And should be changed to:

    ["snaplets/heist/templates", "snaplets/lss"]


# Tests

There are test suites for both the core language (in package/directory
`lss`) and the snap adaptor (in package/directory `snaplet-lss`).

For each, change into the corresponding directory and:

First install dependencies:

    cabal install --only-dependencies --enable-tests

And then build/run the test suite with:

    cabal test --show-details=streaming

If you are only changing tests, you can re-run the tests faster (and
with color output) by just running the test main (which is faster
because it won't check and rebuild the library):

    cabal exec runghc -- test/Main.hs

# License
BSD3

# Authors
Daniel Patterson (dbp@dbpmail.net)
