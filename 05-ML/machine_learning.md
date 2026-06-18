## Amazon Rekognition

##₹ What is Amazon Rekognition?

Amazon Rekognition is an AWS computer vision service that uses Machine Learning (ML) to analyze images and videos.

Think of it as:

> AI that can see and understand images and videos.

You upload an image or video, and Rekognition identifies what is present inside it.

---

## Key Features

### Object Detection
### Scene Detection

Recognizes environments like:

- Beach
- Office
- Mountain
- City
- Classroom

### Facial Analysis

Analyzes facial attributes:

- Age range
- Gender
- Smile
- Emotions
- Beard
- Glasses
### Face Comparison

Compares two images and determines whether they belong to the same person.

Use Cases:

- Passport verification
- Identity verification
- KYC systems

# Amazon Rekognition Content Moderation

## What is Content Moderation?

Content Moderation automatically detects inappropriate or unsafe content in images and videos.

## Workflow

```text
User Uploads Image
        |
        v
Rekognition Moderation
        |
    +---+---+
    |       |
  Safe    Unsafe
    |       |
 Publish  Reject

```

## Use Cases

### Social Media

Filter harmful content.

### Dating Apps

Validate profile pictures.

### Online Communities

Prevent offensive uploads.

### Educational Platforms

Ensure safe learning content.


# Amazon Transcribe

## What is Amazon Transcribe?

Amazon Transcribe converts speech into text.

This is a Speech-to-Text service.It automatically detect the language.

# Amazon Polly

## What is Amazon Polly?

Amazon Polly converts text into human-like speech.

This is a Text-to-Speech (TTS) service.


# Amazon Polly Lexicons

## What is a Lexicon?

A Lexicon is a custom pronunciation dictionary.

Used when Polly pronounces a word incorrectly.

---

## Example

Word:

```text
SQL
```

Default:

```text
S-Q-L
```

Desired:

```text
Sequel
```

Lexicon fixes the pronunciation.

---

## Use Cases

### Company Names

Correct brand pronunciation.

### Technical Terms

Cloud and IT terminology.

### Medical Terms

Drug names and procedures.

---

# Amazon Polly SSML

## What is SSML?

SSML stands for:

```text
Speech Synthesis Markup Language
```

It allows customization of generated speech.

---

## Controls

- Speed
- Pitch
- Volume
- Pauses
- Emphasis

---

## Example

```xml
<speak>
Hello
<break time="2s"/>
Welcome to AWS.
</speak>
```

Adds a 2-second pause.

---

## Common Tags

### Pause

```xml
<break time="1s"/>
```

---

### Emphasis

```xml
<emphasis level="strong">
Important
</emphasis>
```

---

### Slow Speech

```xml
<prosody rate="slow">
Hello
</prosody>
```

---

### Fast Speech

```xml
<prosody rate="fast">
Hello
</prosody>
```

---

### Whisper Effect

```xml
<amazon:effect name="whispered">
Secret
</amazon:effect>
```

---

## Use Cases

### Story Narration

Different speaking styles.

### Voice Bots

Natural conversations.

### E-Learning

Interactive learning experiences.

---

# Amazon Translate

## What is Amazon Translate?

Amazon Translate converts text from one language to another.

Uses Neural Machine Translation (NMT).

---

## Example

Input:

```text
Hello
```

Output:

```text
Hola
```

---

## Features

- Real-time translation
- Batch translation
- Multi-language support
- Custom terminology

---

## Architecture

```text
English
    |
    v
Amazon Translate
    |
    v
Spanish
```

---

## Use Cases

### E-Commerce

Translate product descriptions.

### Customer Support

Multilingual chat systems.

### Mobile Apps

Global language support.

### News Platforms

Translate articles.

---

# Amazon Lex

## What is Amazon Lex?

Amazon Lex is a service for building conversational chatbots and voice bots.

It uses the same technology behind Alexa.

---

## Core Components

### Intent

What the user wants.

Examples:

```text
BookFlight
CancelOrder
CheckBalance
```

---

### Utterances

Different ways users ask the same thing.

Example:

```text
Book a flight
Reserve a ticket
Need a flight
```

---

### Slots

Information needed to fulfill an intent.

Example:

