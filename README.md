# WhatsMed - Team Synergy 🏥

## Index 📑
- [Who we are 👥👥](#who-we-are-)
- [Background 🏞️](#background-)
- [Description of the process 📝](#description-of-the-process-)
  - [AS-IS 📋](#as-is-)
  - [AS-IS Pain Points ⚠️](#as-is-pain-points-)
  - [TO-BE 🔄](#to-be-)
    - [Process Overview 🗂️](#process-overview-)
- [Technical Background 🛠️](#technical-background-)
  - [Deliverables and Artefacts 📦](#deliverables-and-artefacts-)
- [Conclusion 🏁](#conclusion-)
- [References and Sources 📚](#references-and-sources-)
- [Presentation slides 📊](#presentation-slides-)

## Who we are 👥👥
The team consists of the following members:
- Hirenkumar Soni <hirenkumar.soni@students.fhnw.ch>
- Emmanouil Damilakis <emmanouil.damilakis@students.fhnw.ch>
- Paul Gross <paul.gross@students.fhnw.ch>
- Carl Chrobak <carl.chrobak@students.fhnw.ch>

## Background 🏞️
Our aim was to model a clinical pathway and to improve work efficiency on time-intensive tasks, which overlap with different roles. We analyzed the patient process in the hospital, aiming to identify time-intensive key tasks and to simplify them by reducing the manual and user - i.e. physician-related - work tasks in the documentation process.

## Description of the process 📝
### AS-IS 📋
We modeled a hospital workflow as a pool consisting of a pharmacy pool, which was basically used to check and prepare the medication for the hospital ward, and an in/outpatient ward consisting of the treating staff including the physician. The physician visits the patient, documents the case, queries additional information from the database, and finalizes entry documentation. The patient is treated at the hospital ward and presented with discharge documents - including a medication list - before leaving the hospital. After the discharge process is complete, the responsibility of the hospital is practically completely transferred to the ambulatory medical providers, who is most of the time a GP or a specialist. The ambulatory sector was not modeled in detail, but the context is important to provide against intermingling the ambulatory and inpatient sector after completing the discharge process.

### AS-IS Pain Points ⚠️
- **Manual Data Input**: The manual processes involve the documentation by hand - i.e. keyboard, copy-paste - from PDF files and scanned entry documentation received from the patient. This leads to a time-intensive task, which is highly vulnerable to errors, due to wrongly transcribed data or misspelled information, lost information due to time constraints, and the lack of or shift work.
- **Database**: Only local records are available and the fragmentation of the medical sector in Switzerland greatly contributes to the workload for the medical staff, be it physicians, caregivers, physiotherapists, or other staff.

### TO-BE 🔄

#### Process Overview 🗂️

## Technical Background 🛠️
### Deliverables and Artefacts 📦
- Link to a running workflow(s) and microservice instantiation(s):
  - Link to start form(s) and cloud-based deployment(s)

## Conclusion 🏁
The Swiss Healthcare System suffers from the constant breaching of the main features of electronic health records by creating printouts and PDF files, which are not saved in a centralized database and partially lost due to fragmentation, culminating in physicians and other healthcare professionals constantly copying PDFs or printouts into the local EHR and painstakingly comparing multiple records of discharge documents to summarize important and essential information like the medication taken by the patient prescribed by multiple parties without being orchestrated sufficiently. Every practitioner and hospital uses their own and local EHR system and database, which is most of the time not accessible by other parties. The proposed solution for Switzerland - Electronic Patient Record (EPR) - is not used by most actors and till now an expensive dead end, due to the lack of adoption, but generally would have a lot of promise, if the adoption was more common.

Our project aimed to simplify certain documentation tasks for the physicians in inpatient settings, which are based on the lack of a centralized patient record, and can easily be adapted to the ambulatory sector in terms of different users, e.g. GP, pharmacy, patient, or other healthcare professionals. Our project is a proof of concept to facilitate the documentation process for medical experts, especially physicians or practical nurses by offering the possibility to scan different medications as either blister, from a paper list, or from packaging to create a list for care providers with additional information about the medication. Further functions could easily be added due to our OCR integration and the use of a large language model to get additional information about medication available in Switzerland or even in other countries. We could prove that support for the current workflows does not have to be a complete workover in terms of digitalization, but a thorough and smooth process which can be incrementally integrated into current processes without much effort.

## References and Sources 📚
- Müller, Richard, and Andreas Rogge-Solti. "BPMN for healthcare processes." Proceedings of the 3rd Central-European Workshop on Services and their Composition (ZEUS 2011), Karlsruhe, Germany. Vol. 1. 2011.
- Pufahl, Luise, et al. "BPMN in healthcare: Challenges and best practices." Information Systems 107 (2022): 102013.
- Atasoy, Hilal, Brad N. Greenwood, and Jeffrey Scott McCullough. "The digitization of patient care: a review of the effects of electronic health records on health care quality and utilization." Annual review of public health 40.1 (2019): 487-500.

## Presentation slides 📊
- See folder [presentation](presentation/)
- Images for the presentation were created using OpenAI’s DALL·E 3 and free for Non-Commercial Use.