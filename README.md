pipeline {

    agent {

        docker {

            image "python:3.8"

        }

    }

    environment {

       HOME = "${env.WORKSPACE}@tmp"

       BIN_PATH = "${HOME}/.local/bin/"
    }

    stages {

        stage('Git clone') {

           steps {

              git changelog: false, credentialsId: 'gitlab', http://gitlab.devops.ru/Ibrahiimm/jenkins-devops4.git'

           }

        }

        stage("Prepare") {

           steps {

                 sh 'python --version'

                 sh 'pip install virtualenv'

                 sh "${BIN_PATH}virtualenv venv"

                 sh 'bash -c "source venv/bin/activate"'

                 sh "pip install -r requirement.txt"
           }

        }

        stage("Test") {

            steps{

                sh "python -m unittest discover -s "./tests" -p "*_test.py"'

                sh "${BIN_PATH}flake8 ."

                sh "${BIN_PATH}mypy ."

            {

        {

    {

{

