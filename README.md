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
