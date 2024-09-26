#### 1. Adım localde fastapi çalıştır ve kontrol et.

#### 2. Localde imaj ve container çalışıtır ve uygulamayı kontrol et

```
pipeline {
    agent any

    triggers {
        githubPush()  // Listen for GitHub push events
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/KuserOguzHan/githup_jenkins_1.git'
            }
        }

        stage('Install Python venv') {
            steps {
                script {
                    echo 'Installing python3-venv if necessary...'
                    sh '''#!/bin/bash
                    sudo apt-get update
                    sudo apt-get install -y python3-venv
                    '''
                }
                echo 'python3-venv installed or already present'
            }
        }

        stage('Virtualenv and Requirements') {
            steps {
                script {
                    echo 'Setting up Python virtual environment and installing requirements...'
                    sh '''#!/bin/bash
                    python3 -m venv venv
                    . venv/bin/activate
                    pip install -r requirements.txt
                    '''
                }
                echo 'Python environment is set up and requirements are installed'
            }
        }

        stage('Run FastAPI Application') {
            steps {
                script {
                    echo 'Starting FastAPI application with Uvicorn...'
                    sh '''#!/bin/bash
                    . venv/bin/activate
                    uvicorn main:app --host 0.0.0.0 --port 8002
                    '''
                }
                echo 'Uvicorn is running'
            }
        }
    }

    post {
        always {
            cleanWs()
        }
    }
}
```

### 3. Test ortamında çalıştırma
