# Docker Tasks

* Build a custom nginx container image and push to a private DockerHub registry.<br /> <br /><br />


1. Created a Dockerfile that copies the official Nginx image and copies an HTML file into the html directory. <br /> 

  <img width="876" height="184" alt="1" src="https://github.com/user-attachments/assets/304f8146-1ab9-4114-bc01-52dee6361440" />    <br /> <br />

    
2. Used the build command to build the image with a name matching the private Docker Hub repository I just created, using a dynamic tag generated from the date command.    

<img width="856" height="280" alt="2" src="https://github.com/user-attachments/assets/f20358a4-f73b-4b12-abf3-2270c55edb61" />
<img width="854" height="120" alt="Screenshot_4" src="https://github.com/user-attachments/assets/ee1dcd6d-e562-404e-ba44-285ceee7346b" /> <br /> <br />


3. Pushed the image to the private Docker Hub repository and confirmed that it exists. <br />
<img width="1592" height="516" alt="Screenshot_5" src="https://github.com/user-attachments/assets/0e1a955c-d904-4e87-a382-06dc2ec05cbf" />  <br /> <br />


* Docker - Networking Task
