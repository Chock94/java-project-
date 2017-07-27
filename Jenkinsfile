pipeline {
	agent any 
	
	stages {
	  stage('Unit Tests') {
	    steps {
	      sh 'ant -f test.xml -v'
	      junit 'reports/results.xml'
	    }
	  }
	  stage('build') {
	    steps {
	      sh 'ant -f build.xml -v'
	    }
	    post {
	      success {
	        archiveArtifacts artifacts: 'dist/*.jar', fingerprint: true
	      }
	    } 
	  }
       	}
}
