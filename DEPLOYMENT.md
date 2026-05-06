# Deployment Guide

Complete guide for deploying the Login Web Application to Apache Tomcat.

---

## Prerequisites

- ✅ Built WAR file: `target/login-app.war`
- ✅ Apache Tomcat 9+ installed
- ✅ Java 11+ installed on the server
- ✅ Network access to the server (if remote)

---

## Deployment Options

### Option 1: Manual Deployment (Recommended for Production)

#### Step 1: Stop Tomcat
```bash
# Linux/Mac
$CATALINA_HOME/bin/shutdown.sh

# Windows
%CATALINA_HOME%\bin\shutdown.bat
```

#### Step 2: Copy WAR File
```bash
# Linux/Mac
cp /Users/sairam/Desktop/cc/target/login-app.war $CATALINA_HOME/webapps/

# Windows
copy C:\path\to\cc\target\login-app.war %CATALINA_HOME%\webapps\
```

**Note:** Find CATALINA_HOME (Tomcat installation directory):
- **Linux:** `/opt/tomcat` or `/usr/local/tomcat`
- **Mac:** `/Library/Tomcat` or wherever you installed it
- **Windows:** `C:\Program Files\Apache Software Foundation\Tomcat 9.0`

#### Step 3: Start Tomcat
```bash
# Linux/Mac
$CATALINA_HOME/bin/startup.sh

# Windows
%CATALINA_HOME%\bin\startup.bat
```

Tomcat will automatically extract the WAR file.

#### Step 4: Verify Deployment
```bash
ls -la $CATALINA_HOME/webapps/login-app
```

You should see the extracted application directory.

#### Step 5: Access the Application
Open your browser and navigate to:
```
http://localhost:8080/login-app
```

---

### Option 2: Tomcat Manager GUI (Easy - Requires Manager App)

#### Step 1: Start Tomcat
Ensure Tomcat is running.

#### Step 2: Access Tomcat Manager
Open in browser:
```
http://localhost:8080/manager/html
```

You may be prompted for credentials:
- **Username:** (default) `tomcat`
- **Password:** (default) `tomcat`

*Note: Change these default credentials in production!*

#### Step 3: Deploy Application
1. Scroll to "Deploy" section
2. Under "Select WAR file to upload"
3. Click "Choose File" and select: `/Users/sairam/Desktop/cc/target/login-app.war`
4. Click "Deploy"

#### Step 4: Verify Deployment
- Application appears in the applications list with status "running"
- Click on `/login-app` to access the application

---

### Option 3: Maven Tomcat Plugin (Development Only)

#### Step 1: Run from Project Directory
```bash
cd /Users/sairam/Desktop/cc
mvn tomcat7:run
```

#### Step 2: Access Application
Open in browser:
```
http://localhost:8080/login-app
```

#### Step 3: Stop Application
Press `Ctrl+C` in the terminal

**Note:** This is ideal for development/testing, not production.

---

## Deployment Verification

### Check Application is Running
```bash
curl http://localhost:8080/login-app
```

### Check Tomcat Logs
```bash
# Linux/Mac
tail -f $CATALINA_HOME/logs/catalina.out

# Check for errors
grep -i error $CATALINA_HOME/logs/catalina.out
```

### Access Login Page
1. Open browser: `http://localhost:8080/login-app`
2. You should see the login page
3. Login with:
   - **Username:** `admin`
   - **Password:** `password123`
4. You should be redirected to the dashboard

---

## Post-Deployment Configuration

### 1. Change Default Credentials (Important!)

Edit: `src/main/java/com/example/servlet/LoginServlet.java`

Replace lines 16-17:
```java
private static final String VALID_USERNAME = "admin";
private static final String VALID_PASSWORD = "password123";
```

With your credentials, then rebuild and redeploy.

### 2. Enable HTTPS (Production)

1. Generate SSL certificate
2. Configure in `$CATALINA_HOME/conf/server.xml`
3. Update application URL to use `https://`

### 3. Change Tomcat Manager Credentials

Edit: `$CATALINA_HOME/conf/tomcat-users.xml`

