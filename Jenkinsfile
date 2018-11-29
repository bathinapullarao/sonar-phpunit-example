pipeline {
	agent {
		label 'master'
	}
	environment {
		scannerHome = tool 'sonar'
	}  
	stages {
		stage ('checkout') {
			steps {
				node ('master') {
					checkout scm
				}
			}
		}
		stage ('sonar') {
			steps {
			node ('master') {
				withSonarQubeEnv('sonar') {
					sh "phpunit --bootstrap src/Math.php tests/SomeTest.php"
					sh '${scannerHome}/bin/sonar-scanner'
				}
			}	}
		}
		stage ('sleep') {
			steps {
				node ('master') {
					sleep 100 
				}
			}
		}
		stage (sonarstatus) {
			steps {
				script {
					def qualitygate = waitForQualityGate()
					if (qualitygate.status != "OK") {
						echo "Quality gate is fail"
	}
					else {
						echo "Quality Gate has pass"
					}
				}
			}
		}
}
}
