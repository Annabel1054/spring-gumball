# Spring Gumball CI/CD Lab 10

### This lab demonstrates the following two GitHub Workflows.

* https://help.github.com/actions/language-and-framework-guides/building-and-testing-java-with-gradle

* https://github.com/google-github-actions/setup-gcloud/tree/master/example-workflows/gke

### Build Dependencies

* Gradle 5.6
* JDK 11

## CI Workflow (Part 1)

### Using the Gradle Starter Workflow
<img width="1470" alt="1- gradle yml file" src="https://user-images.githubusercontent.com/55632013/235800544-29aa10cb-c4d2-430a-ab41-5a8b4ce183c1.png">

### Committed code changes to main branch to trigger the following workflow:
<img width="1470" alt="1- github workflow" src="https://user-images.githubusercontent.com/55632013/235800760-ed351b59-8f37-4fe2-9824-a5ffe33b6688.png">
<img width="1470" alt="1- github workflow 2" src="https://user-images.githubusercontent.com/55632013/235800771-c44dbf3c-f3ce-43ca-bbc3-7fee185f8c62.png">
<img width="1470" alt="1- github workflow 3" src="https://user-images.githubusercontent.com/55632013/235800785-b8a2aaf1-52c6-4986-bf1b-3c6efc227a54.png">

## CD Workflow (Part 2)
This section goes over deploying to Google Kubernetes Engine.

### Creating GKE Cluster
A standard cluster was built with the following conifgurations:
- GCP Project: cmpe172
- GKE Cluster Name: cmpe172
- GKE Cluster Zone: us-central1-c
<img width="1470" alt="2- creating GKE cluster" src="https://user-images.githubusercontent.com/55632013/235801589-7ae7ff9a-7c05-431b-992d-30f95730a393.png">

### Enabling the APIs
<img width="1470" alt="2- enable APIs" src="https://user-images.githubusercontent.com/55632013/235801934-4ad4f13a-6970-4151-8d23-6f1729a50aef.png">
<img width="1470" alt="2- enable APIs 2" src="https://user-images.githubusercontent.com/55632013/235801954-e972c8e8-c74d-432b-a48d-2acd68c128ce.png">

### Setting up secrets in your Workspace

#### Step 1. Locate your GKE Project ID
<img width="1470" alt="2- find GKE project ID" src="https://user-images.githubusercontent.com/55632013/235802087-0be7638e-f21d-4c7f-abf7-07ffce29103d.png">

#### Step 2. Create a Service Account for GitHub Access
<img width="1470" alt="2- create spring-gumball service account" src="https://user-images.githubusercontent.com/55632013/235802217-0a899e70-2d06-4c9f-aad0-d60e834b7e27.png">

#### Step 3. Add the following Cloud IAM roles to your service account for your GKE Cluster:
- Cloud IAM roles added: 
  - Kubernetes Engine Developer - Full access to Kubernetes API objects inside Kubernetes Clusters.
  - Storage Admin - Full control of GCS resources.
<img width="1470" alt="2- adding IAM Roles" src="https://user-images.githubusercontent.com/55632013/235802292-b6884b39-8ef6-4fe6-8059-31ab4dc66ea1.png">
<img width="1470" alt="2- adding IAM roles 2" src="https://user-images.githubusercontent.com/55632013/235802297-366315ab-e7a7-4e58-805c-89970945c911.png">

#### Step 4. Create a JSON service account key for the service account.
<img width="1470" alt="2- create JSON service acc key" src="https://user-images.githubusercontent.com/55632013/235802507-4a93054c-05b2-4313-b576-017bdb523df4.png">
<img width="1470" alt="2- create JSON service acc key 2" src="https://user-images.githubusercontent.com/55632013/235802527-1de16764-227c-4353-a8d0-80db20f6e06d.png">
<img width="1470" alt="2- create JSON service acc key 3" src="https://user-images.githubusercontent.com/55632013/235802539-29d45a99-33cf-4dc6-a229-97c228b36b5b.png">

#### Step 5. Configure GitHub Secrets
- Add the following secrets to your repository's secrets:
  - GKE_PROJECT: Google Cloud project ID
  - GKE_SA_KEY: The content of the service account JSON file 
