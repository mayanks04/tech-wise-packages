Below is a **general-purpose Jenkins pipeline (Declarative syntax)** that covers all stages of a standard **CI/CD flow: Checkout → Build → Test → Package → Dockerize → Push to Registry → Deploy**.

I'll provide a modular template that you can customize for any technology (Java, Node, Python, etc.).

---

### ✅ Jenkinsfile (Generic CI/CD Pipeline)

```groovy
pipeline {
    agent any

    environment {
        REGISTRY_CREDENTIALS = credentials('docker-registry-creds') // Jenkins Credentials ID
        IMAGE_NAME = 'your-app-name'
        REGISTRY = 'your-docker-registry.com'
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                script {
                    if (fileExists('pom.xml')) {
                        echo "Building Java project with Maven"
                        sh 'mvn clean package -DskipTests'
                    } else if (fileExists('build.gradle')) {
                        echo "Building Java project with Gradle"
                        sh './gradlew build -x test'
                    } else if (fileExists('package.json')) {
                        echo "Building Node.js project"
                        sh 'npm install && npm run build'
                    } else if (fileExists('setup.py')) {
                        echo "Building Python package"
                        sh 'python setup.py sdist bdist_wheel'
                    } else {
                        error "No recognizable build file found"
                    }
                }
            }
        }

        stage('Test') {
            steps {
                script {
                    if (fileExists('pom.xml')) {
                        sh 'mvn test'
                    } else if (fileExists('build.gradle')) {
                        sh './gradlew test'
                    } else if (fileExists('package.json')) {
                        sh 'npm test || echo "No tests defined"'
                    } else {
                        echo "No test stage defined for this project"
                    }
                }
            }
        }

        stage('Package') {
            steps {
                script {
                    echo "Packaging stage - handled during build for most languages"
                }
            }
        }

        stage('Docker Build') {
            steps {
                script {
                    sh """
                    docker build -t ${REGISTRY}/${IMAGE_NAME}:${BUILD_NUMBER} .
                    """
                }
            }
        }

        stage('Docker Push') {
            steps {
                script {
                    sh """
                    echo "${REGISTRY_CREDENTIALS_PSW}" | docker login ${REGISTRY} -u "${REGISTRY_CREDENTIALS_USR}" --password-stdin
                    docker push ${REGISTRY}/${IMAGE_NAME}:${BUILD_NUMBER}
                    docker logout ${REGISTRY}
                    """
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    echo "Triggering deployment"
                    // Example for Kubernetes:
                    sh """
                    kubectl set image deployment/${IMAGE_NAME} ${IMAGE_NAME}=${REGISTRY}/${IMAGE_NAME}:${BUILD_NUMBER}
                    """
                }
            }
        }
    }

    post {
        always {
            echo 'Pipeline completed.'
        }
        failure {
            echo 'Pipeline failed!'
        }
        success {
            echo 'Pipeline succeeded!'
        }
    }
}
```

---

## 🔧 Prerequisites

* **Credentials**: Create a Jenkins credential with ID `docker-registry-creds` (type: Username/Password).
* **Dockerfile**: Ensure there's a valid `Dockerfile` at the project root.
* **kubectl** (optional): Ensure Jenkins agent has `kubectl` configured and access to your cluster.

---

## ✅ Customization by Tech Stack

| Tech      | Build Tool       | Deploy Target Example     |
| --------- | ---------------- | ------------------------- |
| Java      | Maven/Gradle     | Tomcat, Spring Boot (JAR) |
| Node.js   | npm              | NGINX, Node server        |
| Python    | setup.py/Poetry  | Gunicorn, Flask, Django   |
| .NET Core | `dotnet publish` | IIS, Azure App Service    |
| Go        | `go build`       | Binary via Docker/K8s     |

---
