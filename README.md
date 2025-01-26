# Wologa API Documentation

Welcome to the **Wologa API Documentation**! This repository contains the comprehensive documentation for the API, designed to help developers integrate and utilize its functionalities effectively.

---

## **Table of Contents**

- [Overview](#overview)

---

## **Overview**

This repository serves as the single source of truth for the API's documentation. It includes details about available endpoints, request/response formats, error handling, and usage examples.

Download [Bruno](https://www.usebruno.com/) to view the API documentation in a more user-friendly format, and play with the API requests.
- The API provides 3 main collections: `auth` , `homologation-process` and `signed-url`.

---
# **Auth**

## **1. Create an App**

In order to use the API, an application must be created:

- Make a `POST` request to the `/app` endpoint with the following payload example:

```json
{
  "name": "My App Name",
  "host": "https://myapp.com" // (optional)
}
```

- The response will include the `api_key` that must be used to authenticate all the other requests.

```json
{
  "name": "my_app_name",
  "host": "https://myapp.com",
  "api_key": "SECRET_API_KEY"
}
```
- Before using the API Key, contact the Wologa team to enable it with a usage plan.
---
## **2. Create a User**

Once you have an app, you can create a user for it:

- Make a `POST` request to the `/user` endpoint with the following payload example:

```json
{
  "name": "John Doe",
  "email": "jhon@doe.com",
  "internal_id": "123456",
  "can_write": true
}
```
- If the `can_write` field is set to `true`, the user will be able to create and update processes.

```json
{
  "id": "RESPONSIBLE_ID",
  "name": "Jhon Doe",
  "email": "jhon@doe.com",
  "internal_id": "123456",
  "can_write": true
}
```
- The response will include the user `id` that must be used as `responsible` to be able to track who made a change.
---
# **Homologation Process**

To create and modify a homologation process, you must send a _Command_, which is a JSON object with a `type` field that indicates the action to be performed.

All the requests to the `/homologation-process` endpoint must be authenticated with the `api_key` obtained when creating an app.

## **Create Process Command**

Once you have an app and a user, you can create a process:

- Make a `POST` request to the with a payload like:

```json
{
  "type": "CreateProcess",
  "country": "ES",
  "program": "Medicina",
  "degree": "UNDERGRAD_DEGREE",
  "person": {
    "name": "John",
    "last": "Doe",
    "email": "jhon@mail.com",
    "gender": "MALE",
    "national_id": "123456789",
    "nationality": "CO",
    "birthplace": "CO",
    "birthdate": "1990-01-01T00:00:00Z"
  },
  "responsible": "RESPONSIBLE_ID"
}
```
- For degree, you can have the following values:
1. `UNDERGRAD_DEGREE`: ("Bachelor's Degree (e.g., AA, AS, BA, BS)"), 
2. `MASTERS_DEGREE`: ("Master's Degree (e.g., MA, MS, MBA)"),
3. `PROFESSIONAL_DEGREE`: ("Professional Degree (e.g., MD, JD, DDS)"),
4. `SPECIALIZED_DEGREE`: ("Specialized Professional Degree (e.g., Medical Specialization)"),
5. `DOCTORATE`: ("Doctorate (e.g., PhD, EdD)");

```json
{
  "id": "0001KSCCZ175X5M19WGY6857YV",
  ...,
  "status": "Created"
}
```
- The response will include the process `id` that must be used to identify any of its modifications.
- The process is different for each degree type. The following sections describe the process for each degree type.
---

## **Undergrad Process**

For an Undergrad Process, you can update the process with the following _Commands_:

### **Step 1: Save Title Details**
```json
{
  "type": "SaveTitleDetails",
  "id": "PROCESS_ID",
  "name": string,
  "title_university": string?,
  "title_country": "ES",
  "title_in_spanish": boolean,
  "applicant_from_spanish_country": boolean,
  "title_issued_in_eu": boolean,
  "title_older_than_5_years": boolean,
  "currently_residing_in_spain": boolean?,
  "worked_or_working_in_spain": boolean?,
  "responsible": "RESPONSIBLE_ID"
}
```
### **Step 2: Upload Undergrad Certificates**
```json
{
  "type": "UploadUndergradCertificates",
  "id": "PROCESS_ID",
  "undergrad_title_url": string,
  "undergrad_title_apostille_url": string?,
  "undergrad_title_legalization_url": string?,
  "undergrad_title_resolution_es_url": string?,
  "undergrad_title_resolution_other_url": string?,
  "undergrad_title_resolution_other_apostille_url": string?,
  "responsible": "RESPONSIBLE_ID"
}
```
### **Step 3: Upload Academic Certificates**
```json
{
  "type": "UploadAcademicCertificates",
  "id": "PROCESS_ID",
  "academic_certificate_url": string,
  "academic_certificate_apostille_url": string?,
  "academic_certificate_legalization_url": string?,
  "responsible": "RESPONSIBLE_ID"
}
```
### **Step 4: Upload Job Certificates**
```json
{
  "type": "UploadJobCertificates",
  "id": "PROCESS_ID",
  "job_certificates_url": string?,
  "job_certificates_notarized_url": string,
  "job_certificates_apostille_url": string,
  "responsible": "RESPONSIBLE_ID"
}
```
### **Step 5: Upload Passport**
```json
{
  "type": "UploadPassport",
  "id": "PROCESS_ID",
  "passport_url": string,
  "responsible": "RESPONSIBLE_ID"
}
```
### **Step 6: Accept Responsible Declaration**
```json
{
  "type": "AcceptResponsibleDeclaration",
  "id": "PROCESS_ID",
  "responsible": "RESPONSIBLE_ID"
}
```
---
## **Specialist Process**

For a Specialist Process, you can update the process with the following _Commands_:

// TODO
