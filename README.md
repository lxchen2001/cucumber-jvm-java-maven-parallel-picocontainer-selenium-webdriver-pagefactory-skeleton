![Under construction](https://upload.wikimedia.org/wikipedia/commons/f/f9/Road-under-construction.png "Under construction")

<sup>(all together now...)</sup>

# cucumber-jvm-java-maven-parallel-picocontainer-selenium-webdriver-pagefactory-skeleton
*Because no-one else should have to suffer what I did to work this out*

All the things required to run cucumber-jvm in parallel and cross-browser in one repo. No, thank *you*.

## Usage

Convention suggests that the Maven [surefire](http://maven.apache.org/surefire) should be used for unit tests and the [failsafe](http://maven.apache.org/surefire/maven-failsafe-plugin/usage.html) plugin should be used for integration testing.  The main reason for this is that surefire will automatically fail the build on any unit test failure but the failsafe plugin allows for greater developer control over how failure should impact overall build status.

`mvn test` to run unit tests (currently just an example unit test)

`mvn verify` to run the feature/scenario you're currently working on (tagged with `@wip`)

`mvn verify -Dcucumber.opts="--tags @foo"` to run features/scenarios tagged `@foo`

`mvn -Pparallel-cuke verify` to run in parallel all tests tagged `@complete`

> It's worth being aware that the failsafe `integration-test` goal will NOT fail a build, this is why you would typically run `verify`.  Note also unit tests will ALWAYS run regardless of your target goal - this is intentional.

## Requirements

- Java 8
- Maven 3.3

### Parallel test execution

Currently hardcoded in the pom.xml file to run 2 feature files at a time. This is set in the
`acceptance.test.parallel.count` property.

Results of parallel execution are in `junit` format and can be found in the `target/cucumber-parallel` directory as configured in the pom.xml file. Because of [this bug](https://github.com/cucumber/cucumber-jvm/issues/477) the `testsuite/name` value ends up being `cucumber.runtime.formatter.JUnitFormatter` :/

Parallel test functionality provided by the [cucumber-jvm-parallel-plugin](https://github.com/temyers/cucumber-jvm-parallel-plugin) and the [maven-failsafe-plugin](http://maven.apache.org/surefire/maven-failsafe-plugin/usage.html).

### Cross browser

`mvn test -Dbrowser=chrome`

Options are:

- `firefox` (default)
- `chrome`
- `ie`

Driver executable locations are currently hardcoded in the browser classes in the `automation.ui`
packages. Hack as necessary.

## Project status

- [x] [Mavenized](https://maven.apache.org/pom.html) the project
- [x] [Cucumber](https://cucumber.io/) test runner, because shiny
- [x] [Picocontainer integration](https://cucumber.io/blog/2015/07/08/polymorphic-step-definitions), because [dependency injection](http://martinfowler.com/articles/injection.html)
- [x] JUnit format output, because [continuous delivery](https://continuousdelivery.com/foundations/test-automation/)
- [x] Parallel test execution, because slightly faster snail
- [x] [Selenium Webdriver](http://www.seleniumhq.org/projects/webdriver/), because browser testing
- [x] [PageFactory](https://github.com/SeleniumHQ/selenium/wiki/PageFactory), because page object model
- [x] Cross-browser, because cross-browser
- [ ] Target env a runtime switch, because flexible
- [x] `mvn test` for wip, `mvn verify` for regression
- [x] Logging with slf4j/logback, because useful
- [ ] Example tests

## Acknowledgements

- https://opencredo.com/test-automation-concepts-parallel-test-execution/
- https://azevedorafaela.wordpress.com/2015/11/25/polymorphic-step-definitions-with-cucumber-jvm/
