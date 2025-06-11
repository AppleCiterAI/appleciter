# AppleCiter

A school-safe AI chatbot for finding trustworthy academic sources (.edu, .gov) using APIs like arXiv.

## Try It Out
Use AppleCiter at: [https://appleciterchatbot.onrender.com](https://appleciterchatbot.onrender.com)

## Features
- Queries academic sources safely.
- Filters inappropriate content for student use.
- Deployed on Render, embeddable in websites like Wix or GitHub Pages.

## Preview
![AppleCiter Preview](path/to/screenshot.png)
from flask import Flask, request, render_template
from profanity import profanity
import requests

app = Flask(__name__)

@app.route('/')
def home():
    return render_template('index.html')  # Chatbot UI

@app.route('/chat', methods=['POST'])
def chat():
    query = request.form.get('query')
    # Basic safety filter for school-safe content
    if profanity.contains_profanity(query):
        return {"error": "Inappropriate query detected"}, 400
    try:
        # Query arXiv API (example)
        response = requests.get(f'http://export.arxiv.org/api/query?search_query={query}&max_results=5')
        results = response.text  # Process XML as needed (simplified)
        return {"results": results}
    except Exception as e:
        return {"error": str(e)}, 500

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=10000)
    <!DOCTYPE html>
<html>
<head>
    <title>AppleCiter Chatbot</title>
    <style>
        body { font-family: Arial, sans-serif; text-align: center; margin: 20px; }
        iframe { border: none; width: 100%; max-width: 800px; height: 600px; }
        @media (max-width: 600px) { iframe { height: 400px; } }
    </style>
</head>
<body>
    <h1>AppleCiter: Academic Source Finder</h1>
    <p>Use the chatbot below to find trustworthy academic sources.</p>
    <iframe src="https://appleciterchatbot.onrender.com" title="AppleCiter Chatbot"></iframe>
</body>
</html>
services:
  - type: web
    name: appleciter
    env: python
    plan: free
    buildCommand: pip install -r requirements.txt
    startCommand: gunicorn app:app
    autoDeploy: false
