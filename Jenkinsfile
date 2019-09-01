pipeline {
    agent { //这里使用docker镜像来启动maven,这样有个好处就是多个工程同时构建时不会出现冲突而失败
        docker {
            image 'maven:3.6-alpine' 
            cp '-v /home/jenkins/maven/settings.xml /usr/share/maven/ref/' //阿里镜像源加速
            args '-v /home/jenkins/mvnrepo:/root/.m2'  //持载jar信赖到本地缓存，减少重复下载量
        }
    }
    stages {
        stage('Pull Git Demo') {
            steps{
                //拉取代码
            	git 'https://github.com/hellxz/springboot-demo1.git'
            }
        }
        stage('Build') { 
            steps {
                //执行构建命令
                sh 'mvn -B -DskipTests clean package' 
            }
        }
    }
}
