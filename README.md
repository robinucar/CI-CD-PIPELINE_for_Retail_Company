## Use Case

ABC Technologies is a prominent e-commerce platform that has recently obtained a sizable brick-and-mortar retail establishment. This retail entity boasts numerous physical stores worldwide but has adhered to traditional methods of growth and expansion. Consequently, it has incurred significant losses and is grappling with the subsequent hurdles.

- Low available.
- Low scalable.
- Low performance.
- Hard to build maintain.

Developing and deploying consume significant time and resources. ABC Technologies aims to leverage data from various storage systems to drive analytics and predict the company's growth and sales trajectory. Initially, ABC will focus on creating servlets for product addition and displaying product details. It's essential to include servlet dependencies necessary for compiling the servlets. Additionally, an HTML page will be developed for adding products.

The team employs Git for source code management. ABC has opted to adopt the DevOps model. Once the source code is hosted on GitHub, integration with Jenkins will enable continuous build generation for seamless continuous delivery. Furthermore, integration with Ansible and Kubernetes is vital for efficient deployment. Docker Hub will facilitate the transfer of images between Ansible and Kubernetes, ensuring smooth operations.

## Goal of the Project

To implement CI/CD such that ABC Company is able to be —

- high available
- high scalable
- high performant
- easily built and maintained
- developed and deployed quickly

## Problem Statements / Tasks

We need to develop a CI/CD pipeline to automate the software development, testing, packaging, and deployment, reducing the time to market the app and ensuring good quality service is experienced by end users. In this project, we need to —

- Push the code to our GitHub repository
- Create a continuous integration pipeline using Jenkins to compile, test, and package the code present in GitHub.
- Write Dockerfile to push the war file to the Tomcat server
- Integrate Docker with Ansible and write the playbook
- Deploy artifacts to the Kubernetes cluster
- Monitor resources using Grafana.

## Task 1: Clone the project from the GitHub link shared in resources to your local machine. Build the code using Maven commands.

### Setup Project

> **Important Note:** All commands below should be run from the root directory.

- To achieve this task first of all, download the provided Java Source Code into on my Local PC and then created a GitHub Repository and uploaded all the project files to the repo.
  ![Github repository CI/CD for Retail Company](assests/github-repo.png)

- Launch an EC2 Linux instance (type: t2.micro) named retail-app-master.
  ![AWS EC2 retail-app-master INSTANCE](assests/retail-app-master-instance.png)

- Connect to retail-app-pro server using local terminal and installed git to retail-app-master instance.

  `sudo apt-get install git`

- Clone the repository using git clone command as shown below to clone the repo from my Github to my Amazon EC2 Linux machine.

  `git clone https://github.com/robinucar/CI-CD-PIPELINE_for_Retail_Company.git`

- Install java to my EC2 retail-app-master instance.

  ```
   #To update the machine
      sudo apt update
    #To install java
      sudo apt install openjdk-11-jdk -y
  ```