```text
Source
Destination
Date
```

---

## Workflow

```text
User Input
      |
      v
Amazon Lex
      |
      v
Intent Recognition
      |
      v
Response
```

---

## Use Cases

### Customer Support

24/7 chatbot.

### Banking

Balance inquiries.

### Travel

Booking flights and hotels.

---

# Amazon Connect

## What is Amazon Connect?

Amazon Connect is a cloud-based contact center solution.

Used to build customer support centers.

---

## Features

### IVR

Interactive Voice Response.

Example:

```text
Press 1 for Sales
Press 2 for Support
```

---

### Call Routing

Route calls to appropriate agents.

---

### Call Recording

Store conversations.

---

### Analytics

Monitor customer interactions.

---

### Lex Integration

Use AI chatbots during calls.

---

## Architecture

```text
Customer Call
      |
      v
Amazon Connect
      |
  +---+---+
  |       |
Agent   Amazon Lex
```

---

## Use Cases

### Customer Service Centers

### Banking Support

### Telecom Support

### Healthcare Helpdesks

---

# Amazon Comprehend

## What is Amazon Comprehend?

Amazon Comprehend is a Natural Language Processing (NLP) service.

It understands and analyzes text.

---

## Features

### Sentiment Analysis

Determines emotions.

Output:

```text
Positive
Negative
Neutral
Mixed
```

---

### Entity Recognition

Identifies:

- Names
- Places
- Dates
- Organizations

Example:

```text
John visited Mumbai on Monday.
```

Output:

```text
Person: John
Location: Mumbai
Date: Monday
```

---

### Key Phrase Extraction

Extracts important phrases.

---

### Language Detection

Identifies language automatically.

---

### Topic Modeling

Finds common topics in documents.

---

### PII Detection

Detects:

- Email
- Phone Number
- Credit Card
- Personal Identifiers

---

## Use Cases

### Customer Reviews

Analyze feedback.

### Social Media

Understand public opinion.

### Compliance

Detect sensitive data.

---

# Amazon Comprehend Medical

## What is Amazon Comprehend Medical?

Healthcare-focused NLP service.

Extracts medical information from clinical text.

---

## Example

Input:

```text
Patient diagnosed with diabetes and prescribed insulin.
```

Output:

```text
Condition: Diabetes
Medication: Insulin
```

---

## Detects

### Conditions

- Diabetes
- Cancer
- Asthma

### Medications

- Insulin
- Aspirin

### Dosages

### Procedures

### Treatments

---

## Use Cases

### Hospitals

Medical record analysis.

### Insurance

Claim processing.

### Research

Clinical studies.

### Healthcare Applications

Patient record automation.

---

# Amazon SageMaker AI

## What is SageMaker?

Amazon SageMaker is a fully managed Machine Learning platform.

Used to:

- Build
- Train
- Deploy
- Monitor

ML models.

---

## ML Lifecycle

```text
Data Collection
      |
      v
Preparation
      |
      v
Training
      |
      v
Evaluation
      |
      v
Deployment
      |
      v
Monitoring
```

---

## Major Components

### SageMaker Studio

Development environment.

### Training Jobs

Model training infrastructure.

### Built-in Algorithms

Pre-built ML algorithms.

### Autopilot

Automatic ML model creation.

### Endpoints

Real-time predictions.

### Ground Truth

Data labeling.

### Feature Store

Store ML features.

---

## Use Cases

### Fraud Detection

### Sales Forecasting

### Demand Prediction

### Recommendation Systems

### Predictive Maintenance

---

# Amazon Kendra

## What is Amazon Kendra?

Amazon Kendra is an intelligent enterprise search service.

Provides Google-like search across company data.

---

## Data Sources

- PDFs
- Word Documents
- Databases
- SharePoint
- Websites

---

## Example

Search:

```text
What is the leave policy?
```

Kendra returns the most relevant answer.

---

## Features

### Natural Language Search

Ask questions naturally.

### Semantic Search

Understands meaning.

### Intelligent Ranking

Best results appear first.

---

## Use Cases

### HR Portals

### Internal Knowledge Bases

### Legal Documentation

### Enterprise Search

---

# Amazon Personalize

## What is Amazon Personalize?

Amazon Personalize is a recommendation engine service.

