node {
	def mavenHome
        
        stage('Code Checkout') { 
		// Get code from a repository and Git has to be installed in the system; git must be configured in the Global Tool Configuration
		git 'https://github.com/kratika10rathore/spring-petclinic.git'
           
		// Get the Maven tool configured in Global Tool Configuration 
		// 'apache-maven-3.5.3' Maven tool must be configured in the global configuration.
		mavenHome = tool 'Maven'
        }
        stage('Code Analysis') {
                // Configure SonarQube Scanner in Manage Jenkins -> Global Tool Configuration
                def scannerHome = tool 'SonarQube Scanner 3.0.3';

                // Sonarqube 7 must be configured in the Jenkins Manage Jenkins -> Configure System -> Add SonarQube server 
                withSonarQubeEnv('sonarqube-7.7') {
                        bat "${scannerHome}/bin/sonar-scanner -Dsonar.host.url=http://localhost:9000 -Dsonar.login=ae4ad2719f4e184fca8bc60610d8746a1eeea4cf -Dsonar.projectVersion=1.0 -Dsonar.projectKey=PetClinic_Key -Dsonar.sources=src -Dsonar.java.binaries=**/classes/**"
                }
        } 
	stage('Build') {
                // Execute shell script if OS flavor is Linux
                if (isUnix()) {
                        sh "'${mavenHome}/bin/mvn' -Dmaven.test.failure.ignore clean package"
                        // Publish JUnit Report
                        junit '**/target/surefire-reports/TEST-*.xml'
                } 
                else {
                        // Execute Batch script if OS flavor is Windows		
                        bat(/"${mavenHome}\bin\mvn" clean package/)
                        // Publish JUnit Report
                        junit '**/target/surefire-reports/TEST-*.xml'
                }
	}
}
