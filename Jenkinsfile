pipeline {
    agent any
	triggers {
        pollSCM('* * * * *')
    }
	 parameters {
        booleanParam(name: 'Deploy_To_Staging', defaultValue: false, description: 'Do You Want To Deploy To Staging?')
	}
	
    stages {
        stage('Build') {
            steps {
				echo "Building ${BRANCH_NAME}"
                sh 'nuget restore'
				sh 'msbuild'
            }
        }
        stage('Test') { 
            steps {
                echo "Running Test..."
				sh 'dotnet test -l:trx;LogFileName=.\testResult.xml'
            }
			post{
				 xunit (
                thresholds: [ skipped(failureThreshold: '0'), failed(failureThreshold: '0') ],
                tools: [ BoostTest(pattern: 'testResult/*.xml') ])
			}
        }
		stage('Publish Artifacts') { 
            steps {
                echo "Publishing Artifacts..."
            }
        }
		stage('Deploy To Dev') { 
            steps {
                echo "Deploying To Development..."
            }
        }
    }
	post{
		success {
		    script{
				echo "Success"
			}
		}
	}
}