```xml
<user username="admin" password="secure_password" roles="manager-gui,manager-script" />
```

---

## Undeploying the Application

### Option 1: Using Tomcat Manager
1. Go to Tomcat Manager
2. Find the application in the list
3. Click "Undeploy"

### Option 2: Manual
```bash
# Stop Tomcat
$CATALINA_HOME/bin/shutdown.sh

# Remove the WAR file and extracted directory
rm -rf $CATALINA_HOME/webapps/login-app
rm $CATALINA_HOME/webapps/login-app.war

# Start Tomcat
$CATALINA_HOME/bin/startup.sh
```

---

## Troubleshooting

### Issue: "Application not accessible"
```
HTTP Error 404: The requested resource is not available
```
**Solutions:**
- Check Tomcat is running: `jps | grep Tomcat`
- Verify WAR file deployed: `ls $CATALINA_HOME/webapps/login-app`
- Check application context path is `/login-app`
- Wait 10-15 seconds for deployment to complete

### Issue: "Port 8080 already in use"
```
Address already in use
```
**Solutions:**
```bash
# Find what's using port 8080
lsof -i :8080      # Linux/Mac
netstat -ano | findstr :8080  # Windows

# Kill the process (adjust PID accordingly)
kill -9 <PID>      # Linux/Mac
taskkill /PID <PID> /F  # Windows

# Or change Tomcat port in server.xml
```

### Issue: "Login not working - credentials rejected"
**Solutions:**
- Check browser console for errors (F12)
- Check Tomcat logs: `tail -f $CATALINA_HOME/logs/catalina.out`
- Verify default credentials: admin / password123
- Check Java process is running: `jps`

### Issue: "JSP pages showing as source code"
**Solution:**
- Tomcat not properly configured for JSP
- Reinstall Tomcat from official source
- Ensure Tomcat has JSP support (usually included by default)

### Issue: "404 error on JSP pages"
**Solutions:**
- Check correct context path: `http://localhost:8080/login-app`
- Not `http://localhost:8080/login-app/login.jsp`
- Verify `web.xml` is correctly configured
- Check `WEB-INF` directory exists in deployed app

---

## Monitoring After Deployment

### View Real-time Logs
```bash
tail -f $CATALINA_HOME/logs/catalina.out
```

### View Error Logs
```bash
tail -f $CATALINA_HOME/logs/localhost_*.log
```

### Monitor Memory Usage
```bash
# Linux/Mac
top | grep java

# Windows
tasklist | findstr java
```

### Check Application Statistics
- Visit Tomcat Manager for application status
- View active sessions, memory usage, etc.

---

## Production Deployment Checklist

- [ ] WAR file successfully built
- [ ] Tomcat installed and running
- [ ] WAR file copied to webapps directory
- [ ] Application accessible at http://localhost:8080/login-app
- [ ] Login functionality working
- [ ] Default credentials changed
- [ ] HTTPS/SSL configured
- [ ] Tomcat Manager credentials changed
- [ ] Firewall rules configured
- [ ] Monitoring set up
- [ ] Backup strategy in place
- [ ] Documentation updated

---

## Rollback Procedure

If issues occur after deployment:

1. Stop Tomcat: `$CATALINA_HOME/bin/shutdown.sh`
2. Remove failed deployment: `rm -rf $CATALINA_HOME/webapps/login-app*`
3. Deploy previous version
4. Start Tomcat: `$CATALINA_HOME/bin/startup.sh`

---

## Performance Tips

1. **Increase heap size** for better performance:
   ```bash
   export CATALINA_OPTS="-Xms512m -Xmx1024m"
   ```

2. **Enable compression** in `server.xml`:
   ```xml
   <Connector ... compression="on" />
   ```

3. **Optimize database connections** (when adding database)

4. **Use load balancing** for multiple servers

---

## Support

For issues or questions:
1. Check logs in `$CATALINA_HOME/logs/`
2. Review BUILD.md for build-related issues
3. Check README.md for general information
4. Consult Tomcat documentation: https://tomcat.apache.org/
