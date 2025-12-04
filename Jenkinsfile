pipeline { 
    agent any
    
    tools { 
        maven 'Maven' 
    } 
    
    options { 
        timeout(time: 10, unit: 'MINUTES') 
    } 
    
    environment { 
        APP_ENV  = "DEV" 
    } 
    
   stages {
        stage('Checkout') {
            steps {
                git(
                    branch: 'main',
                    url: 'https://github.com/Hadirgh/pipeline.git',
                    credentialsId: 'jenkinsToken'
                )
           
        
        stage('Code Build') { 
            steps { 
                 sh 'mvn install -Dmaven.test.skip=true' 
            } 
        } 
    } 
    
    post { 
        always {                                                              
             echo "======always======"  
        } 
        success { 
             echo "=====pipeline executed successfully =====" 
        } 
        failure { 
             echo "======pipeline execution failed======" 
        } 
    } 
}
