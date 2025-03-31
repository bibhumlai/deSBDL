pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
               sh 'pipenv --python python3 sync'
            }
        }
        stage('Test') {
            steps {
               sh 'pipenv run pytest'
            }
        }
        stage('Package') {
	    when{
		    anyOf{ branch "master" ; branch 'release' }
	    }
            steps {
               sh 'zip -r sbdl.zip lib'
            }
        }
	stage('Release') {
	   when{
	      branch 'release'
	   }
           steps {
              sh "scp -i /root/.ssh/edge-node_key.pem -o 'StrictHostKeyChecking no' -r sbdl.zip log4j.properties sbdl_main.py sbdl_submit.sh conf bibhumlai7@http://34.58.58.62:/home/bibhu/sbdl-qa"
           }
        }
	stage('Deploy') {
	   when{
	      branch 'master'
	   }
           steps {
               sh "scp -i /root/.ssh/edge-node_key.pem -o 'StrictHostKeyChecking no' -r sbdl.zip log4j.properties sbdl_main.py sbdl_submit.sh conf bibhumlai7@http://34.58.58.62:/home/bibhu/sbdl-prod"
           }
        }
    }
}