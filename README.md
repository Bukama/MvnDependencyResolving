# MvnDependencyResolving
Example of Maven Dependency Resolving

# Background
We are defining the versions of dependencies in our POM/BOM via version-properties.
We do this for two reasons: 1) Using the same version for related dependencies, e.g. log4j (api and core) and 2) to have all versions at the start of the pom, so they are all insight and fast to update. 

When upgrading our projects due the Log4J problems in the end 2021, we had some issues with dependency resolving, when inexperienced teammates overwrote version in the wrong place (here in EJB module) and then wondered why the final EAR archive contains the "wrong" version. The following outputs of "mvn compile dependency:tree" with a wrong (in EJB module) and correct (in parent) overwrite are shown below:

Note: the "compile" is needed as the dependency plugin does not read modules from the reactor (see: https://www.mail-archive.com/users@maven.apache.org/msg104341.html)

```
Output of dependency:tree when overwriting the log4j-version using a property in the EJB-Module
NOTE that the EAR still contains the 2.17.0 version from the BOM! Unexpected, but correct in terms of maven.

my.example:MyProject:pom:1.0.0
my.example:MyEJB:ejb:1.0.0
+- org.apache.logging.log4j:log4j-api:jar:2.17.1:compile
\- org.apache.logging.log4j:log4j-core:jar:2.17.1:compile
my.example:MyEAR:ear:1.0.0
\- my.example:MyEJB:ejb:1.0.0:compile
   +- org.apache.logging.log4j:log4j-api:jar:2.17.0:compile
   \- org.apache.logging.log4j:log4j-core:jar:2.17.0:compile


Output of dependency:tree when overwriting the log4j-version using a property in the parent
NOTE that both moduls get the same version, defined in the parent.

mvn compile dependency:tree -DoutputFile=D:\Github\MvnDependencyResolving\MyProject\deptree.txt -DappendOutput=truemy.example:MyProject:pom:1.0.0
my.example:MyEJB:ejb:1.0.0
+- org.apache.logging.log4j:log4j-api:jar:2.17.1:compile
\- org.apache.logging.log4j:log4j-core:jar:2.17.1:compile
my.example:MyEAR:ear:1.0.0
\- my.example:MyEJB:ejb:1.0.0:compile
   +- org.apache.logging.log4j:log4j-api:jar:2.17.1:compile
   \- org.apache.logging.log4j:log4j-core:jar:2.17.1:compile

```
# Trying to enforce dependency convergence 
When trying to enforce dependencyConvergence via the maven-enforcer-plugin", we realised that the plugin "does not work". I put this in quotes, because it turns out it does not, when using properties (MyProjectWithProps), but it detects conflicts when set the versions directly  (MyProjectWithVersions).

In this repository I want to show exactly this.

# Configuration
1. META-POM: A POM which contains configuration of maven plugins, e.g. compiler and enforcer plugin
2. MYBOM A POM which contains dependency definitions, defined with properties
3. MyProjectWithProps: A multi module project where the EJB module overwrites the version property, but a) the enforcer plugin does not detect it, event that the dependency tree shows the different versions and b) the final EAR contains the version from the parent - this is correct, when looking at how Maven resolves dependencies!
4. MyProjectWithVersions:  multi module project where the EJB module overwrites the version in the version attribute. In this setup the enforcer plugin detects the conflict and fails the build

