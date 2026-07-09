---
title: "Workshop"
date: 2026-06-17
weight: 3
chapter: false
pre: " <b> 5.3. </b> "
---

# Deploying Billo – A Flutter and AWS Serverless QR Ordering & Payment Application

#### Overview

**Billo** is a mobile application for QR code–based food ordering and cashless payment in restaurants and cafés. It was developed during an internship project.

In this workshop, you will deploy the complete Billo system, which consists of two major components:

- A serverless backend on AWS using Lambda, API Gateway, DynamoDB, and Amazon Cognito.
- A Flutter mobile application that supports two user roles within a single app: **Customer** and **Merchant**.

+ **Backend** – The entire infrastructure is defined using AWS SAM (Infrastructure as Code). It consists of seven Lambda functions, each responsible for a specific business capability, exposed through Amazon API Gateway, storing data in five DynamoDB tables, and authenticating users with Amazon Cognito.

+ **Frontend** – A multi-role Flutter application that uses the `provider` package for state management, communicates with the backend through REST APIs, and supports QR code scanning for ordering and payment.

#### Contents

1. [Workshop Overview](5.1-Workshop-overview/)
2. [Prerequisites](5.2-Prerequiste/)
3. [Deploying the Backend](5.3-Backend/)
4. [Deploying the Flutter App](5.4-Flutter-App/)
5. [End-to-End Testing](5.5-e2e-testing/)
6. [Cleaning Up Resources](5.6-Cleanup/)