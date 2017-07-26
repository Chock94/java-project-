pipeline {
	agent master 
	
	stages {
	  stage ('build') {
	    sh 'ant -f build.xml -v'
	  }
	}
}
