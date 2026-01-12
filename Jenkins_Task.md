# Jenkins Task

* ## Deploy jenkins on a server baremetal way (not using docker).<br /> <br />

1. installed jenkins and moved forward with the steps untitil reached the unlock Jenkins step and extracted the administrator password as below:<br />

<img width="1388" height="806" alt="Screenshot_1" src="https://github.com/user-attachments/assets/952d7154-45f3-4976-954f-f705e4efd9cf" />

<br /> <br />

* ## NO hardcoded credentials in the pipeline.
  
2. The following credentials were securely added to Jenkins using the Credentials Manager:

<img width="1754" height="389" alt="Screenshot_3" src="https://github.com/user-attachments/assets/6765820a-3171-47af-98ab-92241e3798e0" />
<br /> <br />

3. Forked a repository containing a Dockerfile into a private GitHub repository
<img width="1665" height="572" alt="image" src="https://github.com/user-attachments/assets/1de97365-2251-4127-807b-de736d174729" />
<br /> <br />

* ## Build a pipeline that builds container image and pushes it to private dokcer registry,
* ## Code should be fetched from a private github repository
* ## Developer want to be able to choose the build branch using a dropdown menu
* ## Developer wants to be able to choose a custom image tag and default to the sha256 commit hash if not tag was specified
<br />


<img width="1355" height="550" alt="image" src="https://github.com/user-attachments/assets/f1cd3ac5-174c-4587-b93d-9b82755bca9e" />


Code Used: 
```
node {
    def app
}
pipeline {
    agent any
    parameters {
        choice(name: 'Branch', choices: ['main', 'dev'], description: 'Choose a Branch')
        string(name: 'Tag', defaultValue: '', description: 'Image Tag.')
    }
    stages {
        stage('Fetch') {
            steps{
                git branch: "${params.Branch}", credentialsId: 'Github_CREDS ', url: 'https://github.com/Anas-Moeen/privet-jenk'
                script{
                    env.CommitHash = sh(script: 'git rev-parse --short HEAD', returnStdout: true).trim()
                }
            }
        }
        stage('Build'){
            steps{
                script{
                    env.ImageTag = params.Tag?: env.CommitHash
                    
                    app = docker.build("anasmo1/pr:${ImageTag}")   
                }
                echo "Image has been built successfully: ${app.imageName()}"
            }
        }
        stage('Push Image'){
            steps{
                script{
                    docker.withRegistry('', 'Docker_CREDs'){
                        app.push()            
                    }
                }
            }
        }
    }
}


```
<br /> <br />

During the first build faced an authorization issue so added the jenkins user to docker group:

<img width="692" height="122" alt="Screenshot_5" src="https://github.com/user-attachments/assets/6cf9631e-ed2e-46b7-940e-b1aafd8b8691" />
<br />

<img width="1401" height="438" alt="Screenshot_16" src="https://github.com/user-attachments/assets/13ef2668-6d58-4bac-8921-c4fcc2a2fee4" />
<br />
<img width="925" height="378" alt="Screenshot_17" src="https://github.com/user-attachments/assets/abe986d2-6329-49c2-9698-84d3c20350ff" />
<br /><br />

* ## Build a pipeline that deploys the application to your server.

<img width="1406" height="618" alt="Screenshot_12" src="https://github.com/user-attachments/assets/e62e34a4-afe2-408d-b4aa-dbbe5c116f3f" />

Code Used:
```
node {
    stage('run'){
        withDockerRegistry(credentialsId: 'Docker_CREDs') {
        sh '''docker run -d anasmo1/pr '''
            }     
    }
}
```

* ## Build a cronjob pipeline that runs docker system prune whenever we have more than 10 dangling images.

  Created a new pipeline that runs every day at 1 am to run the following command if images count exceeds 10: `docker image prune`
  <br />
<img width="1417" height="445" alt="image" src="https://github.com/user-attachments/assets/454951cf-c344-491c-8226-335f0490b9e0" />
<br />

<img width="1326" height="530" alt="Screenshot_15" src="https://github.com/user-attachments/assets/799be2d8-a5c1-4f9f-8c4e-c7c6197a7e95" />

<br />
Code Used:

```
node {
    stage('dangling Prune'){
        sh '''
        if ((docker images -f "dangling=true" -q | wc -l) > 10); then
            docker image prune
            echo 'Done'
        fi
        '''
            }     
}
```

<br /><br />

<img width="1650" height="406" alt="image" src="https://github.com/user-attachments/assets/0ec3e82e-7742-48d3-875d-593de4546b17" />

