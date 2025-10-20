# 🎯 YouTube Video Summarizer — Lab 1 (Tips Hindawi LLLs Training)

## 📖 Overview

This project was developed as part of **Tips Hindawi LLLs Training – Lab 1**.
The objective was to build a Python program capable of:

1. Taking a YouTube video link as input
2. Automatically extracting the video’s transcript
3. Summarizing the transcript into a concise, readable paragraph

This project introduces key skills in:

* API usage
* Natural Language Processing (NLP)
* Text summarization using pre-trained models

---

## ⚙️ Features

✅ Extracts YouTube video ID from any valid URL format
✅ Fetches the transcript using **YouTubeTranscriptApi**
✅ Splits long transcripts into smaller chunks to handle model limits
✅ Summarizes each chunk and combines them into a final summary
✅ Runs smoothly on **CPU or GPU (Kaggle)** environments

---

## 🧩 Project Structure

```
├── lap-1.ipynb        # Jupyter Notebook containing the full implementation
├── Lab_1.pdf          # Task description document
├── README.md          # (this file)
```

---

## 🚀 Installation & Setup

### 1. Clone or open in Kaggle / Colab

If using Kaggle, just upload the `.ipynb` file and run all cells.

Or, to run locally:

```bash
git clone <your-repo-link>
cd youtube-summarizer
```

### 2. Install dependencies

```bash
pip install youtube-transcript-api transformers torch
```

*(If you face CUDA issues on Kaggle, the code automatically switches to CPU.)*

---

## 🧠 How It Works

### 1️⃣ Extract the Video ID

```python
from urllib.parse import urlparse, parse_qs

def extract_video_id(url: str) -> str:
    parsed = urlparse(url)
    qs = parse_qs(parsed.query)
    if 'v' in qs:
        return qs['v'][0]
    if parsed.netloc in ['youtu.be', 'www.youtu.be']:
        return parsed.path.lstrip('/')
    raise ValueError(f"No video id found in URL: {url}")
```

### 2️⃣ Fetch the Transcript

```python
from youtube_transcript_api import YouTubeTranscriptApi
transcript = YouTubeTranscriptApi().fetch(video_id, languages=['en'])
text = " ".join(snippet["text"] for snippet in transcript)
```

### 3️⃣ Summarize the Transcript

```python
from transformers import pipeline
summarizer = pipeline("summarization", model="facebook/bart-large-cnn")

summary = summarizer(text, max_length=200, min_length=50, do_sample=False)
print(summary[0]["summary_text"])
```

---

## ⚡ Advanced: Chunking for Long Videos

Large transcripts can exceed the model’s token limit.
To handle this, the text is split into smaller **chunks**:

```python
def chunk_text(text, max_chars=2500):
    return [text[i:i+max_chars] for i in range(0, len(text), max_chars)]
```

Each chunk is summarized separately and then combined into one final summary.

---

## 🧾 Example Output

**Input:**
`https://youtu.be/vStJoetOxJg`

**Output Example:**

```
The video discusses the process of creating AI-based text summarizers.
It explains how to use APIs, extract transcripts, and generate meaningful
summaries using pre-trained transformer models.
```

---

## 🧰 Technologies Used

* **Python 3**
* **Transformers (Hugging Face)**
* **YouTube Transcript API**
* **Torch (PyTorch backend)**

---

## 💻 Environment

* Developed and tested on **Kaggle GPU (Tesla T4)**
* Works on **Google Colab** and local environments as well

---

## 🧑‍🎓 Author

**Abdallah Elhoseny**
AI Engineer | Tips Hindawi LLLs Trainee