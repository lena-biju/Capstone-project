# 🛠️ Hands-On Tutorial: Your First Test Automation Project

Welcome to your step-by-step journey! This tutorial will guide you through creating and running your first test automation project from scratch.

## 🎯 What You'll Learn

By the end of this tutorial, you will:
- ✅ Set up a complete test automation environment
- ✅ Write your first automated test
- ✅ Understand how to run tests
- ✅ Generate test reports
- ✅ Debug and fix common issues

---

## 📋 Prerequisites Checklist

Before we start, make sure you have:

- [ ] **Java 21** installed and configured
- [ ] **Maven 3.9.6** installed and configured
- [ ] **Chrome Browser** installed
- [ ] **IDE** (IntelliJ IDEA, Eclipse, or VS Code)
- [ ] **Internet connection** (for downloading dependencies)

### Quick Verification Commands

Open your terminal/command prompt and run:

```bash
# Check Java version
java -version
# Should show: openjdk version "21.x.x"

# Check Maven version
mvn -version
# Should show: Apache Maven 3.9.6

# Check Chrome (just open Chrome browser)
```

---

## 🚀 Step 1: Project Setup

### 1.1 Create New Project

**Option A: Using IDE (Recommended)**
1. Open your IDE
2. Create new Maven project
3. Choose Java 21 as project SDK
4. Set Group ID: `com.yourname`
5. Set Artifact ID: `saucedemo-automation`
6. Set Version: `1.0.0`

**Option B: Using Command Line**
```bash
mvn archetype:generate -DgroupId=com.yourname -DartifactId=saucedemo-automation -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
cd saucedemo-automation
```

### 1.2 Project Structure

Your project should look like this:
```
saucedemo-automation/
├── src/
│   ├── main/java/
│   └── test/java/
├── pom.xml
└── README.md
```

---

## 🔧 Step 2: Configure Maven Dependencies

### 2.1 Update pom.xml

Replace the content of your `pom.xml` with:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" 
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 
         http://maven.apache.org/xsd/maven-4.0.0.xsd">
    
    <modelVersion>4.0.0</modelVersion>
    <groupId>com.yourname</groupId>
    <artifactId>saucedemo-automation</artifactId>
    <version>1.0.0</version>
    <packaging>jar</packaging>
    
    <properties>
        <maven.compiler.source>21</maven.compiler.source>
        <maven.compiler.target>21</maven.compiler.target>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <selenium.version>4.15.0</selenium.version>
        <testng.version>7.8.0</testng.version>
    </properties>

    <dependencies>
        <!-- Selenium WebDriver -->
        <dependency>
            <groupId>org.seleniumhq.selenium</groupId>
            <artifactId>selenium-java</artifactId>
            <version>${selenium.version}</version>
        </dependency>

        <!-- WebDriverManager for automatic driver management -->
        <dependency>
            <groupId>io.github.bonigarcia</groupId>
            <artifactId>webdrivermanager</artifactId>
            <version>5.6.2</version>
        </dependency>

        <!-- TestNG for test execution -->
        <dependency>
            <groupId>org.testng</groupId>
            <artifactId>testng</artifactId>
            <version>${testng.version}</version>
        </dependency>

        <!-- Extent Reports -->
        <dependency>
            <groupId>com.aventstack</groupId>
            <artifactId>extentreports</artifactId>
            <version>5.1.1</version>
        </dependency>
    </dependencies>

    <build>
        <plugins>
            <!-- Maven Compiler Plugin -->
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <version>3.11.0</version>
                <configuration>
                    <source>21</source>
                    <target>21</target>
                </configuration>
            </plugin>

            <!-- Maven Surefire Plugin for running tests -->
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-surefire-plugin</artifactId>
                <version>3.2.2</version>
            </plugin>
        </plugins>
    </build>
</project>
```

### 2.2 Download Dependencies

Run this command to download all dependencies:

```bash
mvn clean compile
```

**What's happening?**
- Maven downloads Selenium WebDriver
- Downloads WebDriverManager
- Downloads TestNG
- Downloads ExtentReports
- Compiles your project

---

## 📁 Step 3: Create Project Structure

### 3.1 Create Package Structure

Create these folders in your `src/main/java`:

```
src/main/java/
└── com/
    └── yourname/
        └── saucedemo/
            ├── pages/
            └── utils/
```

Create these folders in your `src/test/java`:

```
src/test/java/
└── com/
    └── yourname/
        └── saucedemo/
            ├── testclasses/
            └── runners/
