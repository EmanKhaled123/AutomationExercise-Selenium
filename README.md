# AutomationExercise – Selenium Test Automation Framework

![Java](https://img.shields.io/badge/Java-8%2B-orange)
![Selenium](https://img.shields.io/badge/Selenium-4.43.0-43B02A)
![TestNG](https://img.shields.io/badge/TestNG-7.12.0-blue)
![Maven](https://img.shields.io/badge/Build-Maven-C71A36)
![Apache POI](https://img.shields.io/badge/Apache%20POI-5.5.1-D22128)

A Java + Selenium WebDriver test automation framework for [automationexercise.com](https://automationexercise.com/), built with the Page Object Model (POM), TestNG, and Maven. It covers Login, Registration, Product/Cart, Checkout, and Contact Us flows, and demonstrates several data-driven testing (DDT) approaches side by side (hardcoded, TestNG @DataProvider, Java .properties files, and Excel via Apache POI).

## Table of Contents

- [Tech Stack](#tech-stack)
- [Project Structure](#project-structure)
- [Prerequisites](#prerequisites)
- [Configuration](#configuration)
- [Running the Tests](#running-the-tests)
- [Test Reports](#test-reports)
- [Data-Driven Testing Approaches](#data-driven-testing-approaches)
- [Notes](#notes)

## Tech Stack

- **Java**
- **Selenium WebDriver 4.43.0** – browser automation
- **TestNG 7.12.0** – test runner, suites, assertions, data providers
- **Apache POI (poi-ooxml) 5.5.1** – reading test data from Excel (.xlsx)
- **Maven** – build and dependency management

## Project Structure

```
├── pom.xml                      # Maven project descriptor / dependencies
├── MasterSuite.xml              # Aggregates all suite files below
├── Login_Suite.xml              # Login happy/negative TestNG suite
├── Register_Suite.xml           # Registration happy/negative TestNG suite
├── ContactUs_Suite.xml          # Contact Us TestNG suite
├── Images/                      # Test fixtures (e.g. profile image upload)
│
├── src/main/java/pages/         # Page Object Model classes
│   ├── PageBase.java            # Base class - PageFactory initialization
│   ├── HomePage.java
│   ├── LoginPage.java
│   ├── RegisterPage.java
│   ├── ProductsPage.java
│   ├── CartPage.java
│   └── ContactUsPage.java
│
└── src/test/java/
    ├── tests/                   # TestNG test classes (one scenario per class)
    │   ├── TestBase.java        # WebDriver setup/teardown (@BeforeTest/@AfterTest)
    │   ├── Login_*.java         # Login scenarios (happy/negative, 4 DDT styles)
    │   ├── Register_*.java      # Registration scenarios (happy/negative)
    │   ├── ContactUs_happyScenario.java
    │   ├── AddToCart*.java, RemoveFromCart.java, VerifyCart.java, ...
    │   ├── PlaceOrderRegister*.java, VerifyAddressCheckout.java
    │   └── VerifyProducts*.java, VerifySubscription.java, VerifyScrollUp*.java
    ├── data/                    # Helpers that load test data
    │   ├── LoadBrowserCommandsProperties.java
    │   ├── LoadLoginHappyDataProperties.java / LoadLoginNegativeDataProperties.java
    │   └── LoadLoginHappyDataExcel.java / LoadLoginNegativeDataExcel.java
    ├── PropertiesFiles/         # BrowserCommands.properties, Login*.properties
    └── Excel/                   # LoginData.xlsx, Register_happyData.xlsx
```

## Prerequisites

- JDK 8+ (JDK 11 or later recommended)
- Maven 3.6+
- Google Chrome, Mozilla Firefox, or Microsoft Edge installed
- Selenium Manager (bundled with Selenium 4.6+) resolves browser drivers automatically — no manual driver downloads required

## Configuration

Browser and timeout settings are controlled in:

src/test/java/PropertiesFiles/BrowserCommands.properties

```
browserName=chrome
timeout=15
```

Change browserName to chrome, firefox, or edge to switch browsers. timeout sets the implicit wait (in seconds).

## Running the Tests

Run everything via the master suite:
```
mvn test -DsuiteXmlFile=MasterSuite.xml
```

Run an individual suite:
```
mvn test -DsuiteXmlFile=Login_Suite.xml
mvn test -DsuiteXmlFile=Register_Suite.xml
mvn test -DsuiteXmlFile=ContactUs_Suite.xml
```

Run a single test class:
```
mvn test -Dtest=Login_happyScenarioWithoutDDT
```

You can also run any suite XML or test class directly from an IDE (Eclipse/IntelliJ) with the TestNG plugin installed.

## Test Reports

TestNG generates its default HTML/XML reports after each run in:

```
test-output/
├── emailable-report.html
├── index.html
└── junitreports/
```

Open test-output/index.html in a browser to view results.

## Data-Driven Testing Approaches

The Login and Registration flows intentionally demonstrate multiple DDT strategies for comparison/learning purposes:

| Approach | Example class |
|---|---|
| Hardcoded values | Login_happyScenarioWithoutDDT |
| TestNG @DataProvider | Login_happyScenarioWithDDTAndDataProvider |
| Java .properties file | Login_happyScenarioWithDDTAndJavaPropertiesFile |
| Excel (.xlsx via Apache POI) | Login_happyScenarioWittDDTAndExcel |

## Notes

- Base URL under test: https://automationexercise.com/
- Each test class extends TestBase, which opens/maximizes the browser before the test and quits the driver afterward.
- target/ and test-output/ are build/run artifacts and should not be committed — see .gitignore.
