- Maybe be irrelevant, after java is used to manage packages for latest builds
- create a maven project using maven archetype or selecting create a maven project to select from a list of project
- POM.xml contains all the information in maven like dependency name, version etc..
- ****Maven**** is a build automation tool developed using the Java programming language. It is primarily used for Java-based projects to manage the build process, including source code compilation, testing, packaging, and more. Maven utilizes the Project Object Model (POM), where the `pom.xml` file describes the project’s configuration and dependency management.[Welcome to Apache Maven – Maven](https://maven.apache.org/)
- One can lookup to this as a sign of resource [Maven Developer Centre – Maven](https://maven.apache.org/developers/index.html) 
- Maven Java follows *palantir java format* but not the *google format*

1) palantir coding style

```java
 private static void configureResolvedVersionsWithVersionMapping(Project project) {
    project.getPluginManager().withPlugin("maven-publish", plugin -> {
        project.getExtensions()
                .getByType(PublishingExtension.class)
                .getPublications()
                .withType(MavenPublication.class)
                .configureEach(publication -> publication.versionMapping(mapping -> {
                    mapping.allVariants(VariantVersionMappingStrategy::fromResolutionResult);
                }));
    });
}
```

2) google code format

``` java
private static GradleException notFound(
        String group, String name, Configuration configuration) {
    String actual =
            configuration.getIncoming().getResolutionResult().getAllComponents().stream()
                    .map(ResolvedComponentResult::getModuleVersion)
                    .map(
                            mvi ->
                                    String.format(
                                            "\t- %s:%s:%s",
                                            mvi.getGroup(), mvi.getName(), mvi.getVersion()))
                    .collect(Collectors.joining("\n"));
    // ...
}
```

### What is *"SVN"*?

- SVN, short for Subversion, is ==a free, open-source, centralized version control system used by software developers to track changes to files and maintain their history, allowing teams to collaborate effectively==
#### key features
	
- Version Control: SVN tracks every change made to files and their history are stored on a central server, and developers connect to this server to make changes and commit to them
- Collaboration: It enables multiple developers to work on the same project simultaneously, managing changes and ensuring that everyone has the latest version of the code
- Branching and Tagging: SVN supports branching (creating separate versions of the project for different features or releases) and tagging (marking specific versions for release or milestones)
- Centralized Repository: All files and their history are stored in a central repository, making it easy to manage and access. [home to Apache SVN](https://subversion.apache.org/)
- Home to add it to maven [home to maven wiki](https://cwiki.apache.org/confluence/display/MAVEN/Index)

**MAVEN CLI COMMANDS**



