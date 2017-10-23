#!/usr/bin/env groovy
pipeline {
    agent any
    stages {
        stage('Build') {
            steps { 
                echo 'Building..'
                echo 'Using Git..'
                checkout scm
                echo 'Composer Installing Packages..'
                sh "composer install"
                echo 'Copy example env to env'
                // need to copy from location that has the env stored.....
                sh 'cp .env.example .env'
                echo 'generate artisan project key'
                sh 'php artisan key:generate'
                echo "Work Space path? ${WORKSPACE}"
                echo "Params Person: ${params.PERSON}"
            }
        }
        stage('Test') {
            steps {
                echo 'Testing..'
                sh './vendor/bin/phpunit'
            }
        }
        stage('Deploy Staging') {
            steps {
                echo 'Testing..'
                sh './vendor/bin/phpunit'
            }
        }
        stage('Deploy Production') {
            steps {
                echo "Deploying...."
                wrap([$class: 'AnsiColorBuildWrapper', colorMapName: "xterm"]) {
                    ansiblePlaybook( 
                        playbook: './deploy/scripts/playbook.yml',
                        inventory: './deploy/scripts/playbook.yml',
                        credentialsId: 'sample-ssh-key',
                        extras: '-e parameter="some value"',
                        colorized: true) 
                }
            }
        }
    }
}