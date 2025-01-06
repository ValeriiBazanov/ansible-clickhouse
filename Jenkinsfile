pipeline {
    agent any

    environment {
        REPO_DIR   = './'
        VENV_DIR   = 'my_env'
    }

    stages {
        stage('Install dependencies') {
            steps {
                dir(REPO_DIR) {
                    script {
                        echo "Activating virtual environment and installing requirements..."
                        sh """
                        python3 -m venv ${VENV_DIR}
                        source ${VENV_DIR}/bin/activate
                        python3 -m pip install wheel
                        python3 -m pip install molecule docker
                        """
                    }
                }
            }
        }

        stage('Molecule setup') {
            steps {
                dir(REPO_DIR) {
                    script {
                        if (fileExists("molecule/default")) {
                            echo "Scenario directory molecule/default exists. Removing it..."
                            sh 'rm -rf molecule/default'
                        }
                        echo "Initializing new Molecule scenario..."
                        sh """
                        source ${VENV_DIR}/bin/activate
                        molecule init scenario default
                        """
                    }
                }
            }
        }

        stage('Molecule test') {
            steps {
                dir(REPO_DIR) {
                    script {
                        echo "Executing Molecule tests..."
                        sh """
                        source ${VENV_DIR}/bin/activate
                        molecule test
                        """
                    }
                }
            }
        }
    }

    post {
        always {
            cleanWs()
            echo "Pipeline execution finished."
        }
        success {
            echo "Pipeline successfully executed!"
        }
        failure {
            echo "Pipeline failed!"
        }
    }
}
