# Dependencies — SBMLReader2 (SF110 #34)

## Overview

SBMLReader2 is a Cytoscape plugin for reading SBML (Systems Biology Markup Language) files.
It depends on several legacy Java libraries, most of which are not available on Maven Central.
We placed these in a project-local Maven repository (`repo/`) so the build works on a fresh
checkout without any manual installation.

## Compile dependencies

**Cytoscape 2.x** (`cytoscape.jar`) —
This is the host application that the plugin runs inside. Cytoscape 2.x was never published
to Maven Central; only the newer 3.x line is there. We installed it to the local repo as
`org.cytoscape:cytoscape:2.x-local`.

**GINY** (`giny.jar`) —
A graph model library used internally by Cytoscape 2.x. Not on Maven Central.
Installed as `giny:giny:local`.

**JSBML 0.8** (`jsbml-0.8.jar`) —
A pure-Java library for reading and writing SBML documents. Version 0.8 is older than the
earliest release on Maven Central (1.0), so we could not use the Central version without
risking API incompatibilities. Installed as `org.sbml.jsbml:jsbml:0.8`.

**libSBML 3.3.2** (`libsbml-3.3.2.jar`) —
Java bindings for the native libSBML C library. Not available on Maven Central at all.
Installed as `org.sbml:libsbml:3.3.2`.

**JigCell SBML2** (`sbml2.jar`) —
Part of the JigCell project from Virginia Tech. Provides SBML Level 2 document handling.
Not on Maven Central. Installed as `jigcell:sbml2:local`.

## Test dependencies

**JUnit 4.11** —
The standard Java testing framework. This is the only dependency resolved from Maven Central
(`junit:junit:4.11`).

## JARs we excluded

**EvoSuite** (`evosuite.jar`) —
Originally used by the EvoSuite-generated tests for the `EvoSuiteRunner` custom JUnit runner
and `Sandbox` security manager. After removing the EvoSuite scaffolding from all test files
(see PATCHLOG.md), no test code references EvoSuite classes, so this dependency was dropped
entirely.

**cytoscape_old.jar** (in the project root) —
This appears to be an older copy of the Cytoscape JAR. It is not referenced by the Ant build
or the new Maven build, so we did not include it.
