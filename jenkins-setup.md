# Jenkins Setup Guide for SauceDemo Test Automation

This guide will help you set up Jenkins for your SauceDemo test automation project.

## 📋 Prerequisites

### Jenkins Installation
1. **Install Jenkins** (if not already installed)
   - Download from: https://www.jenkins.io/download/
   - Or use Docker: `docker run -p 8080:8080 jenkins/jenkins:lts`

### Required Jenkins Plugins
Install the following plugins in Jenkins:

1. **Essential Plugins:**
   - Pipeline
   - Git
   - Maven Integration
   - TestNG Results Plugin
   - Cucumber Reports Plugin
   - HTML Publisher Plugin
   - Email Extension Plugin
   - AnsiColor Plugin
   - Timestamper Plugin
   - Build Timeout Plugin

2. **Installation Steps:**
   ```
   Manage Jenkins → Manage Plugins → Available
   Search and install each plugin listed above
   ```

## 🔧 Jenkins Configuration

### 1. Global Tool Configuration

Navigate to: `Manage Jenkins → Global Tool Configuration`

#### JDK Configuration
- **Name**: `JDK-21`
- **JAVA_HOME**: `/path/to/java21` (or select "Install automatically")

#### Maven Configuration
- **Name**: `Maven-3.9.6`
- **MAVEN_HOME**: `/path/to/maven` (or select "Install automatically")

### 2. System Configuration

Navigate to: `Manage Jenkins → Configure System`

#### Email Configuration (Optional)
- **SMTP Server**: `smtp.gmail.com`
- **Default user e-mail suffix**: `@yourdomain.com`
- **Use SMTP Authentication**: Check
- **User Name**: `your-email@gmail.com`
- **Password**: `your-app-password`

## 🚀 Pipeline Setup

### 1. Create New Pipeline Job

1. **Create New Item**
   - Click "New Item" on Jenkins dashboard
   - Enter job name: `SauceDemo-Test-Automation`
   - Select "Pipeline"
   - Click "OK"

### 2. Configure Pipeline

#### General Settings
- **Description**: `Automated testing pipeline for SauceDemo application`
- **GitHub Project**: `https://github.com/yourusername/Capstone-project`

#### Build Triggers
Choose one or more:
- **GitHub hook trigger for GITScm polling** (for webhook triggers)
- **Poll SCM** with schedule: `H/5 * * * *` (every 5 minutes)
- **Build periodically** with schedule: `H 2 * * *` (daily at 2 AM)

#### Pipeline Configuration
- **Definition**: Pipeline script from SCM
- **SCM**: Git
- **Repository URL**: `https://github.com/yourusername/Capstone-project.git`
- **Credentials**: Add your GitHub credentials if private repo
- **Branch Specifier**: `*/main` or `*/master`
- **Script Path**: `Jenkinsfile`

### 3. Advanced Pipeline Options

#### Environment Variables
Add these in the pipeline configuration:
```
BROWSER=chrome
HEADLESS=false
TEST_ENV=staging
```

#### Build Parameters (Optional)
Add parameters for flexible testing:
- **BROWSER** (Choice): chrome, firefox, edge
- **HEADLESS** (Boolean): true, false
- **TEST_ENV** (Choice): staging, production
- **TEST_SUITE** (Choice): all, smoke, regression

## 🔍 Pipeline Features

### Automated Stages
1. **Checkout**: Git repository checkout
2. **Environment Setup**: Java/Maven version display
3. **Dependency Resolution**: Maven dependency download
4. **Code Quality**: Compilation and static analysis
5. **Test Execution**: Parallel TestNG and Cucumber tests
6. **Report Generation**: HTML and JSON reports
7. **Artifact Collection**: Archive test results and screenshots

### Reporting Integration
- **TestNG Reports**: Automatic test result parsing
- **Cucumber Reports**: BDD test visualization
- **ExtentReports**: Rich HTML reports
- **Screenshots**: Failure screenshots archiving

### Notification System
- **Email Notifications**: Success, failure, and unstable builds
- **Build Status**: Clear status indicators
- **Artifact Links**: Direct access to reports and screenshots

## 🛠️ Troubleshooting

### Common Issues

1. **Java Version Mismatch**
   ```
   Error: Unsupported major.minor version
   Solution: Ensure Jenkins uses Java 21
   ```

2. **Maven Build Failures**
   ```
   Error: Could not resolve dependencies
   Solution: Check internet connectivity and Maven settings
   ```

3. **Chrome Driver Issues**
   ```
   Error: ChromeDriver not found
   Solution: WebDriverManager handles this automatically
   ```

4. **Test Execution Failures**
   ```
   Error: Tests failing in Jenkins but passing locally
   Solution: Check browser installation and display settings
   ```

### Jenkins Logs
- **Console Output**: View real-time build logs
- **System Log**: `Manage Jenkins → System Log`
- **Build Logs**: Available in each build's console output

## 📊 Monitoring and Maintenance

### Build History
- **Keep Last 10 Builds**: Configured in pipeline options
- **Build Discarding**: Automatic cleanup of old builds

### Performance Monitoring
- **Build Duration**: Tracked and displayed
- **Test Execution Time**: Monitored for performance regression
- **Resource Usage**: Monitor Jenkins server resources

### Regular Maintenance
- **Plugin Updates**: Keep Jenkins plugins updated
- **Java Updates**: Maintain Java 21 compatibility
- **Maven Updates**: Keep Maven version current
- **Chrome Updates**: Ensure browser compatibility

## 🔐 Security Considerations

### Credentials Management
- **GitHub Credentials**: Store securely in Jenkins
- **Email Credentials**: Use app passwords for Gmail
- **Environment Variables**: Avoid hardcoding sensitive data

### Access Control
- **User Permissions**: Configure appropriate access levels
- **Build Permissions**: Restrict who can trigger builds
- **Artifact Access**: Control report and artifact visibility

## 📈 Advanced Features

### Parallel Execution
- **TestNG and Cucumber**: Run simultaneously for faster execution
- **Multi-browser Testing**: Configure for cross-browser validation

### Integration Options
- **Slack Notifications**: Add Slack webhook for team notifications
- **JIRA Integration**: Link test results to JIRA tickets
- **SonarQube**: Integrate code quality analysis

### Scaling
- **Jenkins Agents**: Add multiple build agents for parallel execution
- **Docker Integration**: Use Docker containers for isolated test environments
- **Cloud Integration**: Deploy to AWS, Azure, or GCP for scalability

---

## 🎯 Quick Start Checklist

- [ ] Install Jenkins
- [ ] Install required plugins
- [ ] Configure JDK 21 and Maven 3.9.6
- [ ] Set up email notifications (optional)
- [ ] Create pipeline job
- [ ] Configure Git repository
- [ ] Set Jenkinsfile path
- [ ] Run first build
- [ ] Verify test reports
- [ ] Configure build triggers

Your Jenkins pipeline is now ready to automate your SauceDemo test execution! 🚀

