# sbt-softwaremill
[![Build Status](https://travis-ci.org/softwaremill/sbt-softwaremill.svg?branch=master)](https://travis-ci.org/softwaremill/sbt-softwaremill)
[![Maven Central](https://maven-badges.herokuapp.com/maven-central/com.softwaremill.sbt-softwaremill/sbt-softwaremill/badge.svg)](https://maven-badges.herokuapp.com/maven-central/com.softwaremill.sbt-softwaremill/sbt-softwaremill)  
A sane set of common build settings.

## Usage

First, add the plugin to your `project/plugins.sbt` file:

````scala
addSbtPlugin("com.softwaremill.sbt-softwaremill" % "sbt-softwaremill" % "1.2.2")
````

Now you can add `smlBuildSettings` to any set of build settings:

````scala
lazy val commonSettings = smlBuildSettings ++Seq(
  // your settings, which can override some of smlBuildSettings
) 
````

If you only want to import some settings, you can use any subset of `smlBuildSettings`:

````scala
    lazy val smlBuildSettings =
      commonSmlBuildSettings    ++ // compiler flags
      wartRemoverSettings       ++ // warts
      clippyBuildSettings       ++ // enable clippy colors
      acyclicSettings           ++ // check circular dependencies between packages
      splainSettings            ++ // gives rich output on implicit resolution errors 
      dependencyUpdatesSettings ++ // check dependency updates on startup (max once per 12h)
      ossPublishSettings           // configures common publishing process for all OSS libraries
````

`sbt-softwaremill` comes with:
- Coursier
- Scalafmt
- sbt-pgp
- sbt-release
- sbt-sonatype
- scala-clippy
- sbt-updates
- splain
- sbt-reloadquick
- sbt-revolver
- acyclic

## Releasing your library

`sbt-softwaremill` comes with a default configration suitable for releasing open source libraries.
Just add `smlBuildSettings` or `ossPublishSettings` to your project's settings and you're all set, just run the 'release' command.
Consider that:
- You need an [OSS Sonatype account](https://www.scala-sbt.org/1.x/docs/Using-Sonatype.html) and sbt-pgp plugin properly configured with generated and published keys.
- Your README.md will be parsed for `"groupId" %(%) "artifactId" % "someVersion"` and that version value will be bumped.
- If you a have multi-module project, you may need to add `publishArtifact := false` to your root project's settings. 