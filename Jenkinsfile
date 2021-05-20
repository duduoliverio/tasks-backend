pipeline{
	agent any
	stages{
		stage ('Build Backend'){
			steps{
				bat 'mvn clean package -DskipTests=true'
			}
		}
		stage ('Unit Tests'){
			steps{
				bat 'mvn test'
			}
		}
		stage ('Sonar Analysis'){
			environment{
				scannerHome = tool 'SONAR_SCANNER'
			}
			steps{
				withSonarQubeEnv('SONAR_LOCAL'){
					bat "${scannerHome}/bin/sonar-scanner -e -Dsonar.projectKey=DeployBack -Dsonar.host.url=http://localhost:9000 -Dsonar.login=640c9cbdd344dabb64f7a299655ed5a95457aea2 -Dsonar.java.binaries=target -Dsonar.coverage.exclusions=**/.mvn/**,**/src/test/**,**/model/**,**Application.java"
				}
			}
		}
		
		stage ('Deploy Backend'){
			steps{
				deploy adapters: [tomcat8(credentialsId: 'TomcatLogin', path: '', url: 'http://localhost:8001/')], contextPath: 'tasks-backend', war: 'target/tasks-backend.war'
			}
		}
		stage ('API Test'){
			steps{
				dir('api-test'){
					git url: 'https://github.com/duduoliverio/tasks-apitest'
					bat 'mvn test'
				}
			}
		}
	}
}
