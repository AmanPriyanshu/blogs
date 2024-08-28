---
title: "FRACTURED-SORRY-Bench: Unraveling AI Safety through Nature-Inspired Techniques"
date: 2024-08-28
draft: false
categories: ["AI Security", "Bio-Inspired Computing"]
tags: ["FRACTURED-SORRY-Bench", "AI Safety", "Jailbreaks", "Prompt-Injections"]
cover:
    image: "images/fractured_sorry_bench.png"
---

# FRACTURED-SORRY-Bench: When Nature Meets AI Safety

Hello, fellow AI enthusiasts! 🤖 Today, I wanted to dive into the FRACTURED-SORRY-Bench framework and dataset we just released. Check out the [website](https://amanpriyanshu.github.io/FRACTURED-SORRY-Bench/) and [github](https://github.com/AmanPriyanshu/FRACTURED-SORRY-Bench/) for the dataset!

## The FRACTURED-SORRY Saga: A Tale of Adaptation and Decomposition

Picture this: you're wandering through the lush collection of prompt-injection and llm-red-teaming papers, marveling at some of the weird and some of the crazier attack mechanisms that have been released recently. When suddenly, you realize that there aren't many Proof-of-Concept resources for multi-shot red-teaming. That's essentially the story behind creating FRACTURED-SORRY-Bench.

### What's in a Name?

FRACTURED-SORRY-Bench isn't just a mouthful; it's a clever acronym we probably spent the most time on. It stands for:

- **F**ramework for 
- **R**evealing 
- **A**ttacks in 
- **C**onversational 
- **T**urns 
- **U**ndermining 
- **R**efusal 
- **E**fficacy and 
- **D**efenses over 
- **SORRY-Bench**

## The FRACTURED Approach: Divide and Conquer

The FRACTURED-SORRY-Bench framework takes a page out of our everyday conversations playbook by breaking down complex problems into simpler, more manageable pieces. Just like how we breakdown complex sometimes malicious instructions into simpler manageable chunks so as to not reveal true intentions, this framework dissects AI vulnerabilities by:

1. Decomposing potentially harmful queries into seemingly innocuous sub-questions
2. Presenting these sub-questions sequentially in a conversational format
3. Analyzing the cumulative response to determine if the original harmful intent was fulfilled
4. Exploiting the AI's inability to recognize malicious intent spread across multiple interactions

## From Theory to Practice: The Jailbreak Jamboree

Now, let's get to the juicy part – the jailbreaks! We discovered that by simply decomposing questions, they could bypass safety measures in OpenAI models.

Here's a taste of what we found:

- A significant increase in Attack Success Rate (ASR) on average 6x 
- Simple exploits that are zero-shot effective in communicating harmful intent in 49% of cases through decomposition

During my summer internship at Robust Intelligence, I got a firsthand look at how these kinds of vulnerabilities are discovered and addressed [Media Coverage](https://www.theregister.com/2024/07/29/meta_ai_safety/) [Jailbreak Meta's Prompt-Guard LLaMA3.1 Family within 24 hours](https://www.robustintelligence.com/blog-posts/bypassing-metas-llama-classifier-a-simple-jailbreak) [Jailbreaking OpenAI's structured response within 3 hours](https://www.robustintelligence.com/blog-posts/bypassing-openais-structured-outputs-jailbreak). Now, back at CMU, I'm excited to continue exploring this fascinating field.

## The Moral of the Story: Stay FRACTURED, My Friends

So, what can we learn from this decomposed madness? A few key takeaways:

1. **Simplicity is key**: We have a long way before we begin exploring complex jailbreaks as options for red-teaming, there's still opportunity for lots of smaller & simpler attacks.
2. **Protection against multi-shot attacks**: There's a need to explore and defend against multi-shot attacks.

## Conclusion: The Adventure Continues

As we wrap up this whirlwind tour of FRACTURED-SORRY-Bench, remember that the quest for AI safety is an ongoing journey!!

Also, thanks a tonne! to my co-author [**Supriti Vijay**](https://supritivijay.github.io)

---

*P.S. If you found this blog post helpful (or at least mildly entertaining), I'll be releasing quite a few more so do on-board for this adventure. Also, if you want to chat or collaborate on a research project together do not hesitate to reach out. My email is: amanpriyanshusms2001[at]gmail[dot]com* 🔬