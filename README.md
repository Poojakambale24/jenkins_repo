# Portfolio Jenkins Setup

## Quick Start Commands

### 1. Clone Repository
```bash
git clone https://github.com/Jadhav-Prathamesh-01/portfolio.git
cd portfolio
```

### 2. Install and Start Jenkins

#### macOS:
```bash
# Install Jenkins
brew install jenkins-lts

# Start Jenkins
brew services start jenkins-lts

# Get initial admin password
cat ~/.jenkins/secrets/initialAdminPassword
```

#### Windows:
```cmd
# Download Jenkins from https://www.jenkins.io/download/
# Install Jenkins.msi
# Jenkins starts automatically after installation

# Get initial admin password
type C:\ProgramData\Jenkins\.jenkins\secrets\initialAdminPassword
```

#### If Jenkins is Already Installed:

**macOS:**
```bash
# Start Jenkins
brew services start jenkins-lts

# Check if running
brew services list | grep jenkins
```

**Windows:**
```cmd
# Start Jenkins service
net start Jenkins

# Or use Services.msc GUI to start Jenkins service
# Open Services → Find Jenkins → Start
```

**Linux:**
```bash
# Start Jenkins
sudo systemctl start jenkins

# Enable auto-start
sudo systemctl enable jenkins
```

### 3. Access Jenkins
```bash
# Open Jenkins in browser (all platforms)
http://localhost:8080
```

### 4. Create Jenkins Job (One-time setup)
```
1. Open http://localhost:8080
2. Click "New Item"
3. Enter name: "portfolio-website"
4. Select "Pipeline"
5. In Pipeline section:
   - Definition: "Pipeline script"
   - Copy the Jenkinsfile content from repository
   - Check "Use Groovy Sandbox"
6. Click "Save"
```

### 5. Push Changes to Trigger Build
```bash
# Add changes
git add .

# Commit changes
git commit -m "your commit message"

# Push to GitHub (triggers Jenkins build manually)
git push origin main

# Then in Jenkins: Click "Build Now"
```

### 6. Access Deployed Website

**macOS/Linux:**
```bash
# Website runs on
http://localhost:3000
```

**Windows:**
```cmd
# Website runs on
http://localhost:3000
# Or check Jenkins console output for deployment path
```

### Stop Services

**macOS:**
```bash
# Stop Jenkins
brew services stop jenkins-lts

# Stop local server
pkill -f "python3 -m http.server 3000"
```

**Windows:**
```cmd
# Stop Jenkins service
net stop Jenkins

# Stop local server (if running)
# Find and kill Python HTTP server process in Task Manager
```

**Linux:**
```bash
# Stop Jenkins
sudo systemctl stop jenkins

# Stop local server
pkill -f "python3 -m http.server 3000"
```

## Troubleshooting

### Jenkins Won't Start
```bash
# Check if port 8080 is in use
netstat -an | grep 8080

# Change Jenkins port (if needed)
# Edit Jenkins config file or use --httpPort=8081
```

### Can't Access Jenkins
```bash
# Check Jenkins is running
http://localhost:8080

# Check firewall settings
# Ensure port 8080 is not blocked
```