- Install Jenkins

  - To install jenkins go to the website [Jenkins](https://pkg.jenkins.io/debian-stable/) and then select “debian-stable/”for a stable version of jenkins. Follow the procedure to install jenkins.

    ```sudo wget -O /usr/share/keyrings/jenkins-keyring.asc \
      https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key

      # Then add a Jenkins apt repository entry:

      echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
      https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
      /etc/apt/sources.list.d/jenkins.list > /dev/null

      # Update your local package index, then finally install Jenkins:

        sudo apt-get update
        sudo apt-get install fontconfig openjdk-11-jre
        sudo apt-get install jenkins
    ```

  - Start jenkins using the command:

    ```
    sudo systemctl start jenkins
    sudo systemctl enable jenkins
    sudo systemctl status jenkins
    ```

- Install Maven which will be used to Compile, Test and Package the Code. Run the following commands to install maven

  ```
    sudo apt update
    sudo apt install maven
    mvn -version
  ```

  ![Maven version](assests/mvn-version.png)

**Now all the setup required is done. We need to start to building.**

### Building process

1. Navigate to the directory in which the source code and files are and then run the command below:

```
  cd CI-CD-PIPELINE_for_Retail_Company/
  mvn compile
```

![mvn build](assests/mvn-build.png)

2. Testing the code: To test the code go to directory which the source code and files are and then run the command below:

`mvn test`

![mvn test](assests/mvn-test.png)

3. Packaging the code: To package the code go to directory which the source code and files are and then run the command below:

`mvn package`

![mvn package](assests/mvn-package.png)

**_War File Created_**

The result output, the war file file created in the target folder is as shown below image.
![war file created package](assests/warfile.png)

**_ END OF TASK 1 _**

## Task 2: Login to Jenkins and start to create build pipeline containing the jobs.

1.  Log in to Jenkins

- Jenkins can be accessed over the browsers through the port number 8080. Take the public ip address from the EC2 Instance and paste the code below on the browser.

```
  public IP address:8080
  # in my case
    13.40.17.107:8080
```

![Jenkins login](assests/jenkins-login.png)

**Get the password, run the code below on your terminal.**

`cat /var/lib/jenkins/secrets/initialAdminPassword`

Then copy the password and paste to administrator password of Jenkins login page. Follow the promts until the dashboard access is fully set.

2. The next step is to set up the necessary tools with Jenkins: Maven, Java, and Git.
   To do this, go to the Jenkins dashboard > Manage Jenkins > Tools.

- Use the command below to find the JAVA_HOME.

```
  update-alternatives --config java
```

![java home](assests/java-home.png)

**The selected part of output is JAVA_HOME**

- Copy the JAVA_HOME address as shown in the image above, then paste it into the Jenkins JDK installations tool..
  ![Java-Home-Jenkins](assests/Jenkins_java_home.png)

- For Git Integration we do need to make any changes, we work with the default settings.

- Maven has already been installed through the CLI. The MAVEN_HOME from the selected part of the output below should be copied, and then pasted into the 'MAVEN_HOME' input field in the Jenkins Maven tool. Then click 'Save'.
  ![maven home](assests/maven-home.png)
  ![maven home jenkins](assests/maven-jenkins.png)

3. After the tools have been set up, the jobs are now ready to be started.

- Compile Job:
  The first job is the Compile Job, serving as the upstream job for the Test Job, which in turn becomes the upstream job for the Package Job. Additionally, the Compile Job is scheduled to trigger a build every hour.

  - On Jenkins Dashboard click New item > Job name > Select freestyle project and click OK.
    ![1.compile](assests/first-job.png)

  - Go to source code management under git section to add the repository URL:
    [https://github.com/robinucar/CI-CD-PIPELINE_for_Retail_Company/](https://github.com/robinucar/CI-CD-PIPELINE_for_Retail_Company/)

    No credentials are required because it’s a public repo.

    **_ Branches to build should be main _**

    Then click Save

    ![1.job](assests/first-job-pt1.png)

  - Next, navigate to the build steps and choose 'Invoke top-level Maven target.'

    - Under "Maven version," choose "mymaven," then set the command as "compile" under Goals, and finally, save the changes.
      ![1.job-nav](assests/first-job-pt2.png)

  - Now, we are prepared to compile our source code. Click 'Build Now' on the dashboard, and the successful compile job output will be displayed as shown below:
    ![1.job-log](assests/first-job-log.png)

- Test Job:
  Next, we proceed to test the code using the same steps as above, with the only difference being that the goal in this case is 'test'.

  Navigate to build triggers to connect the test as a downstream job of the Compile job. This means that unit testing will be conducted immediately after the compile job is completed.

  To do this, go to 'Build Triggers' and select 'Build after other projects are built'. In the 'Projects to watch' field, type in the upstream job '1.Compile', then save and build the job.

  ![test-job-1](assests/test-job-pt1.png)
  ![test-job-2](assests/test-job-pt2.png)

- Package Job:
  Now, the next step is to package the code into artifacts. Follow the same procedure as above, with the only difference being that the 'Goal' in this case is 'package'.

  Additionally, go to 'Build Triggers' and select 'Build after other projects are built'. In the 'Projects to watch' field, type in the upstream job as '2.Test', then save and build the job. The output is as shown below:

  ![package-job-1](assests/package-job-pt1.png)
  ![package-job-2](assests/package-job-pt2.png)

- Create a build pipeline:
  To visualize all the jobs as a pipeline, we need to download the Build Pipeline plugin.

1. Go to "Manage Jenkins" > "Manage Plugins".
2. Search for the "Build Pipeline" plugin and select it.
3. Install the plugin without restarting Jenkins.
4. After installing the plugin:

Return to the dashboard.

1. Click the plus (+) sign to configure the pipeline.
2. Give a name to the pipeline (e.g., "Retail-app-Pipeline").
   Select "Build Pipeline View".
   Click "OK".

![Build Pipeline](assests/build-pipeline.png)

**_ END OF TASK 2 _**
