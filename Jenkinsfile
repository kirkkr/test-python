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
		package_full="$package_path/$package_name"
	
		sh "rm -Rf $package_full"
		sh "tar -cvf $package_full ."
	}

	stage('Deploy') {
		echo "Deploying to Artifactory..."
		sh "curl -X PUT http://localhost:8081/artifactory/generic-local-dev/$env.JOB_NAME/$env.BUILD_NUMBER/$package_name -T $package_full"
	}
}