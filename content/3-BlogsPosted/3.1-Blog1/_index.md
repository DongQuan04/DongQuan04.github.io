---
title: "Blog 1"
date: 2026-06-30
weight: 2
chapter: false
pre: " <b> 3.2. </b> "
---

# AUTOMATING MEDICAL RECORD DIGITIZATION WITH AMAZON BEDROCK DATA AUTOMATION & AWS HEALTHLAKE

Amazon Bedrock Data Automation (ABDA), combined with AWS HealthLake, opens up a new approach to digitizing medical records: leveraging the power of Generative AI to extract data from unstructured medical documents (prescriptions, scanned/photographed medical records, PDF files, etc.), then standardizing and storing it consistently according to the international healthcare data standard **FHIR (Fast Healthcare Interoperability Resources)**.

Key points to note:

* Handwritten or scanned medical records are often quite messy — Bedrock Data Automation extracts data far more accurately than traditional OCR thanks to its ability to understand medical context.
* Once extracted, raw data is automatically standardized into **FHIR** format, allowing different hospital systems to interoperate and "communicate" with one another.
* The architecture is **fully Serverless**, combining S3, EventBridge, Lambda, Bedrock, and HealthLake, allowing the system to scale automatically and optimize operating costs with a pay-as-you-go model.
* The processing flow is designed to be **Event-Driven**: when a file is uploaded to S3, EventBridge triggers the next processing chain, keeping the system's components decoupled and allowing smooth operation without bottlenecks.

Alongside these advantages, the solution also has some limitations and challenges to consider for real-world deployment:

* **Data security & compliance (HIPAA/Compliance):** medical data (PII/PHI) is highly sensitive; although AWS is committed to security, transferring patient data to an international cloud when deploying in Vietnam may still encounter significant legal barriers.
* **GenAI operating costs:** continuously calling the Bedrock API for a large volume of records (millions of legacy medical records) can incur significant costs without an appropriate optimization strategy and input data filtering.
* **Processing latency:** using an LLM to extract structured data will be slower than reading from a regular database, so this model is better suited to asynchronous processing (Async/Batch) rather than Real-time.

Through this article, we can see that the most notable practical application of GenAI today is not limited to chatbots, but also includes **Data Automation** — turning "messy," unstructured data into clean, structured data ready to be leveraged. At the same time, designing the architecture in an Event-Driven manner (using EventBridge as the trigger point) is also a valuable pattern worth learning and applying to other large-scale data processing systems.

![img_1.png](img_1.png)
Original article link: <https://aws.amazon.com/vi/blogs/architecture/automate-medical-record-digitization-with-amazon-bedrock-data-automation-and-aws-healthlake/>
