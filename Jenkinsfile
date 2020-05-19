pipeline {
    agent any
	
    stages {
		stage('Checking the SCM'){
			git 'https://github.com/Elija1317/Mavenfiles.git'
		
		}
        stage('Build Application') {
            steps {
                sh 'mvn -f pom.xml clean package'
            }
            post {
                success {
                    echo "Now Archiving the Artifacts...."
                    archiveArtifacts artifacts: '**/*.war'
                }
            }
        }
        stage('Deploy in Staging Environment'){
            steps{
	 echo "Now Copying the Artifacts from another project"
	 copyArtifacts filter: '**/*.war', fingerprintArtifacts: true, projectName: 'Building_Packaging_Application'
                deploy adapters: [tomcat8(credentialsId: '8648a7d8-a5af-4ffd-bd49-cd7e99201cd9', path: '', url: 'http://ec2-3-84-79-252.compute-1.amazonaws.com:8080/')], contextPath: '/', war: '**/*.war'
            }
            
        }
		
        
    }
}

