pipeline{
	agent any
		stages{
			stage('Build Application'){
				steps{
					sh 'mvn -f pom.xml clean package'
				}
			
			}
		}
}