<img width="1470" alt="2- configure github secrets" src="https://user-images.githubusercontent.com/55632013/235802779-da0f9a9c-4946-473d-a24f-cd152004a730.png">
<img width="1470" alt="2- configure github secrets 2" src="https://user-images.githubusercontent.com/55632013/235802785-90866034-1563-4ea6-b267-2032e3ceee8b.png">
<img width="1470" alt="2- configure github secrets 3" src="https://user-images.githubusercontent.com/55632013/235802804-88305b66-4946-429a-bd71-37b90af1431d.png">
<img width="1470" alt="2- configure github secrets 4" src="https://user-images.githubusercontent.com/55632013/235802816-69315984-3e8d-458b-a7eb-047b84b69b38.png">

### Configuring Kustomize along with other YML files
Kustomize is an optional tool used for managing YAML specs. After creating a kustomization file, the workflow below can be used to dynamically set fields of the image and pipe in the result to kubectl.
<img width="1470" alt="2- configuring kustomize" src="https://user-images.githubusercontent.com/55632013/235803004-07a4b8b4-7896-4859-9035-59cd5f4f94e4.png">
<img width="1470" alt="2- configuring kustomize 2" src="https://user-images.githubusercontent.com/55632013/235803014-882ee672-116d-4acb-881c-f321191b1c74.png">
<img width="1470" alt="2- configuring kustomize 3" src="https://user-images.githubusercontent.com/55632013/235803027-98f6c449-2344-4929-b9f8-ed6d4235f4cd.png">
<img width="1470" alt="2- configuring kustomize 4" src="https://user-images.githubusercontent.com/55632013/235803035-d4679b39-39ef-44f5-a9de-a5d343dc7e17.png">

### Trigger and Deployment to GKE
<img width="1470" alt="2- Trigger and Deployment to GKE" src="https://user-images.githubusercontent.com/55632013/235803089-40d522f6-0e10-4d9d-8b4e-a45a07db77f3.png">
<img width="1470" alt="2- Trigger and Deployment to GKE 2" src="https://user-images.githubusercontent.com/55632013/235803102-e62fef39-a2b2-434c-bac6-f1d69f7a0e19.png">
<img width="1470" alt="2- Trigger and Deployment to GKE 3" src="https://user-images.githubusercontent.com/55632013/235803113-402155eb-13c0-4768-8e34-9e30cb4e8c30.png">
<img width="1470" alt="2- Trigger and Deployment to GKE 4" src="https://user-images.githubusercontent.com/55632013/235803120-d15f2f94-ca03-41ad-95cb-88dcf9b0d7ee.png">
<img width="1470" alt="2- Trigger and Deployment to GKE 5" src="https://user-images.githubusercontent.com/55632013/235803132-65b4f1e6-95f6-448c-81a4-344b88d0cc1d.png">

#### Confirm the Pods and Service have been Deployed to your GKE Cluster
<img width="1470" alt="2- Trigger and Deployment to GKE 6" src="https://user-images.githubusercontent.com/55632013/235803149-5a70e735-4c53-4f87-9d9b-d973e996c896.png">
<img width="1470" alt="2- Trigger and Deployment to GKE 7" src="https://user-images.githubusercontent.com/55632013/235803215-4528ef9a-4c11-48b7-8387-085cffe0a5ee.png">

#### Set up a External Facing Load Balancer and Test the Gumball Spring App
<img width="1470" alt="2- Trigger and Deployment to GKE 8" src="https://user-images.githubusercontent.com/55632013/235803159-0a55cf07-77c1-4e65-8740-6c5497a3f326.png">
<img width="1470" alt="2- Trigger and Deployment to GKE 9" src="https://user-images.githubusercontent.com/55632013/235803171-d1e8c4fc-05ea-466b-93a9-2db832aef564.png">

#### Web UI on Load Balancer's External IP
<img width="1470" alt="2- Trigger and Deployment to GKE 10" src="https://user-images.githubusercontent.com/55632013/235803233-786eba47-903a-4bf0-aa72-c75726d0ac3b.png">
<img width="1470" alt="2- Trigger and Deployment to GKE 11" src="https://user-images.githubusercontent.com/55632013/235803237-9416ffe9-73c2-45e5-923f-c08ed41908f9.png">





