node{
    def buildNumber = BUILD_NUMBER
    stage("Git Clone"){
        
        git url: 'https://github.com/zayarlab-local/java_web_app_docker.git', branch:'master' 
    }
    stage("Maven Clean Package"){
        
        def mavenHome = tool name: "Maven", type: "maven"
            sh"${mavenHome}/bin/mvn clean package"
    }
    stage("Build Docker Image"){
        
        sh"docker build -t kaungmyat007/java-web-app-docker:${buildNumber} ."
    }
    stage("Docker login And Push"){
        withCredentials([string(credentialsId: 'dockerhubpwd', variable: 'dockerhubpwd')]) {
            sh"docker login -u kaungmyat007 -p ${dockerhubpwd}"
        }
        sh "docker push kaungmyat007/java-web-app-docker:${buildNumber}"
    }
    stage("Deploy App as Docker Container in Dev Server"){
        sshagent(['docker_dev_srv']) {
            sh"ssh -o StrictHostKeyChecking=no ubuntu@172.31.40.2 docker rm -f javawebappcontainer || true"
            sh"ssh -o StrictHostKeyChecking=no ubuntu@172.31.40.2 docker run -d -p 8080:8080 --name javawebappcontainer kaungmyat007/java-web-app-docker:${buildNumber}"
        }
    }
}
