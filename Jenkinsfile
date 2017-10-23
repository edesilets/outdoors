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
            post { 
                //  Other flags always, changed, failure, success, unstable, and aborted
                always { 
                    echo 'Testing is so messy! Time to clean up.'
                }
            }
        }
        stage('Deploy Staging') {
            when {
                // Only run Deploy Production if branch is staging
                expression { env.BRANCH_NAME == 'messing-with-jenkins' }
            }
            steps {
                echo 'O ya deploy to staging!'
            }
        }
        stage('Deploy Production') {
            when {
                // Only run Deploy Production if branch is master
                expression { env.BRANCH_NAME == 'master' }
            }
            steps {
                echo "Deploying....."
                wrap([$class: 'AnsiColorBuildWrapper', colorMapName: "xterm"]) {
                    ansiblePlaybook( 
                        playbook: './deploy/scripts/playbook.yml',
                        inventory: './deploy/scripts/hosts',
                        credentialsId: 'sample-ssh-key',
                        extras: '-e parameter="some value"',
                        colorized: true) 
                }
            }
        }
    }
}
