pipeline {
	agent none 
	
	stages {
	  stage('Unit Tests') {
	    agent {
	      label 'apache'
	    }
	    steps {
	      sh 'ant -f test.xml -v'
	      junit 'reports/results.xml'
	    }
	  }
	  stage('build') {
	    agent {
	      label 'apache'
	    }
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
