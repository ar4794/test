properties([
    parameters([
        [$class: 'ChoiceParameter',
            choiceType: 'PT_SINGLE_SELECT',
            filterable: false,
            name: 'JobName',
            script: [$class: 'GroovyScript',
                fallbackScript: [ classpath: [], sandbox: true, script: 'return ["ERROR"]' ],
                script: [
                    classpath: [],
                    sandbox: true,
                    script: """
                        def build = Thread.currentThread().getName()
                        def regexp= ".+?/job/([^/]+)/.*"
                        def match = build  =~ regexp
                        def jobName = match[0][1]
                        def parts = jobName.split('_');
                        return [jobName]
                    """.stripIndent()
                ]
            ]
        ],
        string(description: 'Enter the GitHub Repo URL', name: 'Repo URL', defaultValue: ''),
        
[
            $class: 'CascadeChoiceParameter',
            choiceType: 'PT_SINGLE_SELECT',
            description: 'Select the Folder',
            referencedParameters: 'Repo URL,JobName',
            name: 'Folder Name',
            script: [
                $class: 'GroovyScript',
                fallbackScript: [
                    classpath: [],
                    sandbox: true,
                    script: 'return ["ERROR"]'
                ],
                script: [
                    classpath: [],
                    sandbox: true,
                    script:  
                    '''
                      import groovy.io.FileType
                      if (Repo URL.isEmpty() || Repo URL==""){
                        return ''
                      }
                      else
                      {
                        def parts = JobName.split('_');
                        if(parts.length > 2){
                            return [parts[2]]
                        } else {
                            def list = []
                            list.add('')
                            def dir = new File("/tmp/application/")
                            dir.eachFile (FileType.DIRECTORIES) { file ->
                                list << file.name
                            }
                            return list.sort() - 'group_vars' - null - ''
                        }
                      }
                    '''
                ]
            ]
        ],
    ])
])



pipeline {
    agent any

    environment {
        GIT_REPO_URL='https://github.com/ar4794/test'
    }
    
    stages {
        stage('checkout') {
            steps {
                     script {
                         checkout ($class: 'GitSCM', branches: [[name: '/dev' ]], userRemoteConfigs: [[url:  GIT_REPO_URL]])
                     }
                }
            }
        
        stage('List folders') {
            steps {
                script {
                    def folder= sh(script: 'ls */', returnStdout: true).trim()
                    echo "Folders in the reporsitory: ${folder}"
                }
            }
        }
    }
    }

