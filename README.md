DevOps - Exercise

Product

Develop a small product that consists of:

●       A web page that presents: "Hello World" (root path "/")

●       A web page that executes some arbitrary CPU-intensive task - for 60 seconds. (path "/intense")

●       A unit test, (a simple assert is sufficient, we just expect to see the unit test being executed - see below).

Source code Repository

The project's source code should be managed in Bitbucket or GitHub.

Build Workflow – Continuous Delivery

Create a new GCE Machine (the cheapest one, you get 300$ for free from google to test) and install Jenkins on it.

Create a Docker image repository.

Configure Jenkins to listen to any push made to the project's repository (master branch only).
When a push is made, Jenkins should:

Execute the unit test - if it fails abort the job.
Build the product.
Create a new Docker image that contains a web server (of your choice) that runs the product.
Push the Docker image into the Docker repository.
Production

Create a Kubernetes cluster that runs 2 replicas of the image built by Jenkins.
Whenever a replica in the cluster exceeds CPU utilization of certain threshold, a new replica should be brought up with the same image.
Once the CPU utilization drops below the threshold, the cluster should be reduced to 2 replicas.

Deploy

Create a Jenkins job used manually for deploying the product to production.
The job receives the image's tag name as an input parameter, and deploys the relevant image from the Docker registry to the Kubernetes cluster.

