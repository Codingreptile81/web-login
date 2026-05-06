# Login Web Application

A simple web application with a login page built using Java, JSP, and Maven. The application is designed to run on Apache Tomcat and produces a WAR (Web ARchive) file for deployment.

## Project Structure

```
login-app/
├── pom.xml                           # Maven configuration file
├── src/
│   └── main/
│       ├── java/
│       │   └── com/example/servlet/
│       │       └── LoginServlet.java  # Login handler servlet
│       └── webapp/
│           ├── login.jsp              # Login page
│           ├── success.jsp            # Dashboard/success page
│           ├── logout.jsp             # Logout handler
│           └── WEB-INF/
│               └── web.xml            # Deployment descriptor
└── target/
    └── login-app.war                  # Generated WAR file
```

## Prerequisites

- **Java Development Kit (JDK)** 11 or higher
- **Maven** 3.6 or higher
- **Apache Tomcat** 9 or higher (for deployment)

## Building the Application

### 1. Build the WAR file

Navigate to the project directory and run:

```bash
mvn clean package
```

This will:
- Clean the project
- Compile the Java code
- Create the `target/login-app.war` file

### 2. The generated WAR file location

```
target/login-app.war
```

## Deploying to Tomcat

### Option 1: Using Tomcat Manager (GUI)

1. Start Tomcat server
2. Open Tomcat Manager at: `http://localhost:8080/manager/html`
3. Under "Deploy" section, choose the `login-app.war` file
4. Click "Deploy"

### Option 2: Manual Deployment

1. Copy `target/login-app.war` to Tomcat's `webapps` directory:
   ```bash
   cp target/login-app.war $CATALINA_HOME/webapps/
   ```

2. Start/Restart Tomcat:
   ```bash
   $CATALINA_HOME/bin/startup.sh   # On Linux/Mac
   $CATALINA_HOME\bin\startup.bat  # On Windows
   ```

3. Tomcat will automatically extract and deploy the WAR file

### Option 3: Using Maven Tomcat Plugin (for testing)

```bash
mvn tomcat7:run
```

This starts a Tomcat server on `http://localhost:8080/login-app`

## Accessing the Application

Once deployed, access the application at:

```
http://localhost:8080/login-app
```

## Demo Credentials

- **Username:** `admin`
- **Password:** `password123`

## Features

✅ **Responsive Login Page** - Clean, modern UI that works on all devices  
✅ **Session Management** - Secure session handling with cookies  
✅ **Login Validation** - Server-side credential validation  
✅ **Dashboard** - Welcome page after successful login  
✅ **Logout Functionality** - Secure session termination  

## Project Files Description

### pom.xml
Maven configuration file that:
- Defines project metadata
- Specifies dependencies (Servlet API, JSP API)
- Configures build plugins (Compiler, WAR, Tomcat7)
- Sets Java version to 11

### LoginServlet.java
Handles login requests with:
- POST request handling for form submission
- Credential validation
- Session creation on success
- Error handling and redirection

### login.jsp
The login page featuring:
- Username and password input fields
- Error message display
- CSS styling for responsive design
- Demo credentials display

### success.jsp
Dashboard page that:
- Displays after successful login
- Shows logged-in username
- Includes logout functionality
- Validates user session

### web.xml
Web application deployment descriptor with:
- Application display name
- Session configuration
- Welcome file settings
- Security settings (HttpOnly cookies)

## Security Notes

⚠️ **This is a demo application!** For production use:

1. **Never hardcode credentials** - Use a proper authentication system
2. **Use HTTPS** - Always encrypt data in transit
3. **Hash passwords** - Use bcrypt or similar algorithms
4. **Input validation** - Validate all user inputs
5. **CSRF protection** - Implement token-based CSRF protection
6. **Parameterized queries** - Prevent SQL injection if using a database

## Troubleshooting

**Problem:** WAR file not being created
- **Solution:** Ensure `pom.xml` has `<packaging>war</packaging>`

**Problem:** 404 error when accessing the app
- **Solution:** Ensure the app is deployed with correct context path (`/login-app`)

**Problem:** JSP pages showing as source code
- **Solution:** Ensure Tomcat is properly installed with JSP support

**Problem:** Login not working
- **Solution:** Check browser console for errors, verify servlet mapping in `web.xml`

## Building Without Tests

To skip tests during build:

```bash
mvn clean package -DskipTests
```

## Viewing Build Output

The build process will output information like:

```
[INFO] --- maven-war-plugin:3.3.2:war (default-war) @ login-app ---
[INFO] Packaging webapp
[INFO] Assembling webapp [login-app] in [/path/to/target/login-app]
[INFO] Processing war project
[INFO] Building war: /path/to/target/login-app.war
[INFO] BUILD SUCCESS
```

## Additional Resources

- [Apache Maven Documentation](https://maven.apache.org/documentation.html)
- [Apache Tomcat Documentation](https://tomcat.apache.org/documentation.html)
- [Java Servlet Documentation](https://docs.oracle.com/javaee/7/tutorial/index.html)
- [JSP Documentation](https://www.oracle.com/java/technologies/pageorientation.html)

## License

This project is provided as-is for educational purposes.
