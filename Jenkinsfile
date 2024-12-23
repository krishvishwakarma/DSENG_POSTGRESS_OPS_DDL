pipeline {
    agent any
    environment {
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
                        // Any feature branch (e.g., feature-1, feature-2, etc.) uses the dev database config
                        flywayConfigFile = 'flyway_dev.conf'
                    } else if (env.BRANCH_NAME == 'dev') {
                        // The dev branch uses the dev database config
                        flywayConfigFile = 'flyway_dev.conf'
                    } else if (env.BRANCH_NAME == 'qa') {
                        // The qa branch uses the qa database config
                        flywayConfigFile = 'flyway_qa.conf'
                    } else if (env.BRANCH_NAME == 'main') {
                        // The main branch uses the prod database config
                        flywayConfigFile = 'flyway_prod.conf'
                    }
                    echo "flyway -v"
                    // Run Flyway migration with the selected config file
                    echo "Running Flyway migration for branch ${env.BRANCH_NAME} using config ${flywayConfigFile}"
                    sh "${FLYWAY_CMD} -configFiles=${FLY_CONFIG_PATH}/${flywayConfigFile} migrate"
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
