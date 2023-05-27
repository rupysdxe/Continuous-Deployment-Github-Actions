# Continuous-Deployment-Github-Actions

**A. How to setup Continuous Deployment using Github Actions, SCP, and SSH for Spring Boot Application**

1. First we create a JAR file using gradle build.

   steps:

   ```yaml
   
    steps:
       - uses: actions/checkout@v2
       - name: Set up JDK 1.8
         uses: actions/setup-java@v1
         with:
           java-version: 1.8
       - name: Build with Gradle
         run: |
           cd project-1
           gradle build
           
   ```

   

2. Next using Secure Copy Protocol we transfer our jar to a Remote Server.

    ```yaml
    
    		- name: copy file via SCP
         uses: appleboy/scp-action@v0.1.4
         with:
           host: ${{ secrets.HOST }}
           username: ${{ secrets.USERNAME }}
           password: ${{ secrets.PASSWORD }}
           source: "project-1/build/libs/app-1.0.jar"
           target: SB/jars
           rm: true
           strip_components: 3
           
    ```

   

3. Next using Secure Socket Shell we log into our server, and run our spring boot inside docker container. See Dockerfile and docker-compose.yml

   ```yaml
   
    		- name: executing remote via SSH
         uses: appleboy/ssh-action@v0.1.10
         with:
           host: ${{ secrets.HOST }}
           username: ${{ secrets.USERNAME }}
           password: ${{ secrets.PASSWORD }}
           script: |
             cd SB
             sudo docker-compose down
             sudo docker-compose up --build -d
             
   ```

   

**B. Guide.txt**

See guide.txt
