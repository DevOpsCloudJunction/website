---
title: "Guide to have GKE based application use Firestore as database"
description: ""
excerpt: "Guide to have GKE based application use Firestore as database"
date: 2024-08-23T09:19:42+01:00
lastmod: 2024-08-23T09:19:42+01:00
draft: false
weight: 70
images: []
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
  - Kubernetes uses a metrics server to gather resource utilization data.
  - Create the Kubernetes service account:
    ```bash
    kubectl create serviceaccount <K8S_SA> --namespace NAMESPACE
    ```

- **Configure IAM policy for Kubernetes SA**:
    ```tf
      resource google_service_account_iam_member "gke_service_account_binding" {
          service_account_id = data.google_compute_default_service_account.default.name
          role               = "roles/datastore.user"
          member             = "principal://iam.googleapis.com/projects/<PROJECT_NUMBER>/locations/global/workloadIdentityPools/<PROJECT_ID>.svc.id.goog/subject/ns/<NAMESPACE OF PODS>/sa/<K8S_SA>?"
      }
    ```

  More principal member conditions can be added like pods from cluster or specific namespace etc. ref [Supported Principal conditions](https://cloud.google.com/kubernetes-engine/docs/concepts/workload-identity#principal-id-examples)


## Best Practices for Workload Identity Federation

- **Provide specific resource access**: Provide access to specific Google Cloud resources that your application needs to interact.

