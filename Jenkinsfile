pipeline{
  environment{
    registry = "7903539838/shopizer"
    registryCredential = 'Docker_Hub_Amrendra'
    dockerImage = ''
  }
  agent any
  stages{
    stage ('Build') {
      steps{
        echo "Building Project"
        sh 'mvn clean install -DskipTests=True'
      }
    }
    stage ('Archive') {
      steps{
        echo "Archiving Project"
        archiveArtifacts artifacts: '**/*.war', followSymlinks: false
      }
    }	  
    stage ('Build Docker Image') {
      steps{
        echo "Building Docker Image"
        sh 'docker build -t 7903539838/shopizer:latest .'
      }
    }
    stage ('Push Docker Image') {
      steps{
        echo "Pushing Docker Image"
	script	{
	   docker.withRegistry('',registryCredential) {
    		sh 'docker push 7903539838/shopizer:latest'  
	   } 
	}      
      }
    }
    stage ('Deploy to Dev') {
      steps{
        echo "Deploying to Dev Environment"
	sh "docker rm -f shopizer || true"
        sh "docker run -d --name=shopizer -p 8081:8080 7903539838/shopizer"      
      }
    }
  }
}
