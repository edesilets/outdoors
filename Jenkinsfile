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
            }
        }
        stage('Test') {
            steps {
                echo 'Testing..'
                sh './vendor/bin/phpunit'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying....'
                ansible-playbook "./deploy/scripts/playbook.yml"
            }
        }
    }
}