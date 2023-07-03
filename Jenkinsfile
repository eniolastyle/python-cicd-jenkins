pipeline {
    agent any

    stages {
        stage("Cleanup Workspace") {
	    steps {
	        cleanWs()
	    }
	}

	stage("Checkout from SCM") {
	    steps {
		git branch: 'main', credentialsId: 'github', url: 'https://github.com/eniolastyle/python-cicd-jenkins'
	    }
	}

	stage("Build Application"){
            steps {
                sh "python app.py"
            }
        }
    }
}

