# 📚 Quick Reference Guide - SauceDemo Test Automation

A handy reference for common commands, code snippets, and troubleshooting tips.

## 🚀 Quick Start Commands

### Maven Commands
```bash
# Clean and compile
mvn clean compile

# Run all tests
mvn test

# Run specific test class
mvn test -Dtest=LoginTest

# Run with TestNG suite
mvn test -DsuiteXmlFile=testng.xml

# Skip tests (compile only)
mvn clean compile -DskipTests

# Download dependencies
mvn dependency:resolve
```

### Java Commands
```bash
# Check Java version
java -version

# Compile Java file
javac MyClass.java

# Run Java class
java MyClass
```

## 🔧 Common Code Snippets

### WebDriver Setup
```java
// Basic WebDriver setup
WebDriverManager.chromedriver().setup();
WebDriver driver = new ChromeDriver();
driver.manage().window().maximize();

// With options
ChromeOptions options = new ChromeOptions();
options.addArguments("--headless");
options.addArguments("--no-sandbox");
WebDriver driver = new ChromeDriver(options);
```

### Common Locators
```java
// By ID (most reliable)
By.id("user-name")

// By Name
By.name("username")

// By Class Name
By.className("login-button")

// By XPath
By.xpath("//input[@id='user-name']")
By.xpath("//button[contains(text(),'Login')]")

// By CSS Selector
By.cssSelector("#user-name")
By.cssSelector(".login-button")
By.cssSelector("input[type='password']")

// By Link Text
By.linkText("Forgot Password?")

// By Partial Link Text
By.partialLinkText("Forgot")
```

### Wait Strategies
```java
// Implicit Wait
driver.manage().timeouts().implicitlyWait(10, TimeUnit.SECONDS);

// Explicit Wait
WebDriverWait wait = new WebDriverWait(driver, Duration.ofSeconds(10));
wait.until(ExpectedConditions.elementToBeClickable(loginButton));

// Fluent Wait
Wait<WebDriver> wait = new FluentWait<>(driver)
    .withTimeout(Duration.ofSeconds(30))
    .pollingEvery(Duration.ofSeconds(5))
    .ignoring(NoSuchElementException.class);
```

### Common Actions
```java
// Click element
driver.findElement(By.id("button")).click();

// Send text
driver.findElement(By.id("input")).sendKeys("text");

// Clear and send text
WebElement element = driver.findElement(By.id("input"));
element.clear();
element.sendKeys("new text");

// Get text
String text = driver.findElement(By.id("element")).getText();

// Get attribute
String value = driver.findElement(By.id("input")).getAttribute("value");

// Check if element is displayed
boolean isDisplayed = driver.findElement(By.id("element")).isDisplayed();

// Check if element is enabled
boolean isEnabled = driver.findElement(By.id("button")).isEnabled();

// Check if element is selected
boolean isSelected = driver.findElement(By.id("checkbox")).isSelected();
```

## 🧪 TestNG Annotations

```java
@BeforeSuite    // Runs once before all tests in suite
@BeforeTest     // Runs before each <test> tag
@BeforeClass    // Runs once before all tests in class
@BeforeMethod   // Runs before each test method
@Test           // Marks a method as a test
@AfterMethod    // Runs after each test method
@AfterClass     // Runs once after all tests in class
@AfterTest      // Runs after each <test> tag
@AfterSuite     // Runs once after all tests in suite
```

### TestNG Assertions
```java
// Basic assertions
Assert.assertTrue(condition, "Message if fails");
Assert.assertFalse(condition, "Message if fails");
Assert.assertEquals(actual, expected, "Message if fails");
Assert.assertNotEquals(actual, expected, "Message if fails");
Assert.assertNull(object, "Message if fails");
Assert.assertNotNull(object, "Message if fails");

// Soft assertions (collects all failures)
SoftAssert softAssert = new SoftAssert();
softAssert.assertTrue(condition1, "First assertion");
softAssert.assertEquals(actual, expected, "Second assertion");
softAssert.assertAll(); // Throws exception if any failed
```