```

### 3.2 Create Base Classes

**Create BasePage.java** in `src/main/java/com/yourname/saucedemo/pages/`:

```java
package com.yourname.saucedemo.pages;

import org.openqa.selenium.WebDriver;
import org.openqa.selenium.support.ui.WebDriverWait;
import java.time.Duration;

public class BasePage {
    protected WebDriver driver;
    protected WebDriverWait wait;
    
    public BasePage(WebDriver driver) {
        this.driver = driver;
        this.wait = new WebDriverWait(driver, Duration.ofSeconds(10));
    }
}
```

**Create DriverManager.java** in `src/main/java/com/yourname/saucedemo/utils/`:

```java
package com.yourname.saucedemo.utils;

import org.openqa.selenium.WebDriver;
import org.openqa.selenium.chrome.ChromeDriver;
import io.github.bonigarcia.wdm.WebDriverManager;

public class DriverManager {
    private static WebDriver driver;
    
    public static WebDriver getDriver() {
        if (driver == null) {
            WebDriverManager.chromedriver().setup();
            driver = new ChromeDriver();
            driver.manage().window().maximize();
        }
        return driver;
    }
    
    public static void quitDriver() {
        if (driver != null) {
            driver.quit();
            driver = null;
        }
    }
}
```

---

## 🧪 Step 4: Create Your First Test

### 4.1 Create LoginPage.java

Create `LoginPage.java` in `src/main/java/com/yourname/saucedemo/pages/`:

```java
package com.yourname.saucedemo.pages;

import org.openqa.selenium.By;
import org.openqa.selenium.WebDriver;

public class LoginPage extends BasePage {
    
    // Constructor
    public LoginPage(WebDriver driver) {
        super(driver);
    }
    
    // Locators - how to find elements on the page
    private By usernameField = By.id("user-name");
    private By passwordField = By.id("password");
    private By loginButton = By.id("login-button");
    private By errorMessage = By.xpath("//h3[@data-test='error']");
    
    // Actions - what we can do on this page
    public void enterUsername(String username) {
        driver.findElement(usernameField).sendKeys(username);
    }
    
    public void enterPassword(String password) {
        driver.findElement(passwordField).sendKeys(password);
    }
    
    public void clickLoginButton() {
        driver.findElement(loginButton).click();
    }
    
    public String getErrorMessage() {
        try {
            return driver.findElement(errorMessage).getText();
        } catch (Exception e) {
            return "";
        }
    }
    
    public void login(String username, String password) {
        enterUsername(username);
        enterPassword(password);
        clickLoginButton();
    }
}
```

### 4.2 Create BaseTest.java

Create `BaseTest.java` in `src/test/java/com/yourname/saucedemo/testclasses/`:

```java
package com.yourname.saucedemo.testclasses;

import org.testng.annotations.AfterMethod;
import org.testng.annotations.BeforeMethod;
import com.yourname.saucedemo.utils.DriverManager;
import org.openqa.selenium.WebDriver;

public class BaseTest {
    protected WebDriver driver;
    
    @BeforeMethod
    public void setUp() {
        driver = DriverManager.getDriver();
    }
    
    @AfterMethod
    public void tearDown() {
        DriverManager.quitDriver();
    }
}
```

### 4.3 Create Your First Test

Create `LoginTest.java` in `src/test/java/com/yourname/saucedemo/testclasses/`:

```java
package com.yourname.saucedemo.testclasses;

import org.testng.Assert;
import org.testng.annotations.Test;
import com.yourname.saucedemo.pages.LoginPage;

public class LoginTest extends BaseTest {
    
    @Test
    public void testValidLogin() {
        // Step 1: Navigate to SauceDemo
        driver.get("https://www.saucedemo.com/");
        
        // Step 2: Create LoginPage object
        LoginPage loginPage = new LoginPage(driver);
        
        // Step 3: Perform login
        loginPage.login("standard_user", "secret_sauce");
        
        // Step 4: Verify we're on the products page
        String currentUrl = driver.getCurrentUrl();
        Assert.assertTrue(currentUrl.contains("inventory.html"), 
            "Login failed - not on products page");
        
        System.out.println("✅ Valid login test passed!");
    }
    
    @Test
    public void testInvalidLogin() {
        // Step 1: Navigate to SauceDemo
        driver.get("https://www.saucedemo.com/");
        
        // Step 2: Create LoginPage object
        LoginPage loginPage = new LoginPage(driver);
        
        // Step 3: Try to login with wrong credentials
        loginPage.login("wrong_user", "wrong_password");
        
        // Step 4: Verify error message is displayed
        String errorMessage = loginPage.getErrorMessage();
        Assert.assertTrue(errorMessage.contains("Username and password do not match"), 
            "Error message not displayed for invalid login");
        
        System.out.println("✅ Invalid login test passed!");
    }
    
