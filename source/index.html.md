---
title: Axon language specification

toc_footers:
  - <a href='https://github.com/lord/slate'>Documentation Powered by Slate</a>

includes:
  - tooling
  - types
  - conditionals
  - loops
  - destructuring
  - pipelines
  - data_manipulation
  - itertools

search: true
---

# Guiding Principles

The objective is a simple, concise, and powerful language that allows experienced devs to quickly build reliable data engineering pipelines

### Designed for enterprise

- File style is normalized upon compilation (style wars are impossible)
- Style is designed to allow most readable possible git diffs
- Simple syntax, so most devs know the entire syntax (contrast with SQL)
- Powerful and easy logging capabilities
- Extreme portability through running on the JVM

### Designed for data engineering

- Concise native syntax for specifying data pipelines
- Powerful capabilities for manipulating hierarchical data
- Simple, complete, battle-tested modules for database connections and reading/writing JSON, YAML, CSV, XML, etc

### Designed for big data

- Everything is streams by default
- Support for concurrent execution due to Scala transpilation
