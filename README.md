# SauceDemo Test Automation Project

A comprehensive test automation framework for testing the SauceDemo e-commerce application using Selenium WebDriver, TestNG, and Cucumber BDD.

## 🚀 Project Overview

This project provides automated testing for the SauceDemo application (https://www.saucedemo.com/), covering various user scenarios including login functionality, product browsing, cart management, and checkout processes. The framework is built using industry-standard tools and follows best practices for maintainable and scalable test automation.

## 🛠️ Technology Stack

- **Java 21** - Programming language
- **Selenium WebDriver 4.15.0** - Web automation framework
- **TestNG 7.8.0** - Test execution and reporting
- **Cucumber 7.18.1** - BDD (Behavior Driven Development) framework
- **Maven** - Build and dependency management
- **WebDriverManager 5.6.2** - Automatic driver management
- **ExtentReports 5.1.1** - Advanced HTML reporting
- **ChromeDriver** - Browser automation (Chrome)

## 📁 Project Structure

```
Capstone-project/
├── src/
│   ├── main/java/saucedemo/
│   │   ├── pages/                    # Page Object Model classes
│   │   │   ├── BasePage.java         # Base page with common functionality
│   │   │   ├── LoginPage.java        # Login page interactions
│   │   │   ├── ProductPage.java      # Product listing page
│   │   │   ├── CartPage.java         # Shopping cart page
│   │   │   └── CheckOutPage.java     # Checkout process page
│   │   └── utils/                    # Utility classes
│   │       ├── Driver_Manager.java   # WebDriver management
│   │       ├── ExtentReport_Manager.java # Test reporting
│   │       └── ScreenshotUtil.java   # Screenshot utilities
│   └── test/
│       ├── java/saucedemo/
│       │   ├── runners/
│       │   │   └── TestRunner.java   # Cucumber test runner
│       │   ├── stepDefinitions/      # Cucumber step definitions
│       │   │   ├── CucumberHooks.java # Setup/teardown hooks
│       │   │   └── LoginPageSteps.java # Login page steps
│       │   └── testclasses/          # TestNG test classes
│       │       ├── BaseTest.java     # Base test class
│       │       ├── LoginPageTest.java # Login functionality tests
│       │       ├── ProductPageTest.java # Product page tests
│       │       ├── CartPageTest.java # Cart functionality tests
│       │       └── CheckOutPageTest.java # Checkout tests
│       └── resources/features/
│           └── LoginPageFeature.feature # BDD feature files
├── reports/                          # Test execution reports
├── screenshots/                      # Test execution screenshots
├── test-output/                      # TestNG reports
├── target/                          # Maven build output
├── pom.xml                          # Maven configuration
└── testng.xml                       # TestNG suite configuration
```

## 🎯 Test Coverage

### Login Functionality
- ✅ Valid user login
- ✅ Invalid credentials handling
- ✅ Locked out user scenarios
- ✅ URL validation and logo verification

### E-commerce Features
- ✅ Product browsing and selection
- ✅ Shopping cart management
- ✅ Checkout process validation

## 🚀 Getting Started

### Prerequisites

- Java 21 or higher
- Maven 3.6 or higher
- Chrome browser installed
- Internet connection

### Installation

1. **Clone the repository**
   ```bash
   git clone <repository-url>
   cd Capstone-project
   ```

2. **Install dependencies**
   ```bash
   mvn clean install
   ```

3. **Verify installation**
   ```bash
   mvn --version
   java --version
   ```

## 🏃‍♂️ Running Tests

### Run All Tests (TestNG)
```bash
mvn test
```

### Run Specific Test Class
```bash
mvn test -Dtest=LoginPageTest
```

### Run Cucumber Tests
```bash
mvn test -Dtest=TestRunner
```

### Run with TestNG Suite
```bash
mvn test -DsuiteXmlFile=testng.xml
```

## 📊 Test Reports

After test execution, reports are generated in multiple formats:

### ExtentReports
- **Location**: `reports/Saucedemo_Report.html`
- **Features**: Interactive HTML reports with detailed test results, screenshots, and analytics

### TestNG Reports
- **Location**: `test-output/`
- **Features**: Standard TestNG HTML reports with test execution details

### Cucumber Reports
- **Location**: `reports/cucumberReport.html`
- **Features**: BDD-style reports showing feature execution status

### Screenshots
- **Location**: `screenshots/`
- **Features**: Automatic screenshots captured during test failures

## 🔧 Configuration

### Browser Configuration
The framework is configured to use Chrome browser by default. WebDriverManager automatically handles driver downloads and setup.

### Test Data
Test data is currently hardcoded in the test classes. For production use, consider externalizing test data to:
- Properties files
- JSON files
- Excel/CSV files
- Database

### Environment Configuration
- **Base URL**: https://www.saucedemo.com/
- **Browser**: Chrome (configurable)
- **Implicit Wait**: 2 seconds
- **Window**: Maximized

## 🏗️ Framework Architecture

### Page Object Model (POM)
- **BasePage**: Common functionality and WebDriverWait setup
- **Page Classes**: Specific page interactions and element locators
- **Separation of Concerns**: UI elements separated from test logic

### BDD Implementation
- **Feature Files**: Business-readable test scenarios
- **Step Definitions**: Implementation of BDD steps
- **Hooks**: Setup and teardown operations

### Reporting Strategy
- **ExtentReports**: Rich HTML reports with screenshots
- **TestNG**: Standard test execution reports
- **Cucumber**: BDD-style feature reports

## 🧪 Test Scenarios

### Login Page Tests
1. **URL Validation**: Verify SauceDemo login page loads correctly
2. **Logo Verification**: Confirm "Swag Labs" logo is displayed
3. **Valid Login**: Test successful login with standard user
4. **Invalid Login**: Test error handling for wrong credentials
5. **Locked User**: Test locked out user scenario

### Additional Test Areas
- Product page functionality
- Shopping cart operations
- Checkout process validation

## 🔍 Troubleshooting

### Common Issues

1. **ChromeDriver Issues**
   - WebDriverManager handles automatic driver management
   - Ensure Chrome browser is installed and updated

2. **Test Failures**
   - Check screenshots in `screenshots/` directory
   - Review ExtentReports for detailed failure information

3. **Build Issues**
   - Ensure Java 21 is installed and configured
   - Verify Maven installation and configuration

## 📈 Future Enhancements

- [ ] Cross-browser testing support (Firefox, Edge, Safari)
- [ ] Parallel test execution
- [ ] API testing integration
- [ ] Database validation
- [ ] Mobile testing support
- [ ] CI/CD pipeline integration
- [ ] Test data externalization
- [ ] Performance testing integration

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Add tests for new functionality
5. Ensure all tests pass
6. Submit a pull request

## 📝 License

This project is part of a capstone project and is intended for educational purposes.

## 👥 Team

Developed as part of a capstone project focusing on test automation best practices and modern testing frameworks.

---

**Note**: This framework is designed for testing the SauceDemo application and serves as a comprehensive example of modern test automation practices using Java, Selenium, TestNG, and Cucumber.
