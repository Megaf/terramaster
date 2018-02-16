pipeline {
  agent any
  environment{
      def files = findFiles(glob: '**/terramaster.jar')[0].getPath()
  }
  stages {
    stage( 'build' ) {
      steps{
        withEnv(["JAVA_HOME=${ tool 'jdk1.8.0_121' }"]) {
          withAnt('installation' : 'apache-ant-1.10.1') {
            bat "ant default"
          }
        }  
        archiveArtifacts '*terramaster*.jar'    
      }
    }
    
    stage( 'deploy' ) {
      steps{
        //git credentialsId: 'github', url: 'https://github.com/Portree-Kid/terramaster.git'
        
        withEnv(["SID=${env.sid}"]) {
           bat 'git commit -am "Version "'
           bat 'git push origin master'
           echo """${files}"""
           bat 'C:\\Users\\keith.paterson\\go\\bin\\github-release release -s %SID% -u Portree-Kid -r terramaster -t 1.10'
           bat """C:\\Users\\keith.paterson\\go\\bin\\github-release upload -s %SID% -u Portree-Kid -r terramaster -t 1.10 -n 1.10 -f ${files}"""
        }
        archiveArtifacts '*terramaster*.jar'
      }
    }
  }
}
