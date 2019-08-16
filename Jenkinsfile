node {

   branch_abc =['a', 'b', 'c']
   repo_base=['https://github.com/lauraanddola/pipeline666.git']
   target_repo_list = ['https://github.com/lauraanddola/22222.git']
  
   source_repo_list = ['https://github.com/lauraanddola/helmRepo.git']
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
                checkRepExisted(source_repo_list)
               script{
                sh 'rm -rf *'
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
                     cpToRemote()
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

def cpToRemote(){
 for (int i =0; i < source_repo_list.size(); i++){
                      for (String branch_item : readFile('branch_new.txt').split("\r?\n")) {
                          sh 'git remote rm origin'
                          println  "Start to sync ${branch_item}"
                          sh 'pwd'
                          sh "ls -lrt"
                          sh "rm -rf *"
                          git(
                              url: "${source_repo_list[i]}",
                              credentialsId: 'laura_test6',
                              branch: "${branch_item}"
                           )

                          sh  "git checkout ${branch_item}"
                          sh  'git fetch --tags'
                          sh  'git tag'
                          sh  'git branch -a'
                          sh  'git remote -v'
                          sh  'git remote rm origin'
                          sh  "git remote add origin ${target_repo_list[i]}"
                          sh  'git remote -v'
                          sh  "git push origin ${branch_item} --force"
                          //sh  'git push origin --all'
                          sh  "git push --tags"

                          sh  '''echo "End of sync ${branch_item}"'''
                          sh  ' pwd'
                          sh  'echo "3333"'
                         }
                     }
}

def checkRepExisted(list) { 

//for (int i =0; i < list.size(); i++){
  //apple = "${list[i]}"
  //println "apple is ${apple}"
  withCredentials([sshUserPrivateKey(credentialsId: 'laura_test6', keyFileVariable: 'SSH_KEY_FOR_ABC')]) {
                            sh("echo $SSH_KEY_FOR_ABC")
                            sh("rm -rf repo_result.txt")
                            sh("set -e")
                            sh("EXIT_CODE=0")
                           // sh("echo ${list[i]}")
                            
                           // sh('''echo '${list[i]}' | git ls-remote  &>repo_result.txt || EXIT_CODE=$?''')
                         //   sh('''echo $apple | git ls-remote  &>repo_result.txt || EXIT_CODE=$?''')
                       //     sh('echo ${apple} | git ls-remote  &>repo_result.txt || EXIT_CODE=$?')
                            //sh("echo ${apple} | git ls-remote  &>repo_result.txt || EXIT_CODE=$?")
                     //       sh('echo "${apple}" | git ls-remote  &>repo_result.txt || EXIT_CODE=$?') 
                            sh('git ls-remote https://github.com/lauraanddola/22222.git &>repo_result.txt || EXIT_CODE=$?')
                            sh("cat repo_result.txt")
                            String repo_isFound= readFile('repo_result.txt')
                            println "repo result is : ${repo_isFound}"
                            if (repo_isFound.contains("not found")) {
                               println "666666 repo not found"
                               withCredentials([string(credentialsId: 'laura_test', variable: 'SECRET')]) {
                                    sh('''curl -H "Authorization: token ${SECRET}" --data '{"name":"22222"}' https://api.github.com/user/repos''')
sh("ls -lrt")
sh("rm -rf *")
sh("mkdir repo_temp")
sh("ls -lrt")
sh("cd repo_temp")
sh("git branch")
sh('git init')
sh('git checkout master')
//sh('touch README.md')
sh('git add .')
sh('git commit -m "first commit"')
sh('git remote -v')
sh('git remote rm origin')
sh('git remote add origin https://github.com/lauraanddola/22222.git')
sh('git push origin master --force')
sh('cd ..')
sh('rm -rf repo_temp')   
                                 
                              }
                            }
  }
}
//}
def sayHello(String name = 'human') {
    echo "Hello, ${name}."
}

