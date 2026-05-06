# Quick Start Guide

## ⚡ 5-Minute Setup

### 1. Build the Application

```bash
cd /Users/sairam/Desktop/cc
mvn clean package
```

**Output:** `target/login-app.war` ✓

### 2. Deploy to Tomcat

#### Using Maven (Easy - for testing):
```bash
mvn tomcat7:run
```
Then open: http://localhost:8080/login-app

#### Using Tomcat Manager (Production):
1. Copy `target/login-app.war` to `$CATALINA_HOME/webapps/`
2. Restart Tomcat
3. Open: http://localhost:8080/login-app

### 3. Login

- **Username:** `admin`
- **Password:** `password123`

---

## 📋 Common Commands

```bash
# Build WAR file
mvn clean package

# Build without running tests
mvn clean package -DskipTests

# Run with embedded Tomcat (for development)
mvn tomcat7:run

# Clean build artifacts
mvn clean

# View project dependencies
mvn dependency:tree
```

---

## 📁 Important Files

| File | Purpose |
|------|---------|
| `pom.xml` | Maven configuration, dependencies, build settings |
| `src/main/java/com/example/servlet/LoginServlet.java` | Login request handler |
| `src/main/webapp/login.jsp` | Login form page |
| `src/main/webapp/success.jsp` | Dashboard after login |
| `src/main/webapp/WEB-INF/web.xml` | Web app configuration |

---

## 🐛 Troubleshooting

| Issue | Solution |
|-------|----------|
| WAR file not created | Check Java version (11+) and Maven version (3.6+) |
| 404 error accessing app | Ensure context path is `/login-app` |
| Login button not working | Check browser console, ensure Tomcat is running |
| JSP shows source code | Tomcat not configured for JSP |

---

## 📚 Next Steps

1. **Customize credentials:** Edit `LoginServlet.java` (lines 16-17)
2. **Change styling:** Edit CSS in `login.jsp` and `success.jsp`
3. **Add database:** Integrate JDBC for persistent user storage
4. **Add security:** Implement SSL/HTTPS and password hashing

---

## ✅ Verification Checklist

- [ ] Java 11+ installed: `java -version`
- [ ] Maven installed: `mvn --version`
- [ ] Tomcat installed (for deployment)
- [ ] WAR file generated in `target/` directory
- [ ] Application accessible at http://localhost:8080/login-app
- [ ] Can login with demo credentials
- [ ] Can logout successfully

---

## 📞 Support

Refer to `README.md` for detailed documentation and security notes.
