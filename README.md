# MPHF - *Coordinate System*
> **<sub>Part of the Kireji Project</sub>**<br><sup><i>omnia ex una linea</i></sup>

**MPHF** (Minimal Perfect Hash Function) provides the bijective coordinate system at the heart of the Kireji Project.
It guarantees that every valid state of an app or component corresponds to a unique integer coordinate, with **no collisions, duplicates, or gaps**.
The Kireji Web Framework uses these coordinates to encode application state directly in the URL, making navigation, persistence, and cross-origin traversal deterministic and lossless.
## The Kireji Project
The Kireji Project poses a question: **What if we could treat every web page as a point in a unified, mathematically mapped space?**

| Repo | Purpose
| ---- | -------
| **MPHF** | **Coordinate System - ★ You are here<br><sup>A bijective coordinate system for hashing structured data</sup>**
| [Kireji](https://github.com/kireji-app/kireji#readme) | [Web Framework<br><sup>A reactive web framework with MPHF routing</sup>](https://github.com/kireji-app/kireji#readme)
| [Demo](https://github.com/kireji-app/demo#readme) | [App Ecosystem<br><sup>An example app ecosystem demonstrating the project](https://github.com/kireji-app/demo#readme)</sup>
## Implementation
The set of all application states **S** is structured as a **combinatorial configuration space**.
This allows us to locate every configuration as a **point** on a **discrete manifold**.
A *minimal perfect hash function* is used to define a bijection between $S$ and the integer range $[0, |S|)$.

Each domain is hashed using positional offsets defined by component subdomains with all elements indexed from $0$. For example, a point in the cartesian product space of sets `A` and `B` is assigned a hash using $\text{hash}(A=a, B=b) = a \times |B| + b$.

This generalizes recursively to arbitrary combinations of nested spaces, each component yielding a minimal perfect hash function over the set of valid configurations of its state space.
### Types
Configuration spaces are assembled from components that come in three main types.

The **Part** type models a single state with no mutability.
```js
const apple = new Part("apple")

console.log(apple.cardinality)
// 1n
```
The **Match** type models a choice between two or more options.
```js
const fruit = new Match("fruit", [
 apple,
 "orange",
 "pear"
])

console.log(fruit.cardinality)
// 3n
console.log(fruit.hash("pear"))
// "2"
console.log(fruit.unhash("2"))
// "pear"
```
The **Mix** type models a set of independently mutable variables.
```js
const snack = new Mix("snack", [
 fruit,
 new Match("drink", [
  "water",
  "milk",
  "cola"
 ])
])

console.log(snack.cardinality)
// 9n
console.log(snack.hash({ fruit: "apple", drink: "water" }))
// 6
console.log(snack.unhash("6"))
// { fruit: "apple", drink: "water" }
```
### URL Encoding
MPHF also includes methods for encoding large integers as url-safe base64 strings:
```js
console.log(Part.encode(12345678901234567890n))
// aJkGoPH7MHi

console.log(Part.decode('hello-world'))
// 19857872207319512397n
```
### Tech Stack
This library is written using **Vanilla JavaScript** with no third-party dependencies.
### Status and License
The MPHF Coordinate System is in **Alpha**.

The Kireji Project is in **early research and development**.

[![MPHF on npm](https://img.shields.io/npm/v/mphf?style=for-the-badge&labelColor=CB3837&logo=npm&logoColor=white&label=NPM+package&color=212121)](https://www.npmjs.com/mphf)
<br>[![Project Status: Alpha](https://img.shields.io/badge/status-alpha-212121?style=for-the-badge&labelColor=181717&logo=github&logoColor=white)](https://www.repostatus.org/#alpha)
<br>[![Commits](https://img.shields.io/github/commit-activity/t/kireji-app/mphf?style=for-the-badge&labelColor=181717&color=212121&logo=github&logoColor=white)](https://github.com/kireji-app/demo/commits/)
<br>[![GitHub Last Commit](https://img.shields.io/github/last-commit/kireji-app/mphf?style=for-the-badge&labelColor=181717&color=212121&logo=github&logoColor=white)](https://github.com/kireji-app/demo)
<br>[![Copyright © 2023-2025 <a href="https://www.ejaugust.com">Eric Augustinowicz</a>](https://img.shields.io/badge/2023%20--%202025-Eric_Augustinowicz-212121?labelColor=007ec6&style=for-the-badge&logoColor=white&logo=data:image/svg+xml;base64,PHN2ZyB3aWR0aD0iMTk3cHgiIGhlaWdodD0iMTk3cHgiIHhtbG5zPSJodHRwOi8vd3d3LnczLm9yZy8yMDAwL3N2ZyIgdmVyc2lvbj0iMS4xIj4KIDxkZWZzPgogIDxtYXNrIGlkPSJtYXNrIj4KICAgPGNpcmNsZSBjeD0iOTgiIGN5PSI5OCIgcj0iOTgiIGZpbGw9IndoaXRlIiAvPgogICA8Y2lyY2xlIGN4PSI5OCIgY3k9Ijk4IiByPSI3OCIgZmlsbD0iYmxhY2siIC8+CiAgIDxjaXJjbGUgY3g9Ijk4IiBjeT0iOTgiIHI9IjU1IiBmaWxsPSJ3aGl0ZSIgLz4KICAgPGNpcmNsZSBjeD0iOTgiIGN5PSI5OCIgcj0iMzAiIGZpbGw9ImJsYWNrIiAvPgogICA8cmVjdCB4PSIxMTUiIHk9Ijg1IiB3aWR0aD0iNDUiIGhlaWdodD0iMjUiIGZpbGw9ImJsYWNrIiAvPgogIDwvbWFzaz4KIDwvZGVmcz4KIDxwYXRoIGQ9Ik0gOTgsMCBBIDk4LDk4IDAgMSAxIDk4LDE5NiBBIDk4LDk4IDAgMSAxIDk4LDAgWiIgZmlsbD0id2hpdGUiIG1hc2s9InVybCgjbWFzaykiIC8+Cjwvc3ZnPg==)](http://www.ejaugust.com/)
<br>[![Released under MIT License](https://img.shields.io/badge/License-MIT-212121?labelColor=007ec6&style=for-the-badge&logo=data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIxNiIgaGVpZ2h0PSIxNiIgdmlld0JveD0iMCAwIDEyIDEyIj48cGF0aCBzdHlsZT0iZmlsbDpub25lO3N0cm9rZTp3aGl0ZTtzdHJva2Utd2lkdGg6Ljk5OTc1MDAyO3N0cm9rZS1saW5lY2FwOmJ1dHQ7c3Ryb2tlLWxpbmVqb2luOm1pdGVyO3N0cm9rZS1taXRlcmxpbWl0OjYuNDAwMDAwMTtzdHJva2UtZGFzaGFycmF5Om5vbmU7c3Ryb2tlLW9wYWNpdHk6MSIgZD0iTTQuMjUgOS41SDZNNC4yNSA3Ljc1SDZtMi4wMjMtNS41YzAgLjY5LS45MDQgMS4yNS0yLjAyMyAxLjI1LTEuMTE5IDAtMi4wMjItLjU1OS0yLjAyMi0xLjI1QzMuOTc4IDEuNTU4IDQuODggMSA2IDFjMS4xMTkgMCAyLjAyMy41NTggMi4wMjMgMS4yNXpNNiAxMVYzLjUiLz48L3N2Zz4=&logoColor=white)](https://github.com/kireji-app/mphf/LICENSE.md)
<br>[![Sponsor this Project](https://img.shields.io/badge/Sponsor-212121?labelColor=red&style=for-the-badge&logo=data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSI0OTciIGhlaWdodD0iNDcwIj48cGF0aCBkPSJNMTQwIDIwQzczIDIwIDIwIDc0IDIwIDE0MGMwIDEzNSAxMzYgMTcwIDIyOCAzMDMgODgtMTMyIDIyOS0xNzMgMjI5LTMwMyAwLTY2LTU0LTEyMC0xMjAtMTIwLTQ4IDAtOTAgMjgtMTA5IDY5LTE5LTQxLTYwLTY5LTEwOC02OXoiIHN0cm9rZT0id2hpdGUiIHN0cm9rZS13aWR0aD0iNDAiIGZpbGw9Im5vbmUiLz48L3N2Zz4=&logoColor=white)](https://github.com/sponsors/EJAugust)