### Data Providers
```java
@DataProvider(name = "testData")
public Object[][] getTestData() {
    return new Object[][] {
        {"value1", "value2"},
        {"value3", "value4"}
    };
}

@Test(dataProvider = "testData")
public void testMethod(String param1, String param2) {
    // Test implementation
}
```

## 🎭 Page Object Model Template

### Base Page
```java
public class BasePage {
    protected WebDriver driver;
    protected WebDriverWait wait;
    
    public BasePage(WebDriver driver) {
        this.driver = driver;
        this.wait = new WebDriverWait(driver, Duration.ofSeconds(10));
    }
    
    protected void click(By locator) {
        wait.until(ExpectedConditions.elementToBeClickable(locator)).click();
    }
    
    protected void sendKeys(By locator, String text) {
        wait.until(ExpectedConditions.presenceOfElementLocated(locator)).sendKeys(text);
    }
    
    protected String getText(By locator) {
        return wait.until(ExpectedConditions.presenceOfElementLocated(locator)).getText();
    }
}
```

### Page Class Template
```java
public class LoginPage extends BasePage {
    
    // Locators
    private By usernameField = By.id("user-name");
    private By passwordField = By.id("password");
    private By loginButton = By.id("login-button");
    
    // Constructor
    public LoginPage(WebDriver driver) {
        super(driver);
    }
    
    // Actions
    public void enterUsername(String username) {
        sendKeys(usernameField, username);
    }
    
    public void enterPassword(String password) {
        sendKeys(passwordField, password);
    }
    
    public void clickLogin() {
        click(loginButton);
    }
    
    public void login(String username, String password) {
        enterUsername(username);
        enterPassword(password);
        clickLogin();
    }
}
```

## 🐛 Common Errors and Solutions

### WebDriver Issues
```java
// Error: ChromeDriver not found
// Solution: Use WebDriverManager
WebDriverManager.chromedriver().setup();

// Error: Element not found
// Solution: Add explicit wait
WebDriverWait wait = new WebDriverWait(driver, Duration.ofSeconds(10));
wait.until(ExpectedConditions.presenceOfElementLocated(locator));

// Error: Element not clickable
// Solution: Wait for element to be clickable
wait.until(ExpectedConditions.elementToBeClickable(locator)).click();
```

### TestNG Issues
```java
// Error: Test not running
// Solution: Check @Test annotation and method visibility
@Test
public void testMethod() { // Must be public
    // Test code
}

// Error: DataProvider not working
// Solution: Ensure correct method signature
@Test(dataProvider = "dataProviderName")
public void testMethod(String param1, String param2) {
    // Test code
}
```

### Maven Issues
```bash
# Error: Dependencies not downloading
# Solution: Clear Maven cache
mvn clean
mvn dependency:purge-local-repository

# Error: Compilation failed
# Solution: Check Java version
mvn -version
java -version
```

## 📊 Reporting Commands

### TestNG Reports
```bash
# Generate TestNG reports
mvn test

# View reports
open target/surefire-reports/index.html
```

### ExtentReports Setup
```java
// In your test class
ExtentReports extent = new ExtentReports();
ExtentSparkReporter spark = new ExtentSparkReporter("reports/ExtentReport.html");
extent.attachReporter(spark);

@Test
public void testMethod() {
    ExtentTest test = extent.createTest("Test Method");
    test.log(Status.INFO, "Test started");
    // Test code
    test.log(Status.PASS, "Test passed");
    extent.flush();
}
```

## 🔍 Debugging Tips

### Browser Developer Tools
```javascript
// Find element by ID
document.getElementById("user-name")

// Find element by class
document.getElementsByClassName("login-button")

// Find element by XPath
$x("//input[@id='user-name']")

// Check if element exists
document.getElementById("user-name") !== null
```

