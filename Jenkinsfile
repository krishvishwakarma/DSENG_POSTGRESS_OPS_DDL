pipeline {
    agent any
    environment {
        // Correctly using forward slashes for the file path
        FLYWAY_CMD = "C:/Program Files/flyway/flyway-11.1.0/flyway"
        FLY_CONFIG_PATH = "C:/Program Files/flyway/flyway-11.1.0/flyway/conf"
    }
    stages {
        stage('Checkout') {
            steps {
                // Check out the code from the correct branch
                git branch: "${BRANCH_NAME}", url: 'https://github.com/krishvishwakarma/DSENG_POSTGRESS_OPS_DDL.git'
            }
        }
        stage('Run Flyway Migration') {
            steps {
                script {
                    // Set the Flyway configuration file based on the branch
                    def flywayConfigFile = ''
                    if (env.BRANCH_NAME.startsWith('feature')) {
                        flywayConfigFile = 'flyway_dev.conf'
                    } else if (env.BRANCH_NAME == 'dev') {
                        flywayConfigFile = 'flyway_dev.conf'
                    } else if (env.BRANCH_NAME == 'qa') {
                        flywayConfigFile = 'flyway_qa.conf'
                    } else if (env.BRANCH_NAME == 'main') {
                        flywayConfigFile = 'flyway_prod.conf'
                    }
                    
                    // Run the Flyway migration command
                    echo "Running Flyway migration for branch ${env.BRANCH_NAME} using config ${flywayConfigFile}"
                    sh "\"${FLYWAY_CMD}\" -configFiles=\"${FLY_CONFIG_PATH}/${flywayConfigFile}\" migrate"
                }
            }
        }
    }
    post {
        success {
            echo "Flyway migration applied successfully."
        }
        failure {
            echo "Flyway migration failed."
        }
    }
}
