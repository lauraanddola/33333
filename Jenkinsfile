node {

  // customWorkspace '/Users/i356558/jenkins_mac_666'
   currentpath = pwd() ///Users/i356558/.jenkins/workspace/laura666
   a = currentpath.lastIndexOf('/')
   currentpath = currentpath.substring(0, a) ///Users/i356558/.jenkins/workspace
   datas = readYaml file: "${currentpath}/gen-cmdbserver.yml"
   //mapped to /Users/i356558/.jenkins/workspace/gen-cmdbserver.yml
   branch_abc =['a', 'b', 'c']
   repo_base=['https://github.com/lauraanddola/pipeline666.git']
   def  branch_from_file
}
pipeline {
    agent any

    parameters {
          booleanParam(defaultValue: true, description: '', name: 'userFlag')
        string(defaultValue: '', description: 'Git Branch to be used for Rundeck repo', name: 'GIT_BRANCH')
    }


    options {
           buildDiscarder(logRotator(artifactDaysToKeepStr: '7', daysToKeepStr: '90'))
    }

    stages {

        stage('test') {
            steps {
                sh 'echo "execute say hello script:"'
                sh 'rm -rf 0812_test branch_all.txt file_new.txt'
                sh 'mkdir 0812_test'
                sh 'cd 0812_test'
                git(
                    url: "${repo_base[0]}",

                    credentialsId: 'laura_test6',
                    branch: "master"
                )
                sh 'git branch -a'
                sh '''
                     pwd
                     prefix_head="remotes/origin/"
                     git branch -a > branch_all.txt
                     cat branch_all.txt
                     chmod 777 branch_all.txt

                     filename="branch_all.txt"
                     prefix_head="remotes/origin/"
                     file_new="branch_new.txt"
                                    
                     while read -r line; do
                        name="$line"
                        #echo "Name read from file - $name"
                        if [[ $line == *$prefix_head* ]]; then 
                        echo "Match is $line"; 
                        substr=`echo "$line" | sed 's/remotes//g' | sed 's/origin//g'`
                        echo "888: ${substr:15}"
                        echo "${substr:2}"  >> "${file_new}"
                        ls -lrt
                        pwd
                        fi
                     done < "$filename"  '''
                  script{
                    env.WORKSPACE = pwd() 
                      for (String branch_item : readFile('branch_new.txt').split("\r?\n")) {
      
                          println  "Start to sync ${branch_item}"
                          sh 'pwd'
                          sh "rm -rf ${branch_item}"
                          sh "mkdir ${branch_item}"
                          sh "cd ${branch_item}"
                          git(
                              url: 'https://github.com/lauraanddola/pipeline666.git',
                              credentialsId: 'laur_test6',
                              branch: "${branch_item}"
                           )
                          sh  'pwd'

                          sh  "git checkout ${branch_item}"
                          sh  'git fetch --tags'
                          sh  'git tag'
                          sh  'git branch -a'
                          sh  'git remote -v'
                          sh  'git remote rm origin'
                          sh  'git remote add origin https://github.com/lauraanddola/pipeline0813.git'
                          sh  'git remote -v'
                          withCredentials([sshUserPrivateKey(credentialsId: 'laura_test6', keyFileVariable: 'SSH_KEY')]) 
                          { 
                            sh("if [[ $(git ls-remote https://github.com/lauraanddola/pipeline0813.git) == *remote: Repository not found* ]]; then echo 'not found repo'; fi")
                           
                            sh("git push origin --all")
                            sh("git push --tags") }

                          sh  '''echo "End of sync ${branch_item}"'''
                          sh  'cd ..'
                          sh  ' pwd'
                          sh  'echo "3333"'

                       }
                   }                  

                  sh '''
                    cat ${file_new}
                   
                    cd ..
                  '''
                  
                sayHello("Laura")
                 script {

                 echo "Got version as ${datas.branch} "
                 echo "Current path as ${currentpath}"
             }
            }
        }
    }

    post {
        always {
            cleanWs()
        }
    }
}

def sayHello(String name = 'human') {
    echo "Hello, ${name}."
}

