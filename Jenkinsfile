node {

  // customWorkspace '/Users/i356558/jenkins_mac_666'
   currentpath = pwd() ///Users/i356558/.jenkins/workspace/laura666
   a = currentpath.lastIndexOf('/')
   currentpath = currentpath.substring(0, a) ///Users/i356558/.jenkins/workspace
   datas = readYaml file: "${currentpath}/gen-cmdbserver.yml"
   //mapped to /Users/i356558/.jenkins/workspace/gen-cmdbserver.yml
   branch_abc =['a', 'b', 'c']
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
            environment { 
                GIT_AUTH = credentials('lauraanddora123') 
            }
            steps {
                sh 'echo "execute say hello script:"'
                sh 'rm -rf 0812_test branch_all.txt'
                sh 'mkdir 0812_test'
                sh 'cd 0812_test'
                git(
                    url: 'https://github.com/lauraanddola/pipeline666.git',
                    credentialsId: 'lauraanddora123',
                    branch: "master"
                )
                sh 'git branch -a'
                sh '''
                     unset branch_array
                     pwd
                     prefix_head="remotes/origin/"
                     git branch -a > branch_all.txt
                     cat branch_all.txt
                     chmod 777 branch_all.txt

                     filename="branch_all.txt"
                     prefix_head="remotes/origin/"
                     unset branch_array
                     
                     while read -r line; do
                        name="$line"
                        #echo "Name read from file - $name"
                        if [[ $line == *$prefix_head* ]]; then 
                        echo "Match is $line"; 
                        substr=`echo "$line" | sed 's/remotes//g' | sed 's/origin//g'`
                        echo "888: $substr"
                        branch_array+=("${substr:2}")
                        ${branch_abc} = ${branch_abc} +  "${substr:2}"

                        fi
                     done < "$filename"  '''
                  branch_abc = sh (script : 'cat branch_array', returnStdout: true)
                  println "888888 ${branch_abc}"
                  sh '''
                   
                   
                    cd ..
                    echo "1111"
for branch_item in ${branch_array[*]}
do
  echo "Start to sync $branch_item"
  pwd
  rm -rf $branch_item
  git clone https://github.com/lauraanddola/pipeline666.git $branch_item
  cd $branch_item
  pwd

  git checkout $branch_item
  git fetch --tags
  git tag
  git branch -a
  git remote -v
  git remote rm origin
  git remote add origin https://github.com/lauraanddola/pipeline0812.git
  git remote -v
  git push origin --all
  git push --tags
  echo "End of sync $branch_item"
  cd ..
  pwd
  echo "3333"
done
                 

                    echo "2222" '''
                println "6666666 ${branch_abc}"
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

