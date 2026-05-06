# Build Instructions

## Prerequisites Verification

Before building, ensure you have the following installed:

### Check Java Installation
```bash
java -version
```
**Required:** Java 11 or higher

### Check Maven Installation
```bash
mvn --version
```
**Required:** Maven 3.6 or higher

If not installed, follow the installation guides at:
- Java: https://www.oracle.com/java/technologies/downloads/
- Maven: https://maven.apache.org/download.cgi

---

## Step-by-Step Build Process

### Step 1: Navigate to Project Directory
```bash
cd /Users/sairam/Desktop/cc
```

### Step 2: Clean Previous Builds (Optional)
```bash
mvn clean
```
This removes any previous build artifacts.

### Step 3: Build the Project
```bash
mvn clean package
```

This command will:
1. **Clean** - Remove old build artifacts
2. **Validate** - Check project is correct
3. **Compile** - Compile Java source code
4. **Test** - Run unit tests (if any)
5. **Package** - Create the WAR file

### Step 4: Verify Build Success

You should see output like:
```
[INFO] --- maven-war-plugin:3.3.2:war (default-war) @ login-app ---
[INFO] Packaging webapp
[INFO] Building war: /Users/sairam/Desktop/cc/target/login-app.war
[INFO] BUILD SUCCESS
```

### Step 5: Locate WAR File
```bash
ls -lh target/login-app.war
```

The WAR file should be in: `/Users/sairam/Desktop/cc/target/login-app.war`

---

## Building Without Tests (Faster)

If you want to skip running tests during the build:

```bash
mvn clean package -DskipTests
```

---

## Build Options

### Option 1: Development Build (with embedded Tomcat)
```bash
mvn tomcat7:run
```
- Starts Tomcat on http://localhost:8080/login-app
- Press Ctrl+C to stop
- Perfect for testing and development

### Option 2: Production WAR File
```bash
mvn clean package
```
- Creates WAR file in `target/` directory
- Deploy to production Tomcat server
- Optimized for deployment

### Option 3: Verbose Output (Debugging)
```bash
mvn clean package -X
```
- Shows detailed debug information
- Useful for troubleshooting build issues

---

## Build Output Structure

After successful build, you'll have:

```
target/
├── login-app/                    # Unpackaged web application directory
│   ├── WEB-INF/
│   │   ├── classes/             # Compiled Java classes
│   │   │   └── com/example/servlet/LoginServlet.class
│   │   ├── lib/                 # Application libraries
│   │   └── web.xml
│   ├── login.jsp
│   ├── success.jsp
│   ├── logout.jsp
│   └── index.html
├── login-app.war                # **WAR file (ready for deployment)**
├── maven-status/
├── maven-archiver/
└── ...
```

**The important file:** `target/login-app.war` ← This is what you deploy to Tomcat

---

## Deployment After Build

### For Local Testing with Tomcat Plugin
```bash
mvn tomcat7:run
```

### For Production Deployment
1. Copy `target/login-app.war` to your Tomcat's `webapps/` directory
2. Restart Tomcat
3. Access at `http://your-server:8080/login-app`

---

## Troubleshooting Build Issues

### Issue: "Maven is not recognized"
```
'mvn' is not recognized as an internal or external command
```
**Solution:** 
- Install Maven properly
- Ensure Maven's `bin` directory is in PATH
- Restart terminal after installation

### Issue: "Java is not recognized"
```
'java' is not recognized as an internal or external command
```
**Solution:**
- Install Java (JDK, not JRE)
- Ensure JAVA_HOME is set in environment variables
- Restart terminal after installation

### Issue: Build fails with "BUILD FAILURE"
```
[ERROR] COMPILATION ERROR
```
**Solution:**
- Check Java version: `java -version` (must be 11+)
- Check for syntax errors in Java files
- Run with verbose output: `mvn clean package -X`

### Issue: "No artifacts to deploy"
**Solution:**
- Check `pom.xml` has `<packaging>war</packaging>`
- Ensure build completed without errors
- Check target directory exists

### Issue: Out of Memory during build
```
java.lang.OutOfMemoryError
```
**Solution:**
```bash
export MAVEN_OPTS=-Xmx1024m
mvn clean package
```

---

## Successful Build Checklist

- [ ] Java 11+ installed and in PATH
- [ ] Maven 3.6+ installed and in PATH
- [ ] No syntax errors in source files
- [ ] `pom.xml` is valid XML
- [ ] Build completes with "BUILD SUCCESS"
- [ ] `target/login-app.war` file exists
- [ ] WAR file is > 1 MB in size

---

## Next: Deployment

Once build is successful, proceed to deployment:
1. Copy `login-app.war` to Tomcat `webapps/`
2. Restart Tomcat
3. Visit `http://localhost:8080/login-app`
4. Login with username: `admin` and password: `password123`

For detailed deployment instructions, see README.md