    @Test
    public void testLockedOutUser() {
        // Step 1: Navigate to SauceDemo
        driver.get("https://www.saucedemo.com/");
        
        // Step 2: Create LoginPage object
        LoginPage loginPage = new LoginPage(driver);
        
        // Step 3: Try to login with locked out user
        loginPage.login("locked_out_user", "secret_sauce");
        
        // Step 4: Verify locked out message
        String errorMessage = loginPage.getErrorMessage();
        Assert.assertTrue(errorMessage.contains("Sorry, this user has been locked out"), 
            "Locked out message not displayed");
        
        System.out.println("✅ Locked out user test passed!");
    }
}
```

---

## 🏃‍♂️ Step 5: Run Your First Test

### 5.1 Compile the Project

```bash
mvn clean compile test-compile
```

### 5.2 Run Tests

**Option 1: Run all tests**
```bash
mvn test
```

**Option 2: Run specific test class**
```bash
mvn test -Dtest=LoginTest
```

**Option 3: Run from IDE**
1. Right-click on `LoginTest.java`
2. Select "Run LoginTest"
3. Watch the magic happen! 🎉

### 5.3 What You Should See

1. **Chrome browser opens automatically**
2. **Navigates to SauceDemo website**
3. **Performs login actions**
4. **Verifies results**
5. **Browser closes**
6. **Test results in console**

**Expected Console Output:**
```
[INFO] Tests run: 3, Failures: 0, Errors: 0, Skipped: 0
✅ Valid login test passed!
✅ Invalid login test passed!
✅ Locked out user test passed!
```

---

## 🐛 Step 6: Debugging Common Issues

### Issue 1: ChromeDriver Not Found

**Error:**
```
Exception: The driver executable does not exist
```

**Solution:**
WebDriverManager should handle this automatically. If not:
```bash
# Clear Maven cache
mvn clean
# Re-run
mvn test
```

### Issue 2: Element Not Found

**Error:**
```
NoSuchElementException: Unable to locate element
```

**Solution:**
1. Check if SauceDemo website is accessible
2. Verify element locators are correct
3. Add wait time:

```java
// Add this to your test
Thread.sleep(2000); // Wait 2 seconds
```

### Issue 3: Tests Pass Locally but Fail in CI

**Solution:**
Add headless mode for CI environments:

```java
// In DriverManager.java
ChromeOptions options = new ChromeOptions();
options.addArguments("--headless");
options.addArguments("--no-sandbox");
options.addArguments("--disable-dev-shm-usage");
driver = new ChromeDriver(options);
```

---

## 📊 Step 7: Add Reporting

### 7.1 Create TestNG XML

Create `testng.xml` in your project root:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE suite SYSTEM "https://testng.org/testng-1.0.dtd">
<suite name="SauceDemo Test Suite">
    <test name="Login Tests">
        <classes>
            <class name="com.yourname.saucedemo.testclasses.LoginTest"/>
        </classes>
    </test>
</suite>
```

### 7.2 Run with TestNG XML

```bash
mvn test -DsuiteXmlFile=testng.xml
```

### 7.3 View Reports

