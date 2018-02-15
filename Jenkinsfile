node {

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
		package_path="/tmp"
		package_name="$env.JOB_NAME-$env.BUILD_NUMBER.tar.gz"
		package="$package_path/$package_name"
	
		sh "rm -Rf $package"
		sh "tar -cvf $package ."
	}

	stage('Deploy') {
		echo "Deploying to Artifactory..."
		sh "curl -X PUT http://localhost:8081/artifactory/generic-local-dev/$env.JOB_NAME/$env.BUILD_NUMBER/$package_name -T $package"
	}
}