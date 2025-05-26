Here are **language-specific Jenkins CI/CD pipelines** covering full stages from **build to deploy**:

---

## ☕ Java (Spring Boot with Maven)

```groovy
pipeline {
    agent any

    environment {
        IMAGE = "my-java-app"
        REGISTRY = "your-registry.com"
        CREDS = credentials('docker-registry-creds')
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build with Maven') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }

        stage('Docker Build & Push') {
            steps {
                sh """
                docker build -t $REGISTRY/$IMAGE:$BUILD_NUMBER .
                echo "$CREDS_PSW" | docker login $REGISTRY -u "$CREDS_USR" --password-stdin
                docker push $REGISTRY/$IMAGE:$BUILD_NUMBER
                docker logout $REGISTRY
                """
            }
        }

        stage('Deploy to K8s') {
            steps {
                sh """
                kubectl set image deployment/$IMAGE $IMAGE=$REGISTRY/$IMAGE:$BUILD_NUMBER
                """
            }
        }
    }
}
```

---

## 🟩 Node.js (React/Express)

```groovy
pipeline {
    agent any

    environment {
        IMAGE = "my-node-app"
        REGISTRY = "your-registry.com"
        CREDS = credentials('docker-registry-creds')
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Install & Build') {
            steps {
                sh 'npm install'
                sh 'npm run build'
            }
        }

        stage('Test') {
            steps {
                sh 'npm test || echo "No tests"'
            }
        }

        stage('Docker Build & Push') {
            steps {
                sh """
                docker build -t $REGISTRY/$IMAGE:$BUILD_NUMBER .
                echo "$CREDS_PSW" | docker login $REGISTRY -u "$CREDS_USR" --password-stdin
                docker push $REGISTRY/$IMAGE:$BUILD_NUMBER
                docker logout $REGISTRY
                """
            }
        }

        stage('Deploy to K8s') {
            steps {
                sh """
                kubectl set image deployment/$IMAGE $IMAGE=$REGISTRY/$IMAGE:$BUILD_NUMBER
                """
            }
        }
    }
}
```

---

## 🐍 Python (Flask/Django)

```groovy
pipeline {
    agent any

    environment {
        IMAGE = "my-python-app"
        REGISTRY = "your-registry.com"
        CREDS = credentials('docker-registry-creds')
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Install') {
            steps {
                sh 'pip install -r requirements.txt'
            }
        }

        stage('Test') {
            steps {
                sh 'pytest || echo "No tests"'
            }
        }

        stage('Docker Build & Push') {
            steps {
                sh """
                docker build -t $REGISTRY/$IMAGE:$BUILD_NUMBER .
                echo "$CREDS_PSW" | docker login $REGISTRY -u "$CREDS_USR" --password-stdin
                docker push $REGISTRY/$IMAGE:$BUILD_NUMBER
                docker logout $REGISTRY
                """
            }
        }

        stage('Deploy to K8s') {
            steps {
                sh """
                kubectl set image deployment/$IMAGE $IMAGE=$REGISTRY/$IMAGE:$BUILD_NUMBER
                """
            }
        }
    }
}
```

---

## 🟪 .NET Core

```groovy
pipeline {
    agent any

    environment {
        IMAGE = "my-dotnet-app"
        REGISTRY = "your-registry.com"
        CREDS = credentials('docker-registry-creds')
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Restore & Build') {
            steps {
                sh 'dotnet restore'
                sh 'dotnet build -c Release'
            }
        }

        stage('Test') {
            steps {
                sh 'dotnet test'
            }
        }

        stage('Publish & Dockerize') {
            steps {
                sh 'dotnet publish -c Release -o out'
                sh """
                docker build -t $REGISTRY/$IMAGE:$BUILD_NUMBER .
                echo "$CREDS_PSW" | docker login $REGISTRY -u "$CREDS_USR" --password-stdin
                docker push $REGISTRY/$IMAGE:$BUILD_NUMBER
                docker logout $REGISTRY
                """
            }
        }

        stage('Deploy to K8s') {
            steps {
                sh """
                kubectl set image deployment/$IMAGE $IMAGE=$REGISTRY/$IMAGE:$BUILD_NUMBER
                """
            }
        }
    }
}
```

---

Do you also want templates for:

* 🧱 C++ with CMake?
* 🦫 Go (Golang)?
* 🦀 Rust?

Or deployment alternatives like:

* 🐳 Docker Swarm
* ☁️ AWS ECS/EC2
* 🔧 Tomcat/JBoss for Java WARs?


