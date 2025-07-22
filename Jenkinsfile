node{
    
    def mavenHome=tool name: "maven-3.9.9"
    
    slackSend channel: 'my-new-channel', message: "started ${env.JOB_NAME}  build:${env.BUILD_NUMBER} (<${env.BUILD_URL}|Open>)", tokenCredentialId: 'slack-creds'
    
    stage("git checkout"){
        git branch: 'main', credentialsId: 'git-credentials', url: 'https://github.com/ajay2603/maven_1.git'
    }
    
    stage("compile"){
        sh "${mavenHome}/bin/mvn clean compile"
    }
    
    stage("build"){
        sh "${mavenHome}/bin/mvn package"
    }
    
    stage("sonar report"){
        sh "${mavenHome}/bin/mvn sonar:sonar"
    }
    
    stage("deployment"){
        deploy adapters: [tomcat9(alternativeDeploymentContext: '', credentialsId: 'tomcat-credentials', path: '', url: 'http://172.24.31.226:9090/')], contextPath: '/web-app-new-dev', onFailure: false, war: '**/*.war'
    }
    
    stage("upload artifactory"){
        sh "${mavenHome}/bin/mvn deploy"
    }
    
}
