node {

  // customWorkspace '/Users/i356558/jenkins_mac_666'
   currentpath = pwd() ///Users/i356558/.jenkins/workspace/laura666
   a = currentpath.lastIndexOf('/')
   currentpath = currentpath.substring(0, a) ///Users/i356558/.jenkins/workspace
   datas = readYaml file: "${currentpath}/gen-cmdbserver.yml"
   //mapped to /Users/i356558/.jenkins/workspace/gen-cmdbserver.yml
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
                sh 'rm -rf 0812_test branch_all.txt'
                sh 'git clone https://github.com/lauraanddola/pipeline666.git 0812_test'
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
      
                        fi
                     done < "$filename"



                   for branch_item in ${branch_array[*]}
                   do
                        echo "Start to sync $branch_item"
                         git branch
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

                   done

                    echo "2222" '''

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

