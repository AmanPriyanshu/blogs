---
title: "Contra-Topic-bottleneck-t5: Efficient Topic Extraction Without the Computational Overhead"
date: 2024-11-06
draft: false
categories: ["AI Research", "Natural Language Processing", "Model Optimization"]
tags: ["Topic Extraction", "T5", "Lightweight AI", "Efficiency", "NLP"]
cover:
    image: "images/contra-topic-banner.png"
---

**Quick Links:**
[Model on HuggingFace](https://huggingface.co/AmanPriyanshu/Contra-Topic-bottleneck-t5-large) | [Interactive Demo](https://colab.research.google.com/drive/1_SuTiL3QS-PUYjSrugqqD5mQlMv8Hbfc?usp=sharing)

When it comes to topic extraction, the AI world seems fixated on massive models and expensive compute. But what if there was a simpler way? 🤔 

<div style="display: flex; justify-content: space-between;">
  <img src="https://raw.githubusercontent.com/AmanPriyanshu/blogs/refs/heads/main/content/posts/2024/Contra-Topic/images/contra-topic-banner.png" alt="Topic Modeling" style="width: 96%;"/>
</div>

## The Genesis: Simplicity Through Linear Transformation

Picture this: There I was, looking for an open-source solution to extract topics from text at scale. The available options were either massive language models or complex fine-tuning pipelines. That's when it hit me – what if we could leverage the semantic structure of existing embeddings with just a linear transformation?

## The Architecture: Elegant in Its Simplicity

The solution combines three key components:

1. **Bottleneck T5 Large** as our backbone (credit to @thesephist)
2. Domain-specific 1024×1024 transformation matrices
3. A straightforward mapping from content to topic space

What makes this approach powerful is its minimal computational footprint combined with surprising effectiveness.

## The Numbers That Matter

Here's how it performs across different domains:

| Dataset | Training MSE | Testing MSE | Inter-topic MSE |
|---------|-------------|-------------|-----------------|
| ArXiv   | 0.00225     | 0.00268     | 0.00620        |
| TopicSUM| 0.00252     | 0.00255     | 0.00737        |
| MSD     | 0.00174     | 0.00197     | 0.00566        |

These metrics show consistent performance across domains while maintaining clear topic separation.

## Implementation: Straightforward and Efficient

Here's a complete example of how to use the model:

```python
import torch
import requests
from transformers import AutoTokenizer, AutoModelForCausalLM

class BottleneckT5Autoencoder:
    def __init__(self, model_path: str, device='cpu'):
        self.device = device
        self.tokenizer = AutoTokenizer.from_pretrained(model_path, model_max_length=512)
        self.model = AutoModelForCausalLM.from_pretrained(
            model_path, 
            trust_remote_code=True
        ).to(device)
        self.model.eval()

    def embed(self, text: str) -> torch.FloatTensor:
        inputs = self.tokenizer(text, return_tensors='pt').to(self.device)
        decoder_inputs = self.tokenizer('', return_tensors='pt').to(self.device)
        return self.model(
            **inputs,
            decoder_input_ids=decoder_inputs['input_ids'],
            encode_only=True,
        )[0]

    def generate_from_latent(self, latent: torch.FloatTensor, max_length=512):
        dummy_text = '.'
        dummy = self.embed(dummy_text)
        perturb_vector = latent - dummy
        self.model.perturb_vector = perturb_vector
        input_ids = self.tokenizer(dummy_text, return_tensors='pt').to(self.device).input_ids
        output = self.model.generate(
            input_ids=input_ids,
            max_length=max_length,
            do_sample=True,
            temperature=1.0,
            top_p=0.9,
            num_return_sequences=1,
        )
        return self.tokenizer.decode(output[0], skip_special_tokens=True)

# Load model and transformation matrix
model_path = "AmanPriyanshu/Contra-Topic-bottleneck-t5-large"
matrix_url = 'https://huggingface.co/AmanPriyanshu/Contra-Topic-bottleneck-t5-large/resolve/main/transformation_matrix_arxiv.pt'

autoencoder = BottleneckT5Autoencoder(model_path=model_path)
transformation_matrix = torch.load(
    requests.get(matrix_url).content,
    weights_only=False
).float()

# Extract topic from content
content = "Your text here..."
content_embedding = autoencoder.embed(content)
topic_embedding = content_embedding @ transformation_matrix
topic = autoencoder.generate_from_latent(topic_embedding)
print(f"Extracted topic: {topic}")
```

## Why This Matters

The advantages of this approach are clear:

- Sub-second inference times
- Minimal memory requirements (~4MB per domain)
- No cloud computing dependencies
- Transparent transformation process

## Current Limitations

Let's be clear about what this model can and can't do:

1. **Domain Specificity**: Each transformation matrix is optimized for its specific domain (provided ArXiv (Research), MSD (Medical), and TopicSum (Dialogues) )
2. **Fixed Dimensionality**: The approach is constrained to Bottleneck T5's 1024D embedding space
3. **Linear Transformation Limits**: We assume a linear relationship between content and topic spaces

## Future Directions

There's still plenty of room for improvement:

1. Investigating non-linear transformations for complex topic relationships
2. Developing domain adaptation techniques
3. Exploring dimension reduction possibilities

## Conclusion: Efficiency Through Simplicity

This project demonstrates that effective topic extraction doesn't always require massive models or expensive compute. Through careful use of linear transformations and existing embeddings, we can achieve practical results with minimal resources. 

Is it perfect? No. But it offers a pragmatic solution for those needing efficient, scalable topic extraction without the computational overhead of larger language models.

---

*P.S. If you're interested in exploring this approach further or have ideas for improvements, feel free to reach out at amanpriyanshusms2001[at]gmail[dot]com. The best solutions often come from collaborative thinking!* 🚀✨