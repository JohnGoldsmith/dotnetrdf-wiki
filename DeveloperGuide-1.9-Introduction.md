[[Home]] > [[Developer Guide|DeveloperGuide]] > 1.9 Alpha - Introduction

# 1.9 Alpha Documentation

**Warning** You are viewing documentation for the 1.9 Alpha series of dotNetRDF releases.  The features and APIs described within these documents are currently undergoing active development and should be considered unstable and subject to change without notice.  If you are looking for the stable release documentation please see visit the [Documentation Overview](../../Home)

----

# Introducing dotNetRDF 1.9 Alpha

The dotNetRDF 1.9 Alpha release series is intended as an early developer preview of a major improvement and rewrite of dotNetRDF and are intended to eventually become the stable 2.x line of releases.

These releases focus on a number of themes:

* Modularizing the libraries
* Simplifying the API
* Addressing past design decisions
* Rewriting key features

## Modularizing the Libraries

One of the key aims of these releases is to move away from the single monolithic library approach that we have used in the past and instead break the APIs out into as many libraries as make sense.  The libraries of course build upon each other and so using a library higher up the feature stack will necessitate depending on relevant libraries lower down the stack.

## Simplifying the API

Another key aim is to simplify the API wherever possible in order to make it easier for developers to write code against dotNetRDF.

## Addressing past design decisions

Doing a major new version gives us the opportunity to make significant breaking changes to existing APIs where we feel this is necessary to address design decisions made years ago that have since proven to be a limiting factor in implementing new features and improving performance.

## Rewriting Key Features

On that theme as part of a new major version it also gives us the scope to rewrite some areas of functionality where the existing implementations are known to be limited or prevent us from making the types of improvement we wish to make going forward.

# Migrating to 1.9

Please see the [[Libraries|DeveloperGuide-1.9-Libraries]] page for details on what libraries (and thus features) are currently available.

Please see the [[Migrating to 1.9|DeveloperGuide-1.9-Migrating]] documentation for notes about some of the more developer visible changes and how to update your code to address them.