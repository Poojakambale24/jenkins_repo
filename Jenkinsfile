pipeline {
    agent any
    
    environment {
        NODE_VERSION = '18'
        PROJECT_NAME = 'portfolio-website'
    }
    
    stages {
        stage('Checkout') {
            steps {
                echo 'Checking out source code...'
                git branch: 'main', url: 'https://github.com/Jadhav-Prathamesh-01/portfolio.git'
                echo 'Source code checked out successfully!'
            }
        }
        
        stage('Setup Environment') {
            steps {
                script {
                    echo 'Setting up build environment...'
                    // Install dependencies if needed (like a simple HTTP server)
                    sh '''
                        # Check if Node.js is available
                        if command -v node &> /dev/null; then
                            echo "Node.js version: $(node --version)"
                            echo "npm version: $(npm --version)"
                        else
                            echo "Node.js not found, will use Python's built-in server"
                        fi
                    '''
                }
            }
        }
        
        stage('Validate HTML/CSS/JS') {
            steps {
                script {
                    echo 'Validating static files...'
                    sh '''
                        # Check if HTML files exist
                        if [ -f "index.html" ]; then
                            echo "✅ index.html found"
                        else
                            echo "❌ index.html not found"
                            exit 1
                        fi
                        
                        # Check if CSS files exist
                        if [ -f "styles.css" ]; then
                            echo "✅ styles.css found"
                        else
                            echo "❌ styles.css not found"
                            exit 1
                        fi
                        
                        # Check if JS files exist
                        if [ -f "script.js" ]; then
                            echo "✅ script.js found"
                        else
                            echo "❌ script.js not found"
                            exit 1
                        fi
                        
                        # Basic HTML syntax check
                        grep -q "<!DOCTYPE html>" index.html && echo "✅ DOCTYPE declaration found" || echo "⚠️  DOCTYPE declaration missing"
                        grep -q "<title>" index.html && echo "✅ Title tag found" || echo "⚠️  Title tag missing"
                        grep -q "</html>" index.html && echo "✅ HTML closing tag found" || echo "⚠️  HTML closing tag missing"
                    '''
                }
            }
        }
        
        stage('Build') {
            steps {
                script {
                    echo 'Building static website...'
                    sh '''
                        # Create a build directory
                        mkdir -p dist
                        
                        # Copy static files to build directory
                        cp index.html dist/
                        cp styles.css dist/
                        cp script.js dist/
                        
                        # Create a simple build info file
                        echo "Build Date: $(date)" > dist/build-info.txt
                        echo "Build Number: ${BUILD_NUMBER}" >> dist/build-info.txt
                        echo "Git Commit: $(git rev-parse HEAD)" >> dist/build-info.txt
                        
                        echo "✅ Build completed successfully"
                        ls -la dist/
                    '''
                }
            }
        }
        
        stage('Test') {
            steps {
                script {
                    echo 'Running basic tests...'
                    sh '''
                        # Test if all required files are in the dist directory
                        cd dist
                        
                        # Check file sizes (make sure they're not empty)
                        if [ -s "index.html" ]; then
                            echo "✅ index.html is not empty ($(wc -c < index.html) bytes)"
                        else
                            echo "❌ index.html is empty"
                            exit 1
                        fi
                        
                        if [ -s "styles.css" ]; then
                            echo "✅ styles.css is not empty ($(wc -c < styles.css) bytes)"
                        else
                            echo "❌ styles.css is empty"
                            exit 1
                        fi
                        
                        if [ -s "script.js" ]; then
                            echo "✅ script.js is not empty ($(wc -c < script.js) bytes)"
                        else
                            echo "❌ script.js is empty"
                            exit 1
                        fi
                        
                        echo "✅ All tests passed"
                    '''
                }
            }
        }
        
        stage('Deploy Locally') {
            steps {
                script {
                    echo 'Deploying website locally...'
                    sh '''
                        # Create a deployment directory
                        DEPLOY_DIR="$HOME/portfolio-deployment"
                        mkdir -p "$DEPLOY_DIR"
                        
                        # Copy built files to deployment directory
                        cp -r dist/* "$DEPLOY_DIR/"
                        
                        echo "✅ Website deployed to: $DEPLOY_DIR"
                        echo "📁 Deployment contents:"
                        ls -la "$DEPLOY_DIR"
                        
                        # Start a simple HTTP server in the background (if Python is available)
                        cd "$DEPLOY_DIR"
                        if command -v python3 &> /dev/null; then
                            echo "🚀 Starting local server on http://localhost:3000"
                            # Kill any existing server on port 3000
                            pkill -f "python3 -m http.server 3000" || true
                            # Start new server in background
                            nohup python3 -m http.server 3000 > server.log 2>&1 &
                            echo "Server PID: $!"
                            sleep 2
                            echo "✅ Local server started successfully"
                            echo "🌐 Visit: http://localhost:3000"
                        else
                            echo "Python3 not available for local server"
                        fi
                    '''
                }
            }
        }
    }
    
    post {
        always {
            echo 'Pipeline execution completed!'
            archiveArtifacts artifacts: 'dist/**/*', allowEmptyArchive: true
        }
        success {
            echo '✅ Pipeline executed successfully!'
            echo '🚀 Your portfolio website is ready!'
            echo '📁 Build artifacts are stored in the dist/ directory'
            echo '🌐 Local deployment: http://localhost:3000'
        }
        failure {
            echo '❌ Pipeline failed!'
            echo 'Please check the logs for more details.'
        }
    }
}