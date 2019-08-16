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
   targetRepoList = ['https://github.com/lauraanddola/11111.git', 'https://github.com/lauraanddola/22222.git']
}
pipeline {
    agent any
    environment {
        USER_CREDENTIALS = credentials('laura_test6')
    }

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
                checkRepExisted(targetRepoList)
                sh "echo hihihi1111111"
                sh "echo $USER_CREDENTIALS_USR"
                
                sh 'echo "execute say hello script:"'
                sh 'rm -rf 0812_test branch_all.txt file_new.txt'
                sh 'mkdir 0812_test&&chmod 777 0812_test'
                sh 'cd 0812_test'
                sh 'git  remote rm  origin'
                git(
                    //url: "${repo_base[0]}",
                    url: "https://github.com/lauraanddola/helmRepo.git",
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
                          sh "rm -rf repo_temp"
                          sh "mkdir repo_temp"
                          sh "ls -lrt"
                          sh "cd repo_temp"
                          sh 'pwd'
                          sh "ls -lrt"
                          git(
                              url: 'https://github.com/lauraanddola/helmRepo.git',
                              credentialsId: 'laura_test6',
                              branch: "${branch_item}"
                           )
                          sh  'pwd'

                          sh  "git checkout ${branch_item}"
                          sh  'git fetch --tags'
                          sh  'git tag'
                          sh  'git branch -a'
                          sh  'git remote -v'
                          sh  'git remote rm origin'
                          sh  'git remote add origin https://github.com/lauraanddola/22222.git'
                          sh  'git remote -v'
                          //sh  "git push origin ${branch_item} --force"                         
                          sh  'git push origin --all'
                          sh  "git push --tags"

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


def checkRepExisted(list) { 

for (int i =0; i < list.size(); i++){
  apple = "${list[i]}"
  println "apple is ${apple}"
  withCredentials([sshUserPrivateKey(credentialsId: 'laura_test6', keyFileVariable: 'SSH_KEY_FOR_ABC')]) {
                            sh("echo $SSH_KEY_FOR_ABC")
                            sh("rm -rf repo_result.txt")
                            sh("set -e")
                            sh("EXIT_CODE=0")
                            sh("echo ${list[i]}")
                            
                            sh('git ls-remote ${apple} &>repo_result.txt || EXIT_CODE=$?')
                            //sh('git ls-remote https://github.com/lauraanddola/22222.git &>repo_result.txt || EXIT_CODE=$?')
                            sh('git ls-remote ${list[i]} &>repo_result.txt || EXIT_CODE=$?')
                            sh("cat repo_result.txt")
                            String repo_isFound= readFile('repo_result.txt')
                            println "repo result is : ${repo_isFound}"
                            if (repo_isFound.contains("not found")) {
                               println "666666 repo not found"
                               withCredentials([string(credentialsId: 'laura_test', variable: 'SECRET')]) {
                                    sh('''curl -H "Authorization: token ${SECRET}" --data '{"name":"${list[i]}"}' https://api.github.com/user/repos''')
sh("ls -lrt")
sh("rm -rf *")
sh("mkdir repo_temp")
sh("ls -lrt")
sh("cd repo_temp")
sh("git branch")
sh('git init')
sh('git checkout master')
sh('touch README.md')
sh('git add .')
sh('git commit -m "first commit"')
sh('git remote -v')
sh('git remote rm origin')
//sh('git remote add origin https://github.com/lauraanddola/22222.git')
sh("git remote add origin ${list[i]}")
sh('git push origin master --force')
sh('cd ..')
sh('rm -rf repo_temp')   
                                 
                              }
                            }
  }
}
}
def sayHello(String name = 'human') {
    echo "Hello, ${name}."
}

