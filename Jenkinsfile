node{
   stage('SCM Checkout'){
     git 'https://github.com/BagavathiSelvam/my-app.git'
   }
   stage('Compile-Package'){
      def mvnHome =  tool name: 'maven3', type: 'maven'   
      sh "${mvnHome}/bin/mvn clean package"
	  sh 'mv target/myweb*.war target/newapp.war'}
	stage('SonarQube Analysis') {
	        def mvnHome =  tool name: 'maven3', type: 'maven'
	        withSonarQubeEnv('sonar') { 
	        sh "${mvnHome}/bin/mvn sonar:sonar"
	}
stage('Build Docker Image'){
   sh 'docker build -t bagavath1805/myweb:0.0.2 .'
   }
   stage('Docker Image Push'){
   withCredentials([string(credentialsId: 'dockerPass', variable: 'dockerPassword')]){
   sh "docker login -u bagavath1805 -p ${dockerPassword}"
    }
   sh 'docker push bagavath1805/myweb:0.0.2'
   }
   stage('Nexus Image Push'){
   sh "docker login -u admin -p admin123 3.111.35.209:8083"
   sh "docker tag bagavath1805/myweb:0.0.2 3.111.35.209:8083/damo:1.0.0"
   sh 'docker push 3.111.35.209:8083/damo:1.0.0'}
    stage('Remove Previous Container'){
	try{
	sh 'docker rm -f tomdocker'
	}catch(error){
		//  do nothing if there is an exception
	}
   stage('Docker deployment'){
   sh 'docker run -d -p 8090:8080 --name tomdocker bagavath1805/myweb:0.0.2'}
    }
}
}
