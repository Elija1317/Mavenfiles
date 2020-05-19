pipeline {
    agent any
    stages {
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
                build job: 'Deploy_to_staging_env'
				deploy adapters: [tomcat8(credentialsId: '8648a7d8-a5af-4ffd-bd49-cd7e99201cd9', path: '', url: 'http://ec2-3-84-79-252.compute-1.amazonaws.com:8080/')], contextPath: '/', war: '**/*.war'
 
            }
            
        }
		stage('Deploy to Production'){
            steps{
                timeout(time:5, unit:'DAYS'){
                    input message:'Approve PRODUCTION Deployment?'
                }
				echo "Now Copying the Artifacts from another project"
				copyArtifacts filter: '**/*.war', fingerprintArtifacts: true, projectName: 'Building_Packaging_Application'
                deploy adapters: [tomcat8(credentialsId: '8648a7d8-a5af-4ffd-bd49-cd7e99201cd9', path: '', url: 'http://ec2-3-84-79-252.compute-1.amazonaws.com:9090/')], contextPath: '/', war: '**/*.war'
            }
        }
        
    }
}


