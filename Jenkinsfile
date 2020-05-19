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
 
            }
            
        }
       stage('Deploy to Production'){
          steps{
              timeout(time:5, unit:'DAYS'){
               input message:'Approve PRODUCTION Deployment?'
                }
		echo "Now Copying the Artifacts from another project"
		copyArtifacts filter: '**/*.war', fingerprintArtifacts: true, projectName: 'Building_Packaging_Application'
                build job: 'Deploy_Aritifact_OnProd'
            }
        }
        
    }
}
