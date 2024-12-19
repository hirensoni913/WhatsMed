# WhatsMed - Team Synergy 🏥

## Index 📑
1. [Who we are 👥👥](#who-we-are-)
2. [Background 🏞️](#background-)
3. [Description of the process 📝](#description-of-the-process-)
- [AS-IS 📋](#as-is-)
- [AS-IS Pain Points ⚠️](#as-is-pain-points-)
- [TO-BE 🔄](#to-be-)
- [Process Overview 🗂️](#process-overview-)
4. [Technical Background 🛠️](#technical-background-)
- [Deliverables and Artefacts 📦](#deliverables-and-artefacts-)
5. [Conclusion 🏁](#conclusion-)
6. [References and Sources 📚](#references-and-sources-)
7. [Presentation slides 📊](#presentation-slides-)
  
## Who we are 👥👥
The team consists of the following members:
- Hirenkumar Soni <hirenkumar.soni@students.fhnw.ch>
- Emmanouil Damilakis <emmanouil.damilakis@students.fhnw.ch>
- Paul Gross <paul.gross@students.fhnw.ch>
- Carl Chrobak <carl.chrobak@students.fhnw.ch>

## Background 🏞️
Our aim was to model a clinical pathway and to improve work efficiency on time-intensive tasks, which overlap with different roles. We analyzed the patient process in the hospital, aiming to identify time-intensive key tasks and to simplify them by reducing the manual and user - i.e. physician-related - work tasks in the documentation process. 
Most publications focus on improving existing processes in the EHR (Electronic health record), introduce CDS (Clinical decision support) or relate to outpatient treatment, but do not consider the cross-section between admission documents as paper or pdf and the lack of errors due to transcribing wrong or only selected portions of the material, which is related to time-restraints and other causes. 

We querried Google Scholar, Date Dec 14 2024, with the following search terms:

> drug interaction hospital admission prescription errors  
> hospital admission prescription errors  
> drug interaction EHR  

A few examples of the most important sources of medication interactions and related (critical) incidents help to understand the scope and the implications:
- wrongly or not documented medication taken by the patient before or during admission
- the change of doses due to wrong or multiple information
- prescription of medication that is contraindicated because of allergies, drug-drug interactions risk of Torsade de points or other reasons
- physician's lack of time to review all available documents
- erroneous prescription due to patients misinformation or obliviousness

Our aim was to improve the documentation process and introduce an element, which might make future admissions more streamlined and less likely to suffer transkription errors, while reducing the total amount of time for physician, nurse or other caregivers documenting medication.

## Description of the process 📝

### AS-IS 📋
We modeled a hospital workflow as a pool consisting of a pharmacy pool, which was basically used to check and prepare the medication for the hospital ward, and an in/outpatient ward consisting of the treating staff including the physician. The physician visits the patient, documents the case, queries additional information from the database, and finalizes entry documentation. The patient is treated at the hospital ward and presented with discharge documents - including a medication list - before leaving the hospital. After the discharge process is complete, the responsibility of the hospital is practically completely transferred to the ambulatory medical providers, who is most of the time a GP or a specialist. The ambulatory sector was not modeled in detail, but the context is important to provide against intermingling the ambulatory and inpatient sector after completing the discharge process.

### AS-IS Pain Points ⚠️⚠️⚠️
- **Manual Data Input**: 💻 
The manual processes involve the documentation by hand - i.e. keyboard, copy-paste - from PDF files and scanned entry documentation received from the patient. This leads to a time-intensive task, which is highly vulnerable to errors, due to wrongly transcribed data or misspelled information, lost information due to time constraints, and the lack of or shift work.
- **Database**: 💾
The databases contain human readable local (hospital) records and often scanned (external) documents. The search functions are often rudimentary and the formating of pdfs containing breaks at the end of every line impede the task further.  Further, the fragmentation of the medical sector into local records associated with every caregiver, be it medical staff, be it physicians, caregivers, physiotherapists, or other staff aggravete the non-availability of informations further.

### TO-BE 🔄
The basic pools and lanes remain untouched, but we enriched the physician's subtask by introducing an API - called Whatsmed -, which helps automating the task of documentating the medication prescribed in the ambulatory sector by the GP or multiple additional actors including supplements, which are a well known source of interactions with medication. The API is used to document the administration medication and output it in a usable form (e.g. PDF or CSV file), which can be directly used by the KIS Database after a short review by the prescribing physician and even used to create a medication list for the user which can be send to patient or other stackeholders in the healthcare system, e.g. GP, pharmacy or home care.

### Process Overview 🗂️

## Technical Background 🛠️

This project integrates the **Camunda Workflow Engine** with **ChatGPT** to automate the interpretation of OCR-extracted medication data. The workflow uses a Service Task to query the **WhatsMed API**, which connects to ChatGPT to process and analyze the extracted blister text data.

### Workflow Objective 🎯

The primary objective of this workflow is to process OCR-extracted text from medication blisters provided through a form field in a **User Task**. If the checkbox in the User Task form (indicating manual confirmation) is left unchecked and the **blister_text** field contains data, the workflow proceeds to a Service Task named **Query WhatsMed API**.

This Service Task is designed to analyze the provided blister text and return a structured CSV output detailing the medication’s information and for storing it into a database. The analysis leverages ChatGPT to determine details like brand name, active ingredients, dosage, dosage form, and indication. This process automates the interpretation of blister text, reducing manual effort and ensuring consistency.

---

### How the Workflow Operates 🔄

1. **User Task – Verify Complete Record**:
   - A User Task form captures two key inputs:
     - A checkbox `"is_verified"` indicating whether the information about the medications are complete.
     - The **blister_text**, which contains OCR-extracted text from the medication blister.

2. **Gateway Logic**:
   - If the checkbox `"is_verified"` is **unchecked** and the **blister_text** field is populated, the workflow routes to the Service Task named **Query WhatsMed API**.
   - If the checkbox is checked, the workflow bypasses the Service Task.

3. **Service Task – Query WhatsMed API**:
   - The Service Task triggers a Camunda external task with the topic `"generate_response"`.
   - The worker fetches the task, retrieves the **blister_text** variable, and processes it using ChatGPT.

---

### Connection to WhatsMed API and ChatGPT 🌐

#### WhatsMed API Workflow 🧩
The worker acts as an external task processor for the `"generate_response"` topic. It:
1. Fetches the task from Camunda, extracting the **blister_text** variable from the process.
2. Constructs a prompt that combines a predefined template with the provided blister text.

#### ChatGPT Integration 🧠
The constructed prompt is sent to ChatGPT for processing. The prompt template is as follows:
```plaintext
Imagine you are a pharmacist assistant.

I will provide you with a list of medications in an array. Please provide csv in text format where each row is a medication and the columns are: 

Brand_Name : The brand name of each medication
Substance: The active ingredient in the medications
Dosage: The dose of the medication 
Dosage_form: The dosage form the medication is in
Indication: What indication the medicine is for

For all columns except indication, if some of the information is not available, like for example if I do not define the dosage in my array, you should fill those spaces with NaN. For indication, the information will never be provided and will instead be filled by you. Use https://www.drugs.com as your data source.

Do not provide any other information other than the csv in text format.

It is possible that only part of the medication's name is provided. In this case, do not try to guess the name. Instead, insert Information not available- name unclear for all 5 columns of that row.
```
The blister_text from the Camunda process is appended directly to the end of the prompt.


## Conclusion ✨
The Swiss Healthcare System suffers from the constant breaching of the main features of electronic health records by creating printouts and PDF files, which are not saved in a centralized database and partially lost due to fragmentation, culminating in physicians and other healthcare professionals constantly copying PDFs or printouts into the local EHR and painstakingly comparing multiple records of discharge documents to summarize important and essential information like the medication taken by the patient prescribed by multiple parties without being orchestrated sufficiently. Every practitioner and hospital uses their own and local HIS system and database, which is most of the time not accessible by other parties. The proposed solution for Switzerland - Electronic Patient Record (EPR) - is not used by most actors and till now an expensive dead end, due to the lack of adoption, but generally would have a lot of promise, if the adoption was more common.

Our project aimed to simplify certain documentation tasks for the physicians in inpatient settings, which are based on the lack of a centralized patient record, and can easily be adapted to the ambulatory sector in terms of different users, e.g. GP, pharmacy, patient, or other healthcare professionals. Our project is a proof of concept to facilitate the documentation process for medical experts, especially physicians or practical nurses by offering the possibility to scan different medications as either blister, from a paper list, or from packaging to create a list for care providers with additional information about the medication. Further functions could easily be added due to our OCR integration and the use of a large language model to get additional information about medication available in Switzerland or even in other countries. We could prove that support for the current workflows does not have to be a complete workover in terms of digitalization, but a thorough and smooth process which can be incrementally integrated into current processes without much effort.

## List of abreviations 📒
| Abreviation | Definition |
| :-------- | :----------- |
| BPMN | Business Process Model and Notation |
| CDS | Clinical Decision Support |
| DMN | Decision Model and Notation |
| EHR | Electronic Health Record |
| EPR | Electronic Patient Record |
| GP | General practioneer |
| HIS | Hospital information system |
| OCR | Optical character recognition |

## References and (external) Sources 🔗

- Images for the presentation were created using OpenAI’s DALL·E 3 and are free for Non-Commercial Use (for further information see https://openai.com/policies/terms-of-use/ accessed on Dec, 14. 2024).
- Framework *PLEASE insert additional information*
- Camunda Modeler 7, Software v. 5.27.0, https://camunda.com/platform/modeler/ for BPMN/DMN creation
- Deepnote, https://deepnote.com/
- Github Copilot, https://github.com/features/copilot

## Literature 📚
- Müller, Richard, and Andreas Rogge-Solti. "BPMN for healthcare processes." Proceedings of the 3rd Central-European Workshop on Services and their Composition (ZEUS 2011), Karlsruhe, Germany. Vol. 1. 2011.
- Pufahl, Luise, et al. "BPMN in healthcare: Challenges and best practices." Information Systems 107 (2022): 102013.
- Atasoy, Hilal, Brad N. Greenwood, and Jeffrey Scott McCullough. "The digitization of patient care: a review of the effects of electronic health records on health care quality and utilization." Annual review of public health 40.1 (2019): 487-500.
- Dechanont, Supinya, et al. "Hospital admissions/visits associated with drug–drug interactions: a systematic review and meta‐analysis." Pharmacoepidemiology and drug safety 23.5 (2014): 489-497.
- Westbrook, Johanna I., et al. "Effects of two commercial electronic prescribing systems on prescribing error rates in hospital in-patients: a before and after study." PLoS medicine 9.1 (2012): e1001164.
 
## Presentation slides 📊
See folder [presentation](presentation/)
