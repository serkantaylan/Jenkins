node {
    gitProjectURL = 'https://github.com/serkantaylan/Jenkins.git'
    containerName = 'simple-java'
    latestTag = 'latest'
    branchName = 'main'
    dockerProjectURL = 'https://github.com/serkantaylan/Jenkins.git'
    dockerBranchName = 'main'
    dockerProjectName = 'Dockerfile'
    sonarProjectKey = 'simple-java'
    sonarLoginToken = 'sqp_f92527e047bf2dcfaf5be5794def5f70cedff3e5'
    sonarHostUrl = 'http://192.168.27.45:9000'
    stage('Get Dockerfile repository') {
        fileOperations([folderCreateOperation('mytmp')])
        dir('mytmp') {
            checkout([$class: 'GitSCM', branches: [[name: "${dockerBranchName}"]], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[ url: "${dockerProjectURL}"]]])
        }
    stage('Clone repository') {
        checkout([$class: 'GitSCM', branches: [[name: "${branchName}"]], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[ url: "${gitProjectURL}"]]])        
    }        
        sh("cp mytmp/${dockerProjectName} .")
        
        fileOperations([folderDeleteOperation('mytmp')])
    }
    stage('Build & register') {
         def dockerHome = tool 'docker'
         env.PATH = "${dockerHome}/bin:${env.PATH}"
            
        
            docker.withRegistry('http://192.168.27.45:5000') {
                def customImage = docker.build("${containerName}")
                customImage.push("${latestTag}")
            
        }
    }
}
