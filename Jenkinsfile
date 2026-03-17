pipeline {
    agent any

    stages {

        stage('Build') {
            steps {
                sh '''
                echo "Building Java project..."
                cd "Password Protection"
                mkdir -p build
                javac -d build src/*.java
                echo "Build successful"
                '''
            }
        }

        stage('Test') {
            steps {
                sh '''
                echo "Running JUnit tests..."
                cd "Password Protection"

                # DELETE OLD/CORRUPT JAR (IMPORTANT)
                rm -f junit-platform-console-standalone.jar

                echo "Downloading JUnit..."
                curl -L -o junit-platform-console-standalone.jar https://repo1.maven.org/maven2/org/junit/platform/junit-platform-console-standalone/1.10.0/junit-platform-console-standalone-1.10.0.jar

                mkdir -p test-build

                javac -cp junit-platform-console-standalone.jar:build -d test-build test/*.java

                java -jar junit-platform-console-standalone.jar \
                --class-path build:test-build \
                --scan-class-path

                echo "JUnit tests executed successfully"
                '''
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                echo "Deploying application..."
                cd "Password Protection"
                jar cf FileEncrypter.jar -C build .
                echo "Deployment successful"
                '''
            }
        }
    }

    post {
        success {
            echo "Pipeline executed successfully!"
        }
        failure {
            echo "Pipeline failed!"
        }
    }
}
