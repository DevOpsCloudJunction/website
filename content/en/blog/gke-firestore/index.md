---
title: "Guide to have GKE based application use Firestore as database"
description: ""
excerpt: "Guide to have GKE based application use Firestore as database"
date: 2024-08-23T09:19:42+01:00
lastmod: 2024-08-23T09:19:42+01:00
draft: false
weight: 44
images: [firestore.png]
categories: ["Kubernetes", "Firestore", "Workload-identity"]
tags: ["Kubernetes", "Firestore", "Workload-identity"]
contributors: ["balabtech"]
pinned: true
homepage: true
---

# Guide to have GKE based application use Firestore as database
[Cloud Firestore](https://firebase.google.com/docs/firestore) is a flexible, scalable database for mobile, web, and server development from Firebase and Google Cloud. 

This document provides detailed steps for GKE based application workloads to securely access the Cloud Firestore database using Google's Workload Identity Federation.

## What is Workload Identity Federation?

Workload Identity Federation for GKE is available through IAM Workload Identity Federation, which provides identities for workloads that run in environments inside and outside Google Cloud.

## How to configure Workload identity for GKE workload to access Firestore

- **Set Up Kubernetes Service Account**:
  - Create the Kubernetes service account:
    ```bash
    kubectl create serviceaccount <K8S_SA> --namespace <NAMESPACE>
    ```

- **Configure IAM Policy for the Kubernetes Service Account**

  - You can bind the Kubernetes service account to the necessary IAM roles using the following `gcloud` command:

    ```bash
      gcloud iam service-accounts add-iam-policy-binding <SERVICE_ACCOUNT_EMAIL> \
      --role "roles/datastore.user" \
      --member "principal://iam.googleapis.com/projects/<PROJECT_NUMBER>/locations/global/workloadIdentityPools/<PROJECT_ID>.svc.id.goog/subject/ns/<NAMESPACE>/sa/<K8S_SA>"
    ```

    Replace the placeholders as follows:

      - <SERVICE_ACCOUNT_EMAIL>: The email of the default Google Compute service account (usually in the format <PROJECT_ID>@appspot.gserviceaccount.com).
      - <PROJECT_NUMBER>: Your Google Cloud project number.
      - <PROJECT_ID>: Your Google Cloud project ID.
      - <NAMESPACE>: The Kubernetes namespace of the pods.
      - <K8S_SA>: The name of the Kubernetes service account.

    More principal member conditions can be added like pods from cluster or specific namespace etc. ref [Supported Principal conditions](https://cloud.google.com/kubernetes-engine/docs/concepts/workload-identity#principal-id-examples)

### Steps to Grant Access to the Kubernetes Deployment

1.  **Annotate the Kubernetes Service Account**: You need to annotate the Kubernetes service account with the email of the Google Cloud service account that it will use to authenticate. Run the following command:

    
    ```bash
      kubectl annotate serviceaccount <K8S_SA>\
        --namespace <NAMESPACE>\
        iam.gke.io/gcp-service-account=<YOUR_SERVICE_ACCOUNT_EMAIL>

2.  **Update Your Kubernetes Deployment**: In your Kubernetes deployment manifest, ensure that you specify the Kubernetes service account. Here's an example deployment YAML:

    ```yaml
    apiVersion: apps/v1
    kind: Deployment
    metadata:
      name: my-app
      namespace: <NAMESPACE>
    spec:
      replicas: 1
      selector:
        matchLabels:
          app: my-app
      template:
        metadata:
          labels:
            app: my-app
        spec:
          serviceAccountName: <K8S_SA>  # Specify your Kubernetes service account here
          containers:
          - name: my-app-container
            image: gcr.io/<YOUR_PROJECT_ID>/my-app:latest
            ports:
            - containerPort: 8080

3.  **Deploy Your Application**: Apply your updated deployment configuration:

    ```bash
    kubectl apply -f <your_deployment_file>.yaml

4.  **Verify the Setup**: Once your deployment is up, you can verify that the application can access Firestore by checking the logs or by testing API calls to Firestore from within your application.

### Additional Notes

-   Make sure that the Google Cloud service account has the necessary permissions to access Firestore (e.g., `roles/datastore.user`).
-   When your application runs in the GKE cluster, it will automatically use the Kubernetes service account to authenticate with Google Cloud using the associated service account.

## Best Practices for Workload Identity Federation

- **Provide specific resource access**: Provide access to specific Google Cloud resources that your application needs to interact.