After running tests, check:
- **Console output** for immediate results
- **target/surefire-reports/** for detailed HTML reports

---

## 🎯 Step 8: Advanced Features

### 8.1 Add Screenshots on Failure

Update your `BaseTest.java`:

```java
package com.yourname.saucedemo.testclasses;

import org.testng.annotations.AfterMethod;
import org.testng.annotations.BeforeMethod;
import org.testng.ITestResult;
import org.openqa.selenium.TakesScreenshot;
import org.openqa.selenium.OutputType;
import com.yourname.saucedemo.utils.DriverManager;
import org.openqa.selenium.WebDriver;
import java.io.File;
import java.io.IOException;
import java.nio.file.Files;
import java.nio.file.StandardCopyOption;

public class BaseTest {
    protected WebDriver driver;
    
    @BeforeMethod
    public void setUp() {
        driver = DriverManager.getDriver();
    }
    
    @AfterMethod
    public void tearDown(ITestResult result) {
        if (result.getStatus() == ITestResult.FAILURE) {
            takeScreenshot(result.getName());
        }
        DriverManager.quitDriver();
    }
    
    private void takeScreenshot(String testName) {
        try {
            TakesScreenshot screenshot = (TakesScreenshot) driver;
            File sourceFile = screenshot.getScreenshotAs(OutputType.FILE);
            File destFile = new File("screenshots/" + testName + "_" + System.currentTimeMillis() + ".png");
            Files.copy(sourceFile.toPath(), destFile.toPath(), StandardCopyOption.REPLACE_EXISTING);
            System.out.println("Screenshot saved: " + destFile.getAbsolutePath());
        } catch (IOException e) {
            System.out.println("Failed to take screenshot: " + e.getMessage());
        }
    }
}
```

### 8.2 Add Data-Driven Testing

Create `LoginDataProvider.java`:

```java
package com.yourname.saucedemo.testclasses;

import org.testng.annotations.DataProvider;

public class LoginDataProvider {
    
    @DataProvider(name = "loginData")
    public Object[][] getLoginData() {
        return new Object[][] {
            {"standard_user", "secret_sauce", true, "Valid user login"},
            {"locked_out_user", "secret_sauce", false, "Locked out user"},
            {"problem_user", "secret_sauce", true, "Problem user login"},
            {"performance_glitch_user", "secret_sauce", true, "Performance glitch user"},
            {"invalid_user", "wrong_password", false, "Invalid credentials"}
        };
    }
}
```

Update your `LoginTest.java`:

```java
@Test(dataProvider = "loginData", dataProviderClass = LoginDataProvider.class)
public void testLoginWithData(String username, String password, boolean shouldPass, String description) {
    System.out.println("Testing: " + description);
    
    driver.get("https://www.saucedemo.com/");
    LoginPage loginPage = new LoginPage(driver);
    loginPage.login(username, password);
    
    if (shouldPass) {
        String currentUrl = driver.getCurrentUrl();
        Assert.assertTrue(currentUrl.contains("inventory.html"), 
            "Login should have succeeded for: " + description);
    } else {
        String errorMessage = loginPage.getErrorMessage();
        Assert.assertFalse(errorMessage.isEmpty(), 
            "Error message should be displayed for: " + description);
    }
}
```

---

## 🎉 Congratulations!

You've successfully created and run your first test automation project! 

### What You've Accomplished:

✅ **Set up a complete test automation environment**
✅ **Created a Page Object Model structure**
✅ **Written your first automated tests**
✅ **Implemented proper test organization**
✅ **Added error handling and debugging**
✅ **Generated test reports**

### Your Project Now Includes:

- **3 different login test scenarios**
- **Proper page object structure**
- **Automatic driver management**
- **Screenshot capture on failures**
- **Data-driven testing capability**
- **TestNG reporting**

---

## 🚀 Next Steps

### Immediate Next Steps:
1. **Add more test scenarios** (product page, cart, checkout)
2. **Implement more page objects**
3. **Add better error handling**
4. **Create more comprehensive reports**

### Advanced Next Steps:
1. **Add Cucumber BDD framework**
2. **Implement CI/CD with Jenkins**
3. **Add cross-browser testing**
4. **Integrate with cloud testing platforms**

### Practice Exercises:
1. **Create a ProductPage class** and test product selection
2. **Add cart functionality tests**
3. **Implement checkout flow tests**
4. **Add API testing integration**

---

## 💡 Tips for Success

1. **Practice Regularly**: Run tests daily to get familiar
2. **Read Error Messages**: They tell you exactly what's wrong
3. **Use Browser Developer Tools**: Inspect elements to find locators
4. **Keep Tests Simple**: One test should test one thing
5. **Use Descriptive Names**: Make test names self-explanatory
6. **Handle Waits Properly**: Don't use hardcoded delays
7. **Version Control**: Use Git to track your changes

---

## 🆘 Getting Help

### When You're Stuck:

1. **Check the console output** - it usually tells you what's wrong
2. **Look at the browser** - see what's actually happening
3. **Check element locators** - use browser dev tools
4. **Search online** - Stack Overflow has answers to most questions
5. **Ask in communities** - Reddit, Discord, forums

### Useful Resources:

- **Selenium Documentation**: https://selenium.dev/
- **TestNG Documentation**: https://testng.org/doc/
- **SauceDemo Website**: https://www.saucedemo.com/
- **WebDriverManager**: https://github.com/bonigarcia/webdrivermanager

---

**Happy Testing! 🎉**

Remember: Every expert was once a beginner. Keep practicing, keep learning, and don't be afraid to make mistakes - that's how you learn!

