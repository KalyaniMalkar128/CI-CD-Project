pipeline {
    agent any

    stages {
        stage('Git-Clone') {
            steps {
               git branch: 'main', url: 'https://github.com/kalyani-123-malkar/spring-framework-petclinic.git' 
            }
        }
        stage('package') {
            steps {
                    sh 'mvn package'    
           }
        }
        stage('sonarqube') {
            steps {
                withSonarQubeEnv(installationName: 'sonarqube', credentialsId: 'jenkins-sonar-token')
                  {
                      sh 'mvn verify sonar:sonar'
                  } 
           }
        }
        stage('nexus') {
            steps {
                    nexusArtifactUploader artifacts: [[artifactId: 'spring-framework-petclinic', 
                    classifier: '', 
                    file: 'target/petclinic.war',
                    type: 'war']], 
                    credentialsId: 'nexus',
                    groupId: 'org.springframework.samples', 
                    nexusUrl: '65.1.127.154:8081',
                    nexusVersion: 'nexus3', 
                    protocol: 'http', 
                    repository: 'spring-petclinic',
                    version: '7.0.3' 
           }
        }
        stage('tomcat') {
            steps {
                sshagent(['tomcat'])
                {
                    sh 'scp -o StrictHostKeyChecking=no target/petclinic.war ubuntu@65.0.116.229:/home/ubuntu/apache-tomcat-10.1.52/webapps/'  }
        }
    }
    }
}
