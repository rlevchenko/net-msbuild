#!/usr/bin/env groovy
pipeline {
environment {
        project_path = "${Workspace}/helloworld/"
		branch = "${BRANCH_NAME}"
    }
agent any
    stages {
        
        // project restoration
        stage('restore') {
            steps {
                dir ("$project_path") {
                script {
				if (branch == "scripts") {
                        bat "msbuild ./scripts/msbuild-nuget.csproj /t:nugetrestore"
                    } 
				else if (branch == "feature") {
                        bat "msbuild ./scripts/msbuild.csproj /t:restore"
                    } //elseif
				else {echo "no actions needed"}
                } //scripts
                }	//dir
            }	//steps
        } //stage
    
        // project building    
        stage ('build') {
		        steps {
                dir ("$project_path") {
                script {
				if (branch == "scripts") {
                    bat "msbuild ./scripts/msbuild-nuget.csproj"
				}
				else if (branch == "feature") {
				    bat "msbuild ./scripts/msbuild.csproj"
				} //elseif
				else {echo "no actions needed"}
                } //script
             } //dir
            } //steps
        } //stage
            
        // project packaging
        stage('pack') { 
            steps {
                dir ("$project_path") {
                script {
				if (branch == "scripts") {
                    bat "msbuild ./scripts/msbuild-nuget.csproj /t:nugetpack /p:AssemblyName=HelloWorld;PackageVersion=1.0.0.%BUILD_NUMBER%"
                }
				else if (branch == "feature") {
				    bat "msbuild ./scripts/msbuild.csproj /t:pack /p:AssemblyName=HelloWorld;PackageVersion=1.0.0.%BUILD_NUMBER%"
				} //elseif
				else {echo "no actions needed"}
            }	//script
          }	//dir
        }	//steps
    } //stage
       
	   stage('copy') { 
            steps {
                dir ("$project_path") {
                script {
				   bat "msbuild ./scripts/msbuild.csproj /t:copybuild"   
            }	//script
          }	//dir
        }	//steps
    } //stage
	
	stage('clean') { 
        steps {
                dir ("$project_path") {
                script {
				   bat "msbuild ./scripts/msbuild.csproj /t:clean"   
            }	//script
          }	//dir
        }	//steps
    } //stage

    // debug information
    stage ('debug') { 
        steps {
                dir ("$project_path") {
                script {
                bat "msbuild ./scripts/msbuild-nuget.csproj /t:info"
                }
            }	//dir
          }	  //steps
        }	//stage
  } //stages
} //pipeline