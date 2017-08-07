pipeline {
	agent none
	
	environment {
	MAJOR_VERSION = 1
	}	

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
	  stage('deploy') {
	    agent {
              label 'apache'
            }
	    steps {
	      sh "if ![ -d '/var/www/html/rectangles/all/${env.BRANCH_NAME}' ]; then mkdir /var/www/html/rectangles/all/${env.BRANCH_NAME}; fi"
	      sh "cp dist/rectangle_${env.MAJOR_VERSION}.${env.BUILD_NUMBER}.jar /var/www/html/rectangles/all/${env.BRANCH_NAME}/"
	    }
       	  }
	  stage("Running on CentOS") {
	    agent {
	      label 'CentOS'
	    }
	    steps {
	      sh "wget http://chock944.mylabserver.com/rectangles/all/${env.BRANCH_NAME}/rectangle_${env.MAJOR_VERSION}.${env.BUILD_NUMBER}.jar"
	      sh "java -jar rectangle_${env.MAJOR_VERSION}.${env.BUILD_NUMBER}.jar 3 4"
	    }
	  }
	  stage("Test on Debian") {
	    agent {
	      docker 'openjdk:8u121-jre'
	    }
	    steps {
	      sh "wget http://chock944.mylabserver.com/rectangles/all/${env.BRANCH_NAME}/rectangle_${env.MAJOR_VERSION}.${env.BUILD_NUMBER}.jar"
	      sh "java -jar rectangle_${env.MAJOR_VERSION}.${env.BUILD_NUMBER}.jar 3 4"
	    }
	  }
	  stage('Promote to Green') {
	    agent {
	      label 'apache'
	    }
	  when {
	    branch 'master'
	  }
	    steps {
	      sh "cp /var/www/html/rectangles/all/rectangle_${env.MAJOR_VERSION}.${env.BUILD_NUMBER}.jar /var/www/html/rectangles/green/rectangle_${env.MAJOR_VERSION}.${env.BUILD_NUMBER}.jar"
	    }
	  }
	  stage('Promote development branch to Master') {
	    agent {
	      label 'apache'
	    }
	    when {
	      branch 'development'
	    }
	    steps {
	      echo "Staching any local Changes"
    	      sh 'git stash'
	      echo "Checking out development branch"
	      sh 'git checkout development'
              echo "Checking out Master branch"
	      sh 'git checkout master'
	      echo "Merging development into Master branch"
	      sh ' git merge development'
	      echo "Pushing to origin Master"
	      sh 'git push origin master'
	      echo 'Tagging the release'
	      sh "git tag rectangle.${env.MAJOR_VERSION}.${env.BUILD_NUMBER}"
	      sh "git push origin rectangle.${env.MAJOR_VERSION}.${env.BUILD_NUMER}"
	    }
	  }
	}
      }
