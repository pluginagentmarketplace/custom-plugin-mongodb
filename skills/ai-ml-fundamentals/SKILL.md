---
name: ai-ml-fundamentals
description: Master artificial intelligence and machine learning. Learn model development, training, deep learning, NLP, computer vision, and LLM deployment. Use when building intelligent systems, analyzing data, or implementing AI features.
---

# AI & ML Fundamentals Skill

Master artificial intelligence and machine learning with comprehensive guidance on model development and deployment.

## Quick Start

### Scikit-learn Classification
```python
from sklearn.model_selection import train_test_split
from sklearn.preprocessing import StandardScaler
from sklearn.ensemble import RandomForestClassifier
from sklearn.metrics import accuracy_score, classification_report

# Load and prepare data
X, y = load_data()
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2)

# Preprocess
scaler = StandardScaler()
X_train = scaler.fit_transform(X_train)
X_test = scaler.transform(X_test)

# Train model
model = RandomForestClassifier(n_estimators=100)
model.fit(X_train, y_train)

# Evaluate
predictions = model.predict(X_test)
print(f"Accuracy: {accuracy_score(y_test, predictions)}")
print(classification_report(y_test, predictions))
```

### PyTorch Neural Network
```python
import torch
import torch.nn as nn
from torch.optim import Adam

class NeuralNetwork(nn.Module):
    def __init__(self):
        super().__init__()
        self.fc1 = nn.Linear(784, 128)
        self.fc2 = nn.Linear(128, 64)
        self.fc3 = nn.Linear(64, 10)
        self.relu = nn.ReLU()

    def forward(self, x):
        x = self.relu(self.fc1(x))
        x = self.relu(self.fc2(x))
        x = self.fc3(x)
        return x

# Training loop
model = NeuralNetwork()
optimizer = Adam(model.parameters())
criterion = nn.CrossEntropyLoss()

for epoch in range(10):
    for batch, (X, y) in enumerate(train_loader):
        output = model(X)
        loss = criterion(output, y)
        optimizer.zero_grad()
        loss.backward()
        optimizer.step()
```

### Transformer Model with HuggingFace
```python
from transformers import AutoTokenizer, AutoModelForSequenceClassification, pipeline

# Text classification with pre-trained model
classifier = pipeline("sentiment-analysis", model="distilbert-base-uncased-finetuned-sst-2-english")
result = classifier("This movie is amazing!")
print(result)  # [{'label': 'POSITIVE', 'score': 0.9998}]

# Fine-tuning custom model
model_name = "bert-base-uncased"
tokenizer = AutoTokenizer.from_pretrained(model_name)
model = AutoModelForSequenceClassification.from_pretrained(model_name, num_labels=2)

# Training code...
```

### LLM with Prompt Engineering
```python
from openai import OpenAI

client = OpenAI()

def ask_llm(prompt, system_prompt=None):
    messages = []

    if system_prompt:
        messages.append({"role": "system", "content": system_prompt})

    messages.append({"role": "user", "content": prompt})

    response = client.chat.completions.create(
        model="gpt-4",
        messages=messages,
        temperature=0.7,
        max_tokens=500
    )

    return response.choices[0].message.content

# Few-shot learning
prompt = """Classify these as positive or negative reviews:

Review: "Great product, highly recommend!"
Classification: POSITIVE

Review: "Awful experience, waste of money"
Classification: NEGATIVE

Review: "Not bad, but could be better"
Classification:"""

result = ask_llm(prompt)
```

## Advanced Topics

### Deep Learning
- Convolutional Neural Networks (CNNs)
- Recurrent Neural Networks (RNNs/LSTMs)
- Transformers and attention mechanisms
- Generative models (GANs, VAEs)

### Natural Language Processing
- Text preprocessing and tokenization
- Word embeddings (Word2Vec, GloVe, FastText)
- Language models and transformers
- Named entity recognition and sentiment analysis

### Computer Vision
- Image classification and detection
- Semantic and instance segmentation
- Object tracking and pose estimation
- Image generation and style transfer

### MLOps
- Model versioning and experiment tracking
- Feature stores and pipelines
- Model serving and API deployment
- Monitoring and retraining strategies

### Large Language Models
- Fine-tuning techniques
- Prompt engineering and few-shot learning
- Retrieval-Augmented Generation (RAG)
- Function calling and tool integration

## Resources

- [PyTorch Docs](https://pytorch.org)
- [TensorFlow Docs](https://www.tensorflow.org)
- [HuggingFace Documentation](https://huggingface.co/docs)
- [Scikit-learn Docs](https://scikit-learn.org)
- [Papers with Code](https://paperswithcode.com)
