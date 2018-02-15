node {

	PACKAGE_PATH="/tmp"
	PACKAGE_NAME="$env.JOB_NAME-$env.BUILD_NUMBER.tar.gz"
	PACKAGE="$PACKAGE_PATH/$PACKAGE_NAME"
	
	stage('Checkout') {
		echo "Checkout latest components from SCM"
		checkout scm
	}
	
	stage('Build') {
		echo "Building..."
	}
	
	stage('Test') {
		echo "Testing..."
		sh "python hello-world.py"
	}
	
	stage('Package') {
		sh "rm -Rf $PACKAGE || true"
		sh "tar -cvf $PACKAGE ."
	}

	stage('Deploy') {
		echo "Deploying to Artifactory..."
		sh "curl -X PUT http://localhost:8081/artifactory/generic-local-dev/$env.JOB_NAME/$env.BUILD_NUMBER/$PACKAGE-NAME -T $PACKAGE"
	}
}