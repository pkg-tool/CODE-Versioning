\<CODE\> Versioning: Chronological Orderly Development Evolution
===

# Introduction

**CODE Versioning** (Chronological Orderly Development Evolution) is a system designed to address the shortcomings of traditional versioning systems by introducing a straightforward, easy-to-use approach.
It is particularly useful for developers who require a clear, unambiguous method to indicate significant changes in their software while maintaining a chronological order of releases.

# Version Structure

A **CODE** version number consists of three parts: `<breaking>.<counter>.<identifier>`, where:

1. **Breaking**: A non-negative integer that increments when backward-incompatible changes are introduced.
2. **Counter**: An ever-increasing counter, optionally reset to zero whenever the breaking part increments.
3. **Identifier**: A unique identifier, typically a commit hash or a tag in the version control system.

Example: `0.123.a7f3b2c`

## Breaking

* **Definition**: The breaking part of the version number is incremented whenever there are changes that break backward compatibility.
* **Reset Rule**: When breaking is incremented, the counter could be optionally resets to zero.

## Counter

* **Definition**: The `counter` part is an infinite, ever-increasing integer that ensures the version is sortable. It starts at 0 and increments with each new build.
* **Optional Use**: In cases where only the latest version is relevant, the counter can be omitted, resulting in a version format of `braking.<identifier>` (e.g., `1.a7f3b2c`).
* **Reset Rule**: The counter is reset to zero (optionally) when the breaking increments. This makes it easy to understand and track significant changes in the software.

## Identifier

* **Definition**: The identifier part is a string that uniquely identifies the build, usually a commit hash in a VCS (Version Control System). It can also be a tag or any unique string identifier.
* **Purpose**: The identifier allows you to quickly find the exact version in the repository by its hash. If you don't need to connect versions with the repository, the identifier can be omitted, resulting in a format like `braking.counter` (e.g., `0.123`).

_Notice_: **Both the Counter and the Identifier cannot be omitted simultaneously. At least one of these parameters must be kept to ensure version uniqueness and traceability.**

## Constraints

* Neither `braking` nor `counter` are allowed to decrease over time.
* Versions must always move forward numerically for `braking` and `counter`.
* The `counter` can be reset to **0** when the `braking` part is incremented.

# Detailed Counter Management

## Purpose of the Counter

The counter serves two primary purposes:

1. **Uniqueness**: It ensures each version is uniquely identifiable.
2. **Sortability**: It helps in sorting versions chronologically, making it easy to determine the order of releases.

## When to Increment the Counter

* Regular Build:
  * The `counter` increments by one with each new build that does not involve a breaking change. 
  * Example:
    * Initial release: `1.0.a1b2c3d`
    * Next build: `1.1.b4c5d6e` (counter incremented from 0 to 1)
* Pre-releases:
  * Pre-releases increment the counter just like regular releases to maintain a chronological order.

## When to Reset the Counter

* Breaking Changes:
  * The counter is reset to zero whenever there is a breaking change, which is indicated by an increment in the breaking part. 
  * Example:
    * Previous version: `1.847.a7f3b2c` 
    * Breaking change: `2.0.a7f3b2c` (counter reset to 0)
  * Alternatively, the counter can continue increasing even after a breaking change. This approach is suitable in cases where there are a relatively low number of builds.
    * Example:
      * Previous version: `1.847.a7f3b2c`
      * Breaking change with continued counter: `2.848.b8g4d3e` (counter continues from the previous build)

## Rules for Counter Management

1. Incrementing:
   * The `counter` must increment sequentially by one with each new build.
   * It must not skip numbers or be manually set to a higher value to avoid confusion.

2. Resetting:
   * The `counter` resets to zero (optionally) only when the `breaking` component is incremented.
   * This reset signifies that the new release contains backward-incompatible changes.

3. Pre-release Incrementing:
   * Pre-releases follow the same `counter` incrementing rule to maintain their chronological order.

