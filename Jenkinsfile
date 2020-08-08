pipeline {
	agent any
	stages {
		stage('Lint HTML'){
                	steps {
                    		sh 'tidy -q -e travel_blog/*.html'
                	}
           	}
		stage('Upload to AWS') {
                	steps {
                        	withAWS(region:'us-west-2', credentials:'aws-static'){
                        		s3Upload(bucket:'static-website-mbweb', path:'', includePathPattern:'**/*', workingDir:'travel_blog')
				}    		                           
                	}
            	}
	}
}