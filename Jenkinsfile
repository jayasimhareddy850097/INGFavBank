pipeline {
agent any
stages {
stage('SCM Checkout') {
steps {
git url: 'https://github.com/jayasimhareddy850097/INGFavBank.git'
}
}
stage('Build') {
steps {
    sh"mvn clean package sonar:sonar -Dmaven.test.skip=true"
}
}
stage('Build Analysis') {
steps {
    withSonarQubeEnv('sonar'){
    sh"mvn sonar:sonar"
   }
}
}

stage('Quality Gates'){
steps{
   timeout(time: 5, unit: 'MINUTES') {
    waitForQualityGate abortPipeline: true
}
}
}
stage('Deploy') {
steps {
sh"mvn clean deploy"
}
}
stage('Release') {
steps {
sh"export JENKINS_NODE_COOKIE=dontKillMe; nohup java -jar $WORKSPACE/target/*.jar &"
}
}
}
}