# Counter Examples

Here are some examples to illustrate how the counter should be managed in various scenarios:

* Regular Incrementing:
```
    1.0.a1b2c3d
    1.1.b4c5d6e (counter incremented)
    1.2.c7d8e9f (counter incremented)
```

* Breaking Change and Reset:
```
    1.847.a7f3b2c
    2.0.b8g4d3e (breaking change, counter reset to 0)
    2.1.c9h5i6j (counter incremented from 0)
```

# Pre-releases and Release Candidates

## Pre-release Identifiers

* **Definition**: A pre-release version is an initial version that is not yet suitable for production use but is shared for testing or feedback.
* **Syntax**: Pre-release versions are denoted by appending a hyphen followed by a series of dot-separated identifiers to the version number.
* **Example**: 
  * `1.0.a7f3b2c-alpha`
  * `2.847.a7f3b2c-beta.1`
  * `1.c9h5i6j-alpha` (the `counter` was omitted)
  * `2.848-beta.1` (the `identifier` was omitted)

## Naming Conventions

### Common Identifiers:

* alpha: Early testing, possibly unstable.
* beta: Feature complete but may contain known issues.
* rc: Release candidate, potentially the final version unless significant bugs emerge.

### Incrementing Pre-releases

1. Sequential Increment:

   * If a new pre-release is issued after an existing one, increment the numerical suffix.
   * Example: 
     * After `1.0.a7f3b2c-alpha.1`, the next pre-release could be `1.1.b2ca7f3-alpha.2` (`counter` incremented).

2. Resetting:

   * When transitioning from one pre-release stage to another (e.g., from alpha to beta), reset the numerical suffix.
   * Example: 
     * After `1.0.a7f3b2c-alpha.2`, the next stage would be `1.1.c9h5i6j-beta.1` (`counter` incremented).

# Practical Examples

* Alpha Stage:
```
    1.0.a7f3b2c-alpha
    1.1.a7f3b2c-alpha.1
    1.2.a7f3b2c-alpha.2
```

* Beta Stage:
```
    1.3.a7f3b2c-beta
    1.4.a7f3b2c-beta.1
    1.5.a7f3b2c-beta.2
```

* Release Candidate Stage:
```
    1.6.a7f3b2c-rc
    1.7.a7f3b2c-rc.1
    1.8.a7f3b2c-rc.2
```

# Edge Cases and Considerations

1. Multiple Releases in a short period of time:

   * If multiple versions are released in a short period of time, increment the counter to ensure unique identification.
   * Example:
     * first release: `2.847.a7f3b2c`
     * second release: `2.848.b8g4d3e`

2. Handling Large Number of Builds:
   * For projects with a significant number of builds, consider using a larger numerical range (e.g., HEX or timestamp-based counters) to avoid exhausting the counter quickly.
   * Example using HEX: 
     * `0.x29A.a7f3b2c`

# FAQ

Q: What if I need to release multiple versions on the same day?\
A: Increase the counter number or use a more granular timestamp to ensure unique ordering.

Q: How do I handle pre-releases or release candidates?\
A: Append additional information after the identifier, e.g., 2.847.f9a2d1e-rc.1

Q: Why use CODE Versioning?\
A: CODE Versioning provides a straightforward way to communicate compatibility changes, maintain build order, and identify specific releases, while being flexible enough to adapt to various development workflows.

# About

CODE Versioning specification is open for community feedback and improvement. Developers are encouraged to adopt and adapt this system to best fit their project needs while maintaining the core principles of clarity and ordered evolution.

# License
This work is licensed under the Creative Commons Attribution 4.0 International License (CC BY 4.0).
This license allows anyone to share, copy, and redistribute the material in any medium or format, as well as adapt, remix, transform, and build upon the material for any purpose, even commercially, as long as appropriate credit is given, a link to the license is provided, and any changes made are indicated.

