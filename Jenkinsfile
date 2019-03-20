pipeline {
    agent none
    stages {
        stage('Build') {
            agent {
                docker {
                    image 'python:2-alpine'
                }
            }
            steps {
                sh 'python -m py_compile sources/add2vals.py sources/calc.py'
submitJUnitTestResultsToqTest([apiKey: '0afd1028-4eb7-46a2-a3c9-8144995d1cdb', containerID: 347800, containerType: 'release', createTestCaseForEachJUnitTestClass: false, createTestCaseForEachJUnitTestMethod: true, overwriteExistingTestSteps: true, parseTestResultsFromTestingTools: false, projectID: 84112, qtestURL: 'https://confirmation.qtestnet.com', submitToAReleaseAsSettingFromQtest: true, submitToExistingContainer: false, utilizeTestResultsFromCITool: true])                
            }
        }
        stage('Test') {
            agent {
                docker {
                    image 'qnib/pytest'
                }
            }
            steps {
                sh 'py.test --verbose --junit-xml test-reports/results.xml sources/test_calc.py'
            }
            post {
                always {
                    junit 'test-reports/results.xml'
                }
            }
        }
        stage('Deliver') {
            agent {
                docker {
                    image 'cdrx/pyinstaller-linux:python2'
                }
            }
            steps {
                sh 'pyinstaller --onefile sources/add2vals.py'
            }
            post {
                success {
                    archiveArtifacts 'dist/add2vals'
                }
            }
        }
    }
}
