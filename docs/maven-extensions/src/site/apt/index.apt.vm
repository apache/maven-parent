 -----
 Introduction
 -----
 Hervé Boutemy
 -----
 2020-01-16
 -----

~~ Licensed to the Apache Software Foundation (ASF) under one
~~ or more contributor license agreements.  See the NOTICE file
~~ distributed with this work for additional information
~~ regarding copyright ownership.  The ASF licenses this file
~~ to you under the Apache License, Version 2.0 (the
~~ "License"); you may not use this file except in compliance
~~ with the License.  You may obtain a copy of the License at
~~
~~   http://www.apache.org/licenses/LICENSE-2.0
~~
~~ Unless required by applicable law or agreed to in writing,
~~ software distributed under the License is distributed on an
~~ "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY
~~ KIND, either express or implied.  See the License for the
~~ specific language governing permissions and limitations
~~ under the License.

~~ NOTE: For help with the syntax of this file, see:
~~ https://maven.apache.org/doxia/references/apt-format.html

${project.name}

    This POM is the common parent of all of the {{{/extensions/}Maven extensions}}
    in the Apache Maven project.

* The <<<run-its>>> Profile

    This POM provides <<<run-its>>> profile for running Integration Tests to check real extension execution.
    A default configuration for <<<maven-invoker-plugin>>> is defined, that every extension needs to enhance
    to match its prerequisite. Then ITs are launched in every extension with following command:

+---+
mvn -Prun-its verify
+---+


* Site Publication

    Since Maven extensions are always mono-module builds, this parent POM has configured <<<maven-scm-publish-plugin>>>
    {{{/plugins/maven-scm-publish-plugin/examples/one-module-configuration.html}mono module optimization}}
    to ease site build & deployment in only one integrated and simplified command:

+-----+
mvn -Preporting site-deploy
+-----+
