node {

	major="1"
	minor="0"
	patch="0"
	build_number="${BUILD_NUMBER}"
	build_version="$major.$minor.$patch-$build_number"

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
		package_name="test-python-${build_version}.tar.gz"
		package_full="$package_path/$package_name"
		
		sh "rm -Rf $package_full"
		sh "tar -cvf $package_full ."
		sh "tar -lvf $package_full ."
	}

	stage('Deploy') {
		echo "Deploying to Artifactory..."
		withCredentials([[$class: 'UsernamePasswordMultiBinding', credentialsId: 'artifactory-id',
			usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD']]) {
			sh "curl -u $USERNAME:$PASSWORD -X PUT http://localhost:8081/artifactory/generic-local-dev/${JOB_NAME}/$build_version/$package_name -T $package_full"
		}
	}
}