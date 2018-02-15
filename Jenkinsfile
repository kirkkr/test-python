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
		echo "Packaging..."
		package_path="/tmp"
		echo "$package_path"
		package_name="$JOB_NAME.tar.gz"
		echo "$package_name"
		package_full="$package_path/$package_name"
		echo "$package_full"
	
		sh "rm -Rf $package_full"
		sh "tar -cvf $package_full ."
	}

	stage('Deploy') {
		echo "Deploying to Artifactory..."
		sh "curl -X PUT http://localhost:8081/artifactory/generic-local-dev/$JOB_NAME/$BUILD_NUMBER/$package_name -T $package_full"
	}
}