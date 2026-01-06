# Docker Tasks

* ## Build a custom nginx container image and push to a private DockerHub registry.<br /> <br /><br />


1. Created a Dockerfile that copies the official Nginx image and copies an HTML file into the html directory. <br /> 

  <img width="876" height="184" alt="1" src="https://github.com/user-attachments/assets/304f8146-1ab9-4114-bc01-52dee6361440" />    <br /> <br />

    
2. Used the build command to build the image with a name matching the private Docker Hub repository I just created, using a dynamic tag generated from the date command.    

<img width="856" height="280" alt="2" src="https://github.com/user-attachments/assets/f20358a4-f73b-4b12-abf3-2270c55edb61" />
<img width="854" height="120" alt="Screenshot_4" src="https://github.com/user-attachments/assets/ee1dcd6d-e562-404e-ba44-285ceee7346b" /> 

Command Used: `docker build -t anasmo1/pr:$(date +%Y%m%d-%H%M%S) .`
<br /> <br />


3. Pushed the image to the private Docker Hub repository and confirmed that it exists. <br />
<img width="1592" height="516" alt="Screenshot_5" src="https://github.com/user-attachments/assets/0e1a955c-d904-4e87-a382-06dc2ec05cbf" />

Command Used: `docker push anasmo1/pr:20260105-230421`
<br /> <br />


* ## Docker - Networking Task

1. Created a custom Network `app-net` with the followign subnet: 180.18.0.0/16.


<img width="1011" height="172" alt="Screenshot_6" src="https://github.com/user-attachments/assets/44e01e52-1836-4a69-9e51-b2744cb74177" />

Command Used: `docker network create --driver=bridge --subnet=180.18.0.0/16 app-net`

 <br />
 
 2. ran hashicorp/http-echo as a container on the `app-net` bridge netwok.

<img width="1017" height="390" alt="Screenshot_7" src="https://github.com/user-attachments/assets/25fec049-8f82-40af-a665-c086bf1b06ec" />

Command Used: `docker run --network=app-net hashicorp/http-echo`


 <br />

 
 3. Checked the container IP and it's DNS name.


<img width="914" height="270" alt="Screenshot_8" src="https://github.com/user-attachments/assets/d3db6d63-bd49-45af-b47e-dbf73a3c27af" />

Command Used: `docker inspect network app-net`


 <br />

4. ran curlimages/curl:8.5.0 as a container on the `app-net` bridge netwok and tried to curl the other container using the IP address and connected successfuly.

<img width="1015" height="788" alt="Screenshot_9" src="https://github.com/user-attachments/assets/ec89ce13-fb47-451f-8472-abf2ad14df2a" />

Command Used: `docker run --network=app-net --rm curlimages/curl:8.5.0 -L -v http://180.18.0.2:5678`


 <br />

 5.  ran curlimages/curl:8.5.0 as a container on the `app-net` bridge netwok and tried to curl the other container using the DNS name and connected successfuly.

  <img width="1165" height="811" alt="Screenshot_10" src="https://github.com/user-attachments/assets/41a08f36-13cb-4c49-91fc-022e5232e0b8" />

Command Used: `docker run --network=app-net --rm curlimages/curl:8.5.0 -L -v http://trusting_sinoussi:5678`


 <br />

 6. tried to reach the `http-echo` container from the docker host, connected using the IP but failed using the DNS name.

<img width="1512" height="622" alt="Screenshot_11" src="https://github.com/user-attachments/assets/f808983a-30c0-4ce5-b132-b6e34362340a" />

Commands Used: `curl http://180.18.0.2:5678`  `curl http://trusting_sinoussi:5678`

 <br />


