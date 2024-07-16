`def call() {
    parallel {
        stage('Parallel Step 1') {
            steps {
                echo 'Executing Parallel Step 1...'
                // Add your commands for step 1 here
                sh './script1.sh'
            }
        }
        stage('Parallel Step 2') {
            steps {
                echo 'Executing Parallel Step 2...'
                // Add your commands for step 2 here
                sh './script2.sh'
            }
        }
    }
}
`


` pipeline {
    agent any

    stages {
        stage('Parallel Execution') {
            steps {
                script {
                    def parallelStages = load 'vars/parallelStages.groovy'
                    parallelStages()
                }
            }
        }
    }
} `

