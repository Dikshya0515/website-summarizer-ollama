# 🌐 Website Summarizer using Ollama

A Python project that fetches content from any website and generates a concise summary using Ollama AI models.

## 📋 Table of Contents
- [What Does This Project Do?](#what-does-this-project-do)
- [How It Works](#how-it-works)
- [Requirements](#requirements)
- [Installation](#installation)
- [Usage](#usage)
- [Project Structure](#project-structure)
- [Code Explanation](#code-explanation)
- [Troubleshooting](#troubleshooting)

## 🎯 What Does This Project Do?

This project takes any website URL and:
1. **Fetches** all the text content from that website
2. **Cleans** the text by removing unwanted elements (ads, navigation, etc.)
3. **Summarizes** the content using Ollama's AI models
4. **Displays** a clean, readable summary

**Example:**
- Input: `https://en.wikipedia.org/wiki/Artificial_intelligence`
- Output: A 2-3 paragraph summary of the entire Wikipedia article

## 🔧 How It Works

### Step-by-Step Process:

1. **Web Scraping**: Uses `requests` library to download the website's HTML
2. **Content Extraction**: Uses `BeautifulSoup` to parse HTML and extract only text
3. **Text Cleaning**: Removes unnecessary elements like scripts, styles, navigation bars
4. **AI Summarization**: Sends the cleaned text to Ollama's local AI model
5. **Output**: Returns a concise summary of the main points

### Technology Stack:
- **Python 3.x** - Programming language
- **Requests** - Downloads website content
- **BeautifulSoup4** - Parses HTML and extracts text
- **Ollama** - Local AI model for text summarization

## 📦 Requirements

### Software Requirements:
- Python 3.8 or higher
- Ollama installed on your system
- Internet connection (for fetching websites)

### Python Libraries:
```
requests==2.31.0
beautifulsoup4==4.12.2
ollama==0.1.7
```

## 🚀 Installation

### 1. Install Ollama
Download and install Ollama from [https://ollama.ai](https://ollama.ai)

**For Windows/Mac:**
- Download installer from website
- Run the installer
- Ollama will run in the background

**For Linux:**
```bash
curl -fsSL https://ollama.ai/install.sh | sh
```

### 2. Pull an AI Model
```bash
ollama pull llama3.2
```

Other models you can try:
- `ollama pull llama3.1`
- `ollama pull mistral`
- `ollama pull gemma2`

### 3. Clone This Repository
```bash
git clone https://github.com/Dikshya0515/website-summarizer-ollama.git
cd website-summarizer-ollama
```

### 4. Install Python Dependencies
```bash
pip install -r requirements.txt
```

### 5. Start Ollama Service
```bash
ollama serve
```
Keep this terminal window open!

## 💻 Usage

### In Jupyter Notebook:
1. Open `website-summarizer-ollama.ipynb`
2. Run all cells
3. Change the URL to any website you want to summarize

### As a Python Script:
```python
from summarizer import summarize_website

# Summarize any website
url = "https://example.com"
result = summarize_website(url)
```

### Summarize Multiple Websites:
```python
urls = [
    "https://en.wikipedia.org/wiki/Machine_learning",
    "https://www.bbc.com/news",
    "https://techcrunch.com"
]

for url in urls:
    summarize_website(url)
```

## 📁 Project Structure

```
website-summarizer-ollama/
│
├── website-summarizer-ollama.ipynb   # Main Jupyter notebook
├── requirements.txt                   # Python dependencies
├── README.md                          # This file
├── .gitignore                        # Files to ignore in git
└── examples/                         # Example outputs (optional)
```

## 📚 Code Explanation

### Function 1: `fetch_website_content(url)`
**What it does:** Downloads and cleans website content

```python
def fetch_website_content(url):
    # 1. Create headers (pretend to be a browser)
    headers = {'User-Agent': 'Mozilla/5.0...'}
    
    # 2. Download the website
    response = requests.get(url, headers=headers, timeout=10)
    
    # 3. Parse HTML with BeautifulSoup
    soup = BeautifulSoup(response.content, 'html.parser')
    
    # 4. Remove unwanted elements (scripts, styles, navigation)
    for element in soup(['script', 'style', 'nav', 'footer']):
        element.decompose()
    
    # 5. Extract clean text
    text = soup.get_text(separator='\n', strip=True)
    
    # 6. Clean up extra whitespace
    lines = [line.strip() for line in text.splitlines() if line.strip()]
    
    return '\n'.join(lines)
```

**Why each step:**
- **Headers**: Some websites block requests without proper headers
- **BeautifulSoup**: Converts messy HTML into clean, readable text
- **Remove elements**: Gets rid of menus, ads, and other non-content
- **Clean whitespace**: Makes text neat and organized

### Function 2: `summarize_with_ollama(text, model)`
**What it does:** Sends text to Ollama AI for summarization

```python
def summarize_with_ollama(text, model='llama3.2', max_length=4000):
    # 1. Limit text length (AI models have token limits)
    text_to_summarize = text[:max_length]
    
    # 2. Create a prompt for the AI
    prompt = f"""Please provide a clear summary of: {text_to_summarize}"""
    
    # 3. Send to Ollama API
    response = ollama.chat(
        model=model,
        messages=[{'role': 'user', 'content': prompt}]
    )
    
    # 4. Return the summary
    return response['message']['content']
```

**Why each step:**
- **Limit text**: AI models can't process infinite text, so we limit it
- **Prompt**: Tell the AI exactly what we want
- **ollama.chat()**: Sends our text to the local AI model
- **Return**: Gets the AI's summary back

### Function 3: `summarize_website(url, model)`
**What it does:** Combines everything together

```python
def summarize_website(url, model='llama3.2'):
    # 1. Fetch website content
    content = fetch_website_content(url)
    
    # 2. Check if fetching worked
    if content.startswith("Error"):
        print(content)
        return None
    
    # 3. Show progress
    print(f"✓ Content fetched! Length: {len(content)} characters")
    
    # 4. Generate summary using Ollama
    summary = summarize_with_ollama(content, model=model)
    
    # 5. Display results
    print("SUMMARY")
    print(summary)
    
    # 6. Return everything as a dictionary
    return {
        'url': url,
        'content': content,
        'summary': summary
    }
```

## 🔍 How Ollama Works (Explained Like You're 5)

### What is Ollama?
Ollama is like having a super smart robot friend living in your computer!

### How Does It Work?

1. **You download Ollama** → It's like installing a game on your computer
2. **You pull a model** (like llama3.2) → It's like downloading the robot's brain
3. **Your code sends text to Ollama** → Like asking your robot friend a question
4. **Ollama thinks about it** → The robot reads and understands the text
5. **Ollama sends back a summary** → The robot tells you the short version

### Why Use Ollama Instead of ChatGPT?
- ✅ **Free** - No API costs
- ✅ **Private** - Everything stays on your computer
- ✅ **Fast** - No internet needed (after model download)
- ✅ **Customizable** - You can use different AI models

### The Backend Process:

```
Your Code → Ollama Server → AI Model → Processes Text → Returns Summary → Your Code
```

1. **Your code** sends text to Ollama
2. **Ollama server** (running on localhost:11434) receives it
3. **AI model** (llama3.2) processes the text
4. **Model** generates a summary
5. **Ollama** sends summary back to your code

## 🐛 Troubleshooting

### Error: "name 'fetch_website_content' is not defined"
**Solution:** Run all cells in order, or use the single-cell version

### Error: "Connection refused" from Ollama
**Solution:** 
```bash
# Start Ollama service
ollama serve
```

### Error: "Model not found"
**Solution:**
```bash
# Pull the model
ollama pull llama3.2
```

### Website returns empty text
**Solution:** Some websites block scraping. Try a different website like Wikipedia or news sites.

### Summary is too short/long
**Solution:** Modify the `max_length` parameter:
```python
# For longer text to summarize
summarize_with_ollama(content, max_length=8000)
```

## 📝 License

MIT License - Feel free to use this project for learning!

## 👤 Author

GitHub: @Dikshya0515

## 🙏 Acknowledgments

- Ollama team for the amazing local AI models
- BeautifulSoup for easy web scraping
- Python community for great libraries