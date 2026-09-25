---
title: Upgrading to Maven Parent POM 50
author:
  - Sylwester Lachiewicz
date: 2026-09-25
---

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

# Upgrading to Maven Parent POM 50

Version 50 of this parent upgrades [apache-parent](https://maven.apache.org/pom/apache/) from 39 to 40,
which brings apache-rat-plugin 0.18, spotless-maven-plugin 3.10.2 and maven-compiler-plugin 3.16.0.
This page lists the changes observed when migrating child projects, with the fix for each.

## apache-rat-plugin 0.18

### Rename `<excludes>` to `<inputExcludes>`

Rat 0.18 replaced the deprecated `excludes` parameter with `inputExcludes`. When the parent's
`inputExcludes` is set, values declared by a child under the old `excludes` parameter are ignored,
so custom exclusions silently stop working and the check fails with
`Counter(s) UNAPPROVED exceeded minimum or maximum values`.

Rename the parameter in every child configuration, keeping the append semantics:

```xml
<inputExcludes combine.children="append">
  ...
</inputExcludes>
```

### Run rat only on JDK 17+

Rat 0.18 requires Java 17. This parent binds the check only in the `java17+` profile, so projects
must not declare the plugin unconditionally in `<build><plugins>` - such a declaration re-activates
it on older JDKs and fails with `The plugin ... has unmet prerequisites: Required Java version 17`.
Move the declaration into a profile with the same id, which merges with the parent's profile and
inherits its activation:

```xml
<profile>
  <id>java17+</id>
  <build>
    <plugins>
      <plugin>
        <groupId>org.apache.rat</groupId>
        <artifactId>apache-rat-plugin</artifactId>
        <configuration>
          <inputExcludes combine.children="append">
            ...
          </inputExcludes>
        </configuration>
      </plugin>
    </plugins>
  </build>
</profile>
```

### Patterns are matched root-anchored

Rat 0.18 matches exclude patterns against the path from the reactor root, so entries anchored at a
module-relative path no longer match. Prefix such entries with `**/`:

```xml
<!-- no longer matches in a multi-module build -->
<exclude>src/test/resources/jars/*.jar</exclude>
<!-- matches -->
<exclude>**/src/test/resources/jars/*.jar</exclude>
```

### Binary files are counted as unapproved

Rat 0.18 counts binary test fixtures (jars, images, UTF-16 encoded files) that earlier versions
skipped. Exclude them explicitly.

## `version.maven-surefire` property removed

The `version.maven-surefire` property was removed in apache-parent 40. Use
`version.maven-surefire-plugin`, `version.maven-failsafe-plugin` or
`version.maven-surefire-report-plugin` instead. Check in particular:

- invoker IT projects interpolating `@version.maven-surefire@` tokens,
- archetype templates emitting `${version.maven-surefire}`,
- IT project POMs that inherit the parent directly.

## spotless runs in `check` mode on CI

With `env.CI` set, spotless-maven-plugin 3.10.2 runs as `check` and fails the build on any
formatting deviation, including `sortPom` formatting of POM files. Run `mvn spotless:apply` locally
and commit the result before pushing.
