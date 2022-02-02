# MvnDependencyResolving
Example of Maven Dependency Resolving

# Configuration
1. Install META-POM, a POM which contains configuration of maven plugins and defines version of dependencies in the mangament section.
2. The project contains an EJB module and a EARmodule with a depency of the EJB.

# Background
We had some issues with depency resolving, when unexpierienced team mates overwrote version in the wrong place (e.g. in EJB module) and then wondered why the final EAR archive containts the "wrong" version.

This project should be a show case for wrong and hopefully ways to enforce depenencies.
