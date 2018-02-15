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
		package_name="test-python-${BUILD_NUMBER}.tar.gz"
		package_full="$package_path/$package_name"
		
		sh "rm -Rf $package_full"
		sh "tar -cvf $package_full ."
	}

	stage('Deploy') {
		echo "Deploying to Artifactory..."
		withCredentials([[$class: 'UsernamePasswordMultiBinding', credentialsId: 'artifactory-id',
			usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD']]) {
			sh "curl -u $USERNAME:$PASSWORD -X PUT http://localhost:8081/artifactory/generic-local-dev/${JOB_NAME}/${BUILD_NUMBER}/$package_name -T $package_full"
		}
	}
}