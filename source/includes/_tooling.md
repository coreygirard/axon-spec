# Tooling

Axon's philosophy on style is "figure out what the user means, make sure it's legible, then do it". What this means is that we will never throw errors when the user intent is unambiguous. We'll simply normalize the style, and execute it as intended. The goal is for there to be a canonical source file that corresponds to a given AST (comments excepted). There are many many ways to write source files that compile to a given AST, but all of these files should, during the compilation step, be automatically converted to be identical. Again, with the exception of comments.
