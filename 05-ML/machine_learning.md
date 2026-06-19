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

## Use Cases

### Company Names

Correct brand pronunciation.

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

# Amazon Translate

## What is Amazon Translate?

Amazon Translate converts text from one language to another.

Uses Neural Machine Translation (NMT).

# Amazon Lex

## What is Amazon Lex?

Amazon Lex is a service for building conversational chatbots and voice bots.

It uses the same technology behind Alexa.

# Amazon Connect

## What is Amazon Connect?

Amazon Connect is a cloud-based contact center solution.

Used to build customer support centers.

# Amazon Comprehend

## What is Amazon Comprehend?

Amazon Comprehend is a Natural Language Processing (NLP) service.

It understands and analyzes text.

# Amazon Comprehend Medical

## What is Amazon Comprehend Medical?

Healthcare-focused NLP service.

Extracts medical information from clinical text.

# Amazon SageMaker AI

## What is SageMaker?

Amazon SageMaker is a fully managed Machine Learning platform.

# Amazon Kendra

## What is Amazon Kendra?

Amazon Kendra is an intelligent enterprise search service.

Provides Google-like search across company data.

# Amazon Personalize

## What is Amazon Personalize?

Amazon Personalize is a recommendation engine service.

Uses technology similar to Amazon.com recommendations.


# Amazon Textract

## What is Amazon Textract?

Amazon Textract extracts structured information from documents.

Unlike traditional OCR, it understands document structure.
