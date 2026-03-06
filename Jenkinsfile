node('java&&dev') {
  input 
    message: 'Introduce un número de tarjeta:', 
    parameters: [string(defaultValue: '4111111111111111', description: 'Número de tarjeta por defecto', name: 'cardNumber')]
  
  stage('Get git repo') {
    // en las scripted se debe indicar para que se traiga el resto del repositorio
    git branch: 'main', url: 'https://github.com/framvaq/apasot-jenkins-scripted-params'
  }

  stage('Compile') {
    echo 'Compile'
    sh 'mvn compile'
  }

  stage('Test') {
    echo 'Compile'
    sh 'mvn test'
  }

  stage('Run') {
    echo 'Run'
    sh "mvn exec:java -Dexec.mainClass='com.apasoft.CardProcessor' -Dexec.args='${cardNumber}'"
    // Se guarda en el stash para enviarlo al otro nodo
    stash includes: 'target/**', name: 'target-jar'
  }
}

node('java&&pro') {
  stage('Deploy') {
    // Extraer el artifact
    unstash 'target-jar'
    echo 'Deploying...'
    // Copiar el artifact a la carpeta correcta
    sh 'rm -rf /home/jenkins/app/'
    sh 'mkdir -p /home/jenkins/app/'
    sh 'cp -r target/* /home/jenkins/app/'
    echo 'Deployment completed'
  }
}
