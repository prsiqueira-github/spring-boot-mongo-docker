node{
     
    stage('SCM Checkout'){
        git credentialsId: 'GIT_CREDENTIALS', url:  'https://github.com/prsiqueira-github/spring-boot-mongo-docker.git',branch: 'master'
    }
    
    stage(" Maven Clean Package"){
      def mavenHome =  tool name: "Maven-3.6.1", type: "maven"
      def mavenCMD = "${mavenHome}/bin/mvn"
      sh "${mavenCMD} clean package"
      
    } 
    
    
    stage('Build Docker Image'){
        sh 'docker build -t prsiqueira/spring-boot-mongo .'
    }
    
    stage('Push Docker Image'){
        withCredentials([string(credentialsId: 'DOCKER_HUB_PASSWORD', variable: 'DOCKER_HUB_PASSWORD')]) {
          sh "docker login -u prsiqueira -p ${DOCKER_HUB_PASSWORD}"
        }
        sh 'docker push prsiqueira/spring-boot-mongo'
     }
     
     stage("Deploy To Kubernetes Cluster"){
       kubernetesDeploy(
         configs: 'springBootMongo.yml', 
         kubeconfigId: 'KUBERNETES_CONFIG',
         enableConfigSubstitution: true
        )
     }
	 
	  /**
      stage("Deploy To Kubernetes Cluster"){
        sh 'kubectl apply -f springBootMongo.yml'
      } **/
     
}


