# ce-github-pages
Contains static HTML pages published on github.io
## Adding extra projects
To add a new project to the overview, add a new entry in the POM.xml under the<executions> of the maven-cucumber-reporting plugin
````xml
<execution>
    <id>execution-X</id>                                                  <!-- X: previous execution +1 -->
    <phase>compile</phase>
    <goals>
        <goal>generate</goal>
    </goals>
    <configuration>
        <projectName>NAME</projectName>                                   <!-- NAME: name of the project -->
        <outputDirectory>${basedir}/cucumber-html/NAME</outputDirectory>  <!-- NAME: name of the project -->
        <inputDirectory>json</inputDirectory>
        <jsonFiles>
            <param>REPO/*cucumber.json</param>                            <!-- REPO: repository name of project -->
        </jsonFiles>
        <checkBuildResult>false</checkBuildResult>
    </configuration>
</execution>
````

## Flow:
1. Integration tests are triggered by an upstream repo (through the it-test-code action)
2. The cucumber.json files produced by the integration tests are pushed to this repo under json>'repo-name'
3. On push to this repo, the build action is triggered which executes mvn compile
4. mvn compile is defined as the goal phase of the cucumber-reporter plugin in the pom.xml. This plugin generates HTML code based on the json files
5. The build action then creates an index.html page with links to all found projects under cucumber-html

## URL:
https://acerta-connect-evolution.github.io/ce-bdd-reports
