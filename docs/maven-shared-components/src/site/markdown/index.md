<!--
Licensed to the Apache Software Foundation (ASF) under one
or more contributor license agreements.  See the NOTICE file
distributed with this work for additional information
regarding copyright ownership.  The ASF licenses this file
to you under the Apache License, Version 2.0 (the
"License"); you may not use this file except in compliance
with the License.  You may obtain a copy of the License at

http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing,
software distributed under the License is distributed on an
"AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY
KIND, either express or implied.  See the License for the
specific language governing permissions and limitations
under the License.
-->

# Apache Maven Shared Components Parent POM
This POM is the common parent of all of the [Maven Shared Components](/shared/) in the Apache Maven project.

## Site Publication

Since Shared Components are always mono-module builds, this parent POM has configured `maven-scm-publish-plugin` [mono module optimization](/plugins/maven-scm-publish-plugin/examples/one-module-configuration.html) to ease site build &amp; deployment in only one integrated and simplified command:

```shell
mvn -Preporting site-deploy
```

## History

As of version 38, this POM sets the Java source and target versions to 1.8. Thus, as any plugin (or other component) moved to version 38+ of this POM, it moves to requiring Java 1.8 (was Java 1.5 since version 21, Java 1.6 since version 27, and Java 1.7 since version 34).
