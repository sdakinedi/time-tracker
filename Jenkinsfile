pipeline {
    agent any
    tools {
        maven "maven3.8.1"
    }
    stages {
        stage('Build') {
            steps {
                echo 'THis is Build step'
                git branch: 'develop', credentialsId: 'GithubCredentials', url: 'https://github.com/sdakinedi/time-tracker.git'
            }
        }
        
        stage('Integration Tests') {
            steps {
                echo 'This will test'
                 sh 'mvn test'
            }
        }  
        stage('Results') {
            steps {
                echo 'This will show Results'
                 junit '**/*.xml'
                 
            }
        }
        
        stage('Package') {
            steps {
                echo 'This will Package'
                 sh 'mvn package'
                 archiveArtifacts artifacts: '**/*.war', followSymlinks: false
            }
        }
		
		stage('Upload the Binary') {
            steps {
               nexusPublisher nexusInstanceId: 'nexus-on-ec2', nexusRepositoryId: 'Jenkins-ci-maven-nexus-repo', packages: [[$class: 'MavenPackage', mavenAssetList: [[classifier: '', extension: '', filePath: 'web/target/webapp-0.5.0-SNAPSHOT.war']], mavenCoordinate: [artifactId: 'webapp', groupId: 'com.mycompany', packaging: 'war', version: '1.0']]]
            }
        }
		
       stage('Deploy') {
            steps {
               echo 'This will Package'
                 deploy adapters: [tomcat9(credentialsId: 'TOMCAT_DEPLOYER', path: '', url: 'http://3.127.229.79:8090')], contextPath: null, war: '**/*.war'
           }
        }
		

    }
}
