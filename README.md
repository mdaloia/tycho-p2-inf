# Example Tycho project to show a Tycho regression

This is a reproducer project to ease the support around the support question
`Virtual configuration IU in p2.inf does not work in 4.0.3 (works in 3.0.5)`
asked in https://github.com/eclipse-tycho/tycho/discussions


## Build

Use this command to build it:
`mvn clean package -Dtycho.localArtifacts=ignore -Dtycho-version=4.0.3 -Dtycho.resolver.classic=false`

Will fail with this error:
```
[ERROR] Cannot resolve project dependencies:
[ERROR]   Software being installed: p2.inf 1.0.0.qualifier
[ERROR]   Missing requirement: p2.inf 1.0.0.qualifier requires 'org.eclipse.equinox.p2.iu; configure.p2.inf 0.0.0' but it could not be found
```


Downgrade the version:
`mvn clean package -Dtycho.localArtifacts=ignore -Dtycho-version=3.0.5`

and it will work. It generates the `target/p2content.xml` file with the `<unit id='configure.p2.inf'>` and the `<required namespace='org.eclipse.equinox.p2.iu' name='configure.p2.inf' range='0.0.0'/>`

You will see this message:
```
[INFO] The following requirements are not satisfied yet and must be provided through pom dependencies:
[INFO]    - org.eclipse.equinox.p2.iu; configure.p2.inf 0.0.0
```
This is because of `<pomDependencies>consider</pomDependencies>`. If it is not specified it fails as with 4.0.3


### Links for reference:
- https://github.com/eclipse-tycho/tycho/blob/master/RELEASE_NOTES.md#mixed-reactor-setups-require-the-new-resolver-now
- https://github.com/eclipse-tycho/tycho/blob/master/tycho-its/projects/p2Inf.multiEnv/
- https://github.com/eclipse-tycho/tycho/blob/4c276d99b3ac591e130a60303f19043c35434757/tycho-its/src/test/java/org/eclipse/tycho/test/p2Inf/MultienvP2infTest.java

- https://wiki.eclipse.org/Equinox/p2/Customizing_Metadata
- https://wiki.eclipse.org/Equinox/p2/Setting_Start_Levels
- https://stackoverflow.com/questions/17855554/installable-units-are-missing-in-p2-repository-so-that-tycho-p2-director-fails
- https://stackoverflow.com/questions/3077282/in-equinox-is-it-possible-to-to-mark-an-osgi-bundle-as-started-from-its-containi
- https://wiki.eclipse.org/Tycho/Target_Platform#Dependency_resolution_troubleshooting
