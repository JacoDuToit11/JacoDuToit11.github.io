---
title: 'Generating Audio Files for Learning Languages'
date: 2024-08-24
# categories: [""]
permalink: /posts/LanguageLearningAudio/
tags:
  - LLM
  - Text to Speech
  - AI for Educaction
---

In this post, I'll walk you through how I generated audio files for assisting people in language learning. 

# Generating Audio Files for Learning Languages
We generate audio files which are designed to repeat English phrases followed by their translations in a different language. This can greatly enhance the learning process by reinforcing vocabulary and phrases through repetition. 

[Github](https://github.com/JacoDuToit11/LanguageLearningAudioGeneration)

## Why This Method of Learning is Useful

This method of learning is particularly effective because it leverages the power of repetition and active recall. By hearing a phrase in English followed by its translation, learners can create strong associations between the two languages. This helps in better retention of vocabulary and improves pronunciation as well. The spaced repetition incorporated into this method ensures that learners are repeatedly exposed to the phrases, which solidifies their learning over time. The other massive benefit is that this approach allows users to learn a language while doing other activities such as driving or walking.

## Overview
- **Text Generation**: We generate a series of lessons that contain English phrases and their translations.
- **Text Processing**: The generated text is cleaned and structured, ensuring that it's properly formatted for conversion to audio.
- **Audio Generation**: Each phrase and its translation are converted into audio segments using a text-to-speech model. Silence is added between phrases to create a natural learning experience.
- **Exporting Audio Files**: The final audio files are exported as MP3 files.

## Configuration and Imports
The first step is setting up a configuration file (`config.yaml`) to determine the language and difficulty level of the lessons.

Here’s what the `config.yaml` file might look like:

```yaml
settings:
  language: German
  difficulty_level: intermediate
```

We also use the following imports:
```python
from openai import OpenAI
from pydub import AudioSegment
import os, json, textwrap, re, yaml
```

## Text Generation
Setup for text generation:
```python
openai_client = OpenAI(api_key=os.environ['OPENAI_API_KEY'])
# Model used to generate lessons
model = "gpt-4o"
num_lessons = 10
phrases_per_lesson = 25

with open('config.yaml', 'r') as file:
    config = yaml.safe_load(file)

language = config['settings']['language']
difficulty_level = config['settings']['difficulty_level']
```

First, we generate the lessons through the use of a Large Language Model (LLM) from the OpenAI API as follows:
```python
system_instruction = 'You are a language teacher creating a podcast to help people learn a language.'

prompt = textwrap.dedent(f'''
    Provide exactly {num_lessons} different lessons for people learning {language}. 
    You should give a phrase in English, and then the translation in {language}. 
    Each lesson should contain {phrases_per_lesson} phrases. 
    These lessons should be at a {difficulty_level} level. 
    You can make up details like names and years.
    The format should look like this: 

    ### Lesson 1: title...
    Summary of what we will be learning in this lesson...
    ...
    ---
    ### Lesson 2: title...
    ...
    ''')

messages = [{'role':'system', 'content':system_instruction},
            {'role':'user', 'content':prompt}]

lessons = openai_client.chat.completions.create(
    model=model,
    messages=messages
).choices[0].message.content
```

where an example of this output looks like this:
```
Lesson 1: Basic Greetings and Farewells
In this lesson, we will learn how to greet people in German and say goodbye.
1. Hello. Hallo.
2. Goodbye. Auf Wiedersehen.
3. Good morning. Guten Morgen.
4. Good afternoon. Guten Tag.
...
```

## Text Processing
We process the output of the LLM to prepare it for conversion to a seperate audio file for each lesson.

First we use regex to extract each lesson
```python
lesson_pattern = re.compile(r'### (Lesson ([\w]+):[\S\s]+?)---')
lessons_extracted = re.findall(lesson_pattern, lessons)
```

Then we process the text for each lesson as follows:
```python
lessons_dict = {}
for lesson in lessons_extracted:
    lesson_output = process_lesson_text(lesson[0])
    lessons_dict[lesson[1]] = lesson_output

with open(f'{output_path}lessons.json', 'w') as file:
    json.dump(lessons_dict, file, indent=4)

def process_lesson_text(text):
    # Remove number at start of line
    pattern = r"([0-9]+[.])\s"
    text = re.sub(pattern, r"", text)

    text_lines = text.splitlines()
    if '' in text_lines:
        text_lines.remove('')
    
    output = []
    for i, line in enumerate(text_lines):
        if i < 2:
            output.append(line)
        else:
            # Split sentences
            pattern = re.compile(r'\s*([^.!?]*[.!?])')
            sentences = pattern.findall(line)
            output.extend(sentences)
    return output
```

such that the output json files look like:
```json
{
    "1": [
        "Lesson 1: Basic Greetings and Farewells",
        "In this lesson, we will learn how to greet people in German and say goodbye.",
        "Hello.",
        "Hallo.",
        "Goodbye.",
        "Auf Wiedersehen.",
        ...
    ],
    "2": [
    "Lesson 2: Introducing Oneself and Others",
    "This lesson focuses on introducing yourself and getting to know others.",
    "Who is that?",
    "Wer ist das?",
    "This is my friend.",
    "Das ist mein Freund.",
    ...
    ]
}
```

**Audio Generation**

Setup for audio generation
```python
silence_duration1 = 2000 # milliseconds
silence_duration2 = 3000 # milliseconds

text_lessons_path = f'../data/{language}/{difficulty_level}/lessons.json'
speech_lessons_path = f'../data/{language}/{difficulty_level}/'

speech_model = 'tts-1'
```

Generating audio file for each lesson:
```python
with open(text_lessons_path, 'r') as file:
    lessons_dict = json.load(file)

for lesson_content in lessons_dict.values():
    text_to_speech(lesson_content)

def text_to_speech(lesson_content):
    final_audio = AudioSegment.silent(duration=0)
    # Silence segments with different lengths
    silence_segment1 = AudioSegment.silent(duration=silence_duration1)
    silence_segment2 = AudioSegment.silent(duration=silence_duration2)

    for i in range(0, len(lesson_content), 2):
        # Generate first phrase (English)
        response1 = openai_client.audio.speech.create(
            model=speech_model,
            voice="onyx", 
            input=lesson_content[i],
        )

        temp_audio_file1 = f"tmp1.mp3"
        with open(temp_audio_file1, "wb") as f:
            f.write(response1.content) 

        # Generate second phrase (Translation)
        response2 = openai_client.audio.speech.create(
            model=speech_model,
            voice="onyx", 
            input=lesson_content[i+1],
        )

        temp_audio_file2 = f"tmp2.mp3"
        with open(temp_audio_file2, "wb") as f:
            f.write(response2.content) 

        segment1_audio = AudioSegment.from_file(temp_audio_file1)
        segment2_audio = AudioSegment.from_file(temp_audio_file2)
        
        # Adding phrase and translation 
        final_audio += segment1_audio
        final_audio += silence_segment1
        final_audio += segment2_audio

        if i > 0:
            # Repeating phrase and translation 
            final_audio += silence_segment1
            final_audio += segment1_audio
            final_audio += silence_segment2
            final_audio += segment2_audio
        final_audio += silence_segment1

        # Clean up the temporary files
        os.remove(temp_audio_file1)
        os.remove(temp_audio_file2)
    final_audio.export(f"{speech_lessons_path}{lesson_content[0]}.mp3", format="mp3")
```

Here is an example output for beginner lessons in German:
<audio controls>
  <source src="/files/LanguageLearningAudio/Lesson 1: Basic Greetings and Farewells.mp3" type="audio/mpeg">
  Your browser does not support the audio element.
</audio>

## Conclusion
In this post we have:
- Generated language learning lessons with an LLM.
- Processed and prepared the text for each lesson.
- Converted each lesson to an audio file.

Thank you for following along. Now go try it out yourself!