### Console Debugging
```java
// Print current URL
System.out.println("Current URL: " + driver.getCurrentUrl());

// Print page title
System.out.println("Page Title: " + driver.getTitle());

// Print element text
System.out.println("Element Text: " + driver.findElement(By.id("element")).getText());

// Take screenshot
TakesScreenshot screenshot = (TakesScreenshot) driver;
File sourceFile = screenshot.getScreenshotAs(OutputType.FILE);
```

## 🌐 SauceDemo Test Data

### Valid Users
```java
// Standard user
String username = "standard_user";
String password = "secret_sauce";

// Problem user
String username = "problem_user";
String password = "secret_sauce";

// Performance glitch user
String username = "performance_glitch_user";
String password = "secret_sauce";
```

### Invalid Users
```java
// Locked out user
String username = "locked_out_user";
String password = "secret_sauce";

// Invalid credentials
String username = "invalid_user";
String password = "wrong_password";
```

## 📁 Project Structure Template

```
project/
├── src/
│   ├── main/java/
│   │   └── com/company/project/
│   │       ├── pages/
│   │       │   ├── BasePage.java
│   │       │   ├── LoginPage.java
│   │       │   └── ProductPage.java
│   │       └── utils/
│   │           ├── DriverManager.java
│   │           └── ConfigReader.java
│   └── test/java/
│       └── com/company/project/
│           ├── testclasses/
│           │   ├── BaseTest.java
│           │   └── LoginTest.java
│           └── runners/
│               └── TestRunner.java
├── src/test/resources/
│   ├── features/
│   │   └── login.feature
│   └── config.properties
├── reports/
├── screenshots/
├── pom.xml
└── testng.xml
```

## 🚀 Performance Tips

### Optimize Locators
```java
// Good - ID is fastest
By.id("user-name")

// Good - CSS selector is fast
By.cssSelector("#user-name")

// Avoid - XPath is slower
By.xpath("//input[@id='user-name']")

// Avoid - Text-based locators
By.xpath("//button[text()='Login']")
```

### Optimize Waits
```java
// Good - Use explicit waits
WebDriverWait wait = new WebDriverWait(driver, Duration.ofSeconds(10));

// Avoid - Hardcoded delays
Thread.sleep(5000);

// Good - Wait for specific conditions
wait.until(ExpectedConditions.elementToBeClickable(locator));
```

### Optimize Test Execution
```java
// Good - Reuse driver instance
@BeforeClass
public void setUp() {
    driver = DriverManager.getDriver();
}

// Avoid - Creating new driver for each test
@BeforeMethod
public void setUp() {
    driver = new ChromeDriver(); // Creates new instance each time
}
```

## 📞 Emergency Commands

### When Everything Breaks
```bash
# Nuclear option - clean everything
mvn clean
rm -rf target/
rm -rf ~/.m2/repository/org/seleniumhq/
mvn clean install

# Reset WebDriver
# Delete chromedriver from system PATH
# Let WebDriverManager download fresh copy

# Check Java/Maven versions
java -version
mvn -version

# Verify Chrome installation
# Open Chrome browser manually
```

### Quick Test
```java
// Minimal test to verify setup
@Test
public void quickTest() {
    WebDriverManager.chromedriver().setup();
    WebDriver driver = new ChromeDriver();
    driver.get("https://www.google.com");
    System.out.println("Title: " + driver.getTitle());
    driver.quit();
}
```

---

## 💡 Pro Tips

1. **Always use WebDriverManager** - handles driver downloads automatically
2. **Use explicit waits** - more reliable than implicit waits
3. **Keep locators in page objects** - easier to maintain
4. **Use descriptive test names** - makes debugging easier
5. **Take screenshots on failure** - helps with debugging
6. **Use data providers** - makes tests more maintainable
7. **Keep tests independent** - each test should work alone
8. **Use soft assertions** - collect multiple failures
9. **Version control your tests** - track changes over time
10. **Document your framework** - helps team members understand

---

**Keep this guide handy while learning! 📚**

