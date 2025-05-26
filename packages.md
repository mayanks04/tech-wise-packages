Here's a comprehensive guide to **build/package types and tools** across various programming languages and technologies, covering how applications are typically built and packaged for deployment in CI/CD pipelines.

---

## 🔧 Language & Technology Build/Package Types

| **Technology**            | **Build Tool**       | **Output Package Type**              | **Command(s)**                                      | **Notes**                                     |
| ------------------------- | -------------------- | ------------------------------------ | --------------------------------------------------- | --------------------------------------------- |
| **Java**                  | Maven / Gradle       | `.jar`, `.war`, `.ear`               | `mvn package`, `./gradlew build`                    | WAR: Web app, JAR: libraries or microservices |
| **Spring Boot**           | Maven / Gradle       | Fat `.jar`                           | `./gradlew bootJar`                                 | Fat JAR includes all dependencies             |
| **Node.js**               | npm / yarn           | `.tgz`, `.zip` (custom)              | `npm pack`, `npm run build`                         | Often zipped or Dockerized                    |
| **Python**                | setuptools / Poetry  | `.whl`, `.tar.gz`                    | `python setup.py sdist bdist_wheel`, `poetry build` | `whl` used for installable packages           |
| **C++**                   | CMake / Make         | `.exe`, `.out`, `.dll`, `.so`, `.a`  | `cmake . && make`                                   | OS-specific binaries                          |
| **.NET (C#)**             | MSBuild / dotnet CLI | `.dll`, `.exe`, `.nupkg`             | `dotnet build`, `dotnet publish`                    | For web apps: `dotnet publish -c Release`     |
| **Go**                    | Go toolchain         | Binary (no extension or `.exe`)      | `go build`, `go install`                            | Cross-compile supported                       |
| **Rust**                  | Cargo                | Binary (`target/release/...`)        | `cargo build --release`                             | Produces optimized executable                 |
| **PHP**                   | Composer             | N/A (source deployment)              | `composer install`                                  | Often deployed as raw source with autoloaders |
| **Ruby**                  | Bundler              | `.gem`                               | `gem build`, `bundle install`                       | `.gem` for reusable libraries                 |
| **Scala**                 | SBT                  | `.jar`                               | `sbt package`                                       | Similar to Java, can be fat JAR               |
| **Kotlin**                | Gradle / Maven       | `.jar`, `.war`                       | `./gradlew build`                                   | Same as Java/Spring                           |
| **Android**               | Gradle               | `.apk`, `.aab`                       | `./gradlew assembleDebug` / `assembleRelease`       | AAB is modern app bundle                      |
| **iOS (Swift)**           | Xcode / Fastlane     | `.ipa`                               | `xcodebuild`, `fastlane build`                      | Requires macOS                                |
| **React / Angular / Vue** | npm / yarn           | Static assets in `/dist` or `/build` | `npm run build`                                     | Served via NGINX or packaged in Docker        |
| **Dockerized App**        | Docker               | `.tar` (image), pushed to registry   | `docker build`, `docker save`                       | Preferred deployment format                   |

---

## 📦 Example: What Gets Deployed

| Technology | Build Artifact                    | Deployment Target                      |
| ---------- | --------------------------------- | -------------------------------------- |
| Java WAR   | `app.war`                         | Apache Tomcat / JBoss                  |
| Java JAR   | `app.jar`                         | Spring Boot / Standalone Java          |
| Node.js    | `build/` folder or Docker image   | NGINX, Node server, or Lambda          |
| Python     | `.whl`, source code               | Virtualenv + Gunicorn / Flask / Django |
| .NET       | `publish/` folder                 | IIS, Azure App Service                 |
| C++        | Binary (.exe/.so)                 | Native servers / IoT                   |
| Go         | Binary                            | Lightweight servers                    |
| Docker     | Image pushed to ECR/GCR/DockerHub | Kubernetes / ECS / VMs                 |

---

## 📁 Suggested Folder Structure

```
project-root/
│
├── src/                    # source code
├── build/ or dist/         # compiled output
├── Dockerfile              # optional
├── Makefile or build.gradle/pom.xml/setup.py
├── package.json / composer.json / pyproject.toml
└── README.md
```

---

## 🛠️ Build + Package Commands by Language

### Java

```bash
mvn clean package   # JAR/WAR
./gradlew build     # JAR/WAR
```

### Python

```bash
python setup.py sdist bdist_wheel
poetry build
```

### Node.js

```bash
npm install
npm run build
```

### C++

```bash
cmake .
make
```

### .NET Core

```bash
dotnet build
dotnet publish -c Release -o ./publish
```

### Go

```bash
go build -o myapp
```

### Rust

```bash
cargo build --release
```

---

Would you like a **template GitHub Action or Jenkinsfile** to automate the build for each of these languages? I can provide them per language.