Uses technology similar to Amazon.com recommendations.

---

## Inputs

### User Data

Information about users.

### Item Data

Information about products.

### Interaction Data

User behavior.

---

## Output

Personalized recommendations.

---

## Architecture

```text
User Activity
      |
      v
Amazon Personalize
      |
      v
Recommendations
```

---

## Use Cases

### E-Commerce

Product recommendations.

### OTT Platforms

Movie recommendations.

### Music Streaming

Song recommendations.

### News Applications

Personalized content.

---

# Amazon Textract

## What is Amazon Textract?

Amazon Textract extracts structured information from documents.

Unlike traditional OCR, it understands document structure.

---

## OCR vs Textract

### OCR

Reads text only.

```text
Invoice
Date
Amount
```

### Textract

Understands meaning.

```text
Invoice Number
Customer Name
Date
Amount
```

---

## Features

### Text Extraction

Extract printed text.

---

### Form Extraction

Read form fields.

Example:

```text
Name
Address
Phone
```

---

### Table Extraction

Extract structured tables.

---

### Handwriting Recognition

Read handwritten text.

---

### Document Understanding

Understands relationships between fields.

---

## Architecture

```text
Document
     |
     v
Amazon Textract
     |
     v
Structured Data
```

---

## Use Cases

### Invoice Processing

### Loan Applications

### Insurance Claims

### Medical Records

### Government Records Digitization

---

# Service Comparison Table

| Service | Purpose |
|----------|----------|
| Amazon Rekognition | Image & Video Analysis |
| Rekognition Content Moderation | Unsafe Content Detection |
| Amazon Transcribe | Speech → Text |
| Amazon Polly | Text → Speech |
| Amazon Translate | Language Translation |
| Amazon Lex | Chatbots & Voice Bots |
| Amazon Connect | Contact Center |
| Amazon Comprehend | NLP & Text Analytics |
| Amazon Comprehend Medical | Medical NLP |
| Amazon SageMaker | Build & Deploy ML Models |
| Amazon Kendra | Enterprise Search |
| Amazon Personalize | Recommendation Engine |
| Amazon Textract | Document Extraction |

---

# End-to-End AI Architecture Example

Insurance Claim Processing System

```text
Customer Uploads Claim Form
           |
           v
      Amazon Textract
           |
           v
Structured Claim Data
           |
           v
Customer Uploads Accident Photo
           |
           v
     Amazon Rekognition
           |
           v
Image Analysis
           |
           v
Customer Calls Support
           |
           v
     Amazon Transcribe
           |
           v
Call Transcript
           |
           v
     Amazon Comprehend
           |
           v
Sentiment Analysis
           |
           v
        Amazon Lex
           |
           v
       AI Chatbot
           |
           v
       Amazon Polly
           |
           v
      Voice Response
           |
           v
     Amazon Translate
           |
           v
 Multi-language Support
           |
           v
      Amazon Connect
           |
           v
 Customer Service Routing
           |
           v
     Amazon SageMaker
           |
           v
 Fraud Detection Model
```

---

# Quick Revision Cheat Sheet

| Question | AWS Service |
|-----------|-------------|
| Analyze Images? | Rekognition |
| Detect Nudity/Violence? | Rekognition Content Moderation |
| Speech to Text? | Transcribe |
| Text to Speech? | Polly |
| Fix Pronunciation? | Polly Lexicon |
| Control Voice Output? | Polly SSML |
| Translate Languages? | Translate |
| Build Chatbots? | Lex |
| Build Contact Center? | Connect |
| Analyze Text? | Comprehend |
| Analyze Medical Text? | Comprehend Medical |
| Build ML Models? | SageMaker |
| Enterprise Search? | Kendra |
| Recommendations? | Personalize |
| Extract Data from Documents? | Textract |

---

# One-Line Memory Trick

```text
Images → Rekognition
Unsafe Images → Content Moderation
Speech to Text → Transcribe
Text to Speech → Polly
Translation → Translate
Chatbot → Lex
Call Center → Connect
Text Analytics → Comprehend
Medical NLP → Comprehend Medical
Machine Learning → SageMaker
Enterprise Search → Kendra
Recommendations → Personalize
Document Processing → Textract
```

---
