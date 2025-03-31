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
              sh "cp -r sbdl.zip log4j.properties sbdl_main.py sbdl_submit.sh conf /home/bibhumlai7/sbdl/qa/"
           }
        }
	stage('Deploy') {
	   when{
	      branch 'master'
	   }
           steps {
               sh "cp -r sbdl.zip log4j.properties sbdl_main.py sbdl_submit.sh conf /home/bibhumlai7/sbdl/prod/"
           }
        }
    }
}