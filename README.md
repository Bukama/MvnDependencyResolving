# MvnDependencyResolving
Example of Maven Dependency Resolving

# Configuration
1. Install META-POM, a POM which contains configuration of maven plugins and defines version of dependencies in the mangament section.
2. The project contains an EJB module and a EARmodule with a depency of the EJB.

# Background
We had some issues with depency resolving, when unexpierienced team mates overwrote version in the wrong place (e.g. in EJB module) and then wondered why the final EAR archive containts the "wrong" version.

This project should be a show case for wrong and hopefully ways to enforce depenencies.


# Branches

* main: Default META-POM and multi module project, where the "wanted" version of a depency (here Log4J2) is "correctly" derived from the META-POM 
* overwrite: Example to show overwriting the version defined in the META-POM in the parent of the project, so the overwritten version is correctly placed in the EAR
* overwriteinejb: Showcase of the case, when the version of a depency in the META-POM is overwritten in a module, is not picked up in the final EAR, so the EAR containts the "not wanted" 2.17.0 from the META-POM, but not the "wanted" version 2.17.1 definied/overwritten in the EJB module
* enforceversion: TRY to ensure that the build detects that there are different versions used by using the "maven-enforcer-plugin". BUT this does not work, even that the docs write it will "Dependency Convergence" ( https://maven.apache.org/enforcer/enforcer-rules/dependencyConvergence.html )
