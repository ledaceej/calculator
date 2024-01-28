pipeline {
agent any
stages {
stage("Compile") {
steps {
sh "./gradlew compileJava"
}
}
stage("Unit test") {
steps {
sh "./gradlew test"
}
}
stage("Code coverage") {
steps {
sh "./gradlew jacocoTestReport"
sh "./gradlew jacocoTestCoverageVerification"
}
}
stage("Static code analysis") {
steps {
sh "./gradlew checkstyleMain"
}
}
stage("Package") {
steps {
sh "./gradlew build"
}
}
stage("Docker build") {
steps {
sh "docker build -t ledaceej/calculator ."
}
}
stage("Docker push") {
steps {
sh "docker push ledaceej/calculator"
}
}
stage("Deploy to staging") {
steps {
sh "docker run -d --rm -p 8765:8083 --name calculator ledaceej/calculator"
}
}
stage("Acceptance test") {
steps {
sleep 60
sh "chmod +x acceptance_test.sh && ./acceptance_test.sh"
}
}
}
post {
always {
sh "docker stop calculator"
}
}
}
