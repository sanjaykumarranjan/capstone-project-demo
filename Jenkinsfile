node{ 
    stage('code checkout'){
        git 'https://github.com/sanjaykumarranjan/capstone-project-demo.git'
    }
    
    stage('code build'){
       sh 'mvn clean package' 
    }
    stage('code containerize'){
       sh 'docker build -t sanjaykumarranjan/insure-me:1.0 .'
    }
    
    
    stage('push to dockerhub'){
     withCredentials([string(credentialsId: 'dockerPwd', variable: 'dockerHubPwd')]) {
        sh "docker login -u sanjaykumarranjan -p ${dockerHubPwd}"
        sh 'docker push sanjaykumarranjan/insure-me:1.0'
    }
    }
   
    stage('code deploy'){
        ansiblePlaybook become: true, credentialsId: 'ansible-ssh-jenkins-key', disableHostKeyChecking: true, installation: 'ansible', inventory: '/etc/ansible/hosts', playbook: 'configure-test-server.yml', vaultTmpPath: ''
    }
}
