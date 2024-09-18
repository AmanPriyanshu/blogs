---
title: "API-LLM-Hub: Simplifying LLM-API integration for Static Pages"
date: 2024-09-18
draft: false
categories: ["AI Development", "Web Technologies"]
tags: ["API-LLM-Hub", "Browser AI", "JavaScript", "OpenAI", "Anthropic", "TogetherAI", "Google Gemini", "Natural Language Processing"]
cover:
    image: "images/api-llm-hub-demo.png"
---

Hey there, fellow code enthusiasts and AI wranglers! 🖐️🤖 You know that feeling when you're knee-deep in a project, trying to get multiple AI models to play nice in your browser? Yeah, I've been there. Cue the frustrated sighs, the endless searches over GitHub issues 😢, and the "why-isn't-this-working" hair-pulling sessions.

After one too many nights wrestling with backends, CORS issues, and the general chaos of integrating various AI APIs, I decided enough was enough. There had to be a simpler way, right? Something that didn't require installing a bunch of npm builds, juggling APIs, or managing a backend server farm just to get a chatbot running on a static page.

Turns out, there wasn't a simple solution – shocking, I know. So, armed with nothing but sleeplessness and caffeine, I decided to build it myself. Enter API-LLM-Hub: my first rodeo in JavaScript packaging and a testament to what happens when a Python AI nerd ventures into the wild west of front-end development. It's been a painful journey figuring out things which I have kept stalling all through my undergrad. But hey, if I can wrangle this into existence while learning what a promise is, I can probably claim 'Full-Stack' on my resume now! 🍪💻🚀

## The Spark of Inspiration

Ever tried juggling multiple AI APIs in a browser? 😃 

It's about as fun as catching Zapdos with a normal ball in FireRed. That's why I cooked up API-LLM-Hub – a vanilla JavaScript library that lets you tap into various AI language model APIs right from your browser, no backend required.

## What's in the Box?

API-LLM-Hub is like that Swiss Army knife you always wished you had for static-page AI development (because who doesn't dream about that, right?). Here's what you're getting:

1. **Browser Besties**: This thing runs directly in your browser. No server-side shenanigans required!
2. **CORS Conqueror**: I nearly cried dealing with Anthropic's bizarre API examples—especially since they don't have any for CORS issues!
3. **Multi-AI Handling**: Switch between OpenAI, Anthropic, TogetherAI, and Google's Gemini.
4. **Vanilla JS Goodness**: No fancy frameworks or build steps. Just pure, unadulterated JavaScript joy.

## An Example I Wish I'd Seen Before Starting This Stupic Project

```html
<script type="module">
  import APILLMHub from 'https://amanpriyanshu.github.io/API-LLM-Hub/unified-llm-api.js';

  async function runTest() {
    const ai = new APILLMHub({
      provider: 'openai',
      apiKey: 'your-api-key',
      model: 'gpt-3.5-turbo'
    });

    await ai.initialize();
    const response = await ai.sendMessage("Hello, AI!");
    console.log(response);
  }

  runTest();
</script>
```

That's it. Seriously. You're now conversing with an AI in your browser.

## Why You'll Love It (and Why Your Browser Will Too)

1. **Static Site Sorcery**: GitHub Pages? Netlify? No problem. API-LLM-Hub plays nice with all your favorite static hosts.
2. **AI Provider Roulette**: AI Provider Roulette: Switch between AI providers with a single line of code—well, actually a function call... okay, maybe just one variable (`provider`), and possibly another (`model`).

## Real-World Magic (Because static pages are just crying out for embedded chatbots, aren't they?)

You can now integrate any static page with a chatbot using the user's API keys or add some LLM flair to your browser extension. Since I worked on implementing semantic search over recent YC startup batches in [YC-Dendrolinguistics](https://amanpriyanshu.github.io/YC-Dendrolinguistics/) (I highly recommend checking out the [Startup Linguistic Trees blog](https://amanpriyanshu.github.io/blogs/posts/2024/startup-linguistic-trees/)), combining that search with this system could easily implement RAGs (Retrieval-Augmented Generation) on static webpages—which is both crazy and stupid. With API-LLM-Hub, you're only limited by your imagination (and maybe your API usage limits—but that's a problem for future you).

## Wrapping Up

And there you have it, folks—API-LLM-Hub in all its simplicity. It's a small step towards making LLM-API integration on static pages a bit simpler and a lot more fun. Give it a try!

---

*P.S. If you found this little AI adventure intriguing (or at least mildly entertaining), stay tuned! There's plenty more where this came from. And hey, if you want to collaborate on some wild browser-based AI experiment, don't hesitate to reach out. My email is still amanpriyanshusms2001[at]gmail[dot]com. Let's make the web a little smarter together!* 🌐🤖💡