---
title: "LinearCosine: When AI Researchers Decided Multiplication was Too Mainstream"
date: 2024-10-21
draft: false
categories: ["AI Research", "Algorithm Optimization", "Benchmarking", "Quantization"]
tags: ["LinearCosine", "L-Mul Algorithm", "Cosine Similarity", "Energy-Efficient AI", "Benchmarking", "Green-AI"]
cover:
    image: "images/linearcosine-benchmark.png"
---

Hey there, optimization seekers and efficiency enthusiasts! 📊🧮 Today, we're diving into a world where even basic arithmetic operations are up for debate. Buckle up as we explore [LinearCosine, an experiment that asks: "Do we really need multiplication for AI?"](https://amanpriyanshu.github.io/LinearCosine/)

**Quick Links to skip the talk:**
[Project Website - Linear Cosine](https://amanpriyanshu.github.io/LinearCosine/) | [GitHub Repo](https://github.com/AmanPriyanshu/LinearCosine) | [Original Paper](https://arxiv.org/abs/2410.00907)

## The Paper That Started It All

During my fall break, while I was supposed to be relaxing, my roommate Yash Maurya forwarded me a fascinating paper by Hongyin Luo and Wei Sun titled "[Addition is All You Need for Energy-efficient Language Models](https://arxiv.org/abs/2410.00907)". I was immediately intrigued by their approach to modify one of the core fundamental computations in AI, multiplication. This project builds upon my previous work on [in-browser vanilla js semantic search, such as YC-Dendrolinguistics](https://amanpriyanshu.github.io/blogs/posts/2024/startup-linguistic-trees/), where I implemented a [cosine similarity-based information retrieval system for YC startups](https://amanpriyanshu.github.io/YC-Dendrolinguistics/). LinearCosine takes this a step further by exploring ways to make these fundamental calculations more energy-efficient.

## What is LinearCosine?

LinearCosine is my experimental project to explore the potential of the [L-Mul algorithm for energy-efficient cosine similarity calculations](https://amanpriyanshu.github.io/LinearCosine/). Here's what it's all about:

1. **L-Mul Algorithm Implementation**: Adapting Luo and Sun's approach for cosine similarity.
2. **Cosine Similarity Optimization**: Applying this method to a common calculation in AI and ML.
3. **Comprehensive Benchmarking**: Rigorously testing the performance against traditional methods.

## The Math Behind the Madness

Let's break down the key mathematical concepts:

### Standard Floating-Point Multiplication

The traditional method multiplies two floating-point numbers as follows:

```
Mul(x,y) = (1 + x_m) * 2^(x_e) * (1 + y_m) * 2^(y_e) = (1 + x_m + y_m + x_m * y_m) * 2^(x_e + y_e)
```

Where `x_m` and `y_m` are mantissas, and $x_e$ and $y_e$ are exponents.

### L-Mul Algorithm

The L-Mul method approximates this multiplication:

```
L-Mul(x,y) = (1 + x_m + y_m + 2^(-l(m))) * 2^(x_e + y_e)
```

Where `l(m)` is defined as:

```
l(m) = m if m ≤ 3
l(m) = 3 if m = 4
l(m) = 4 if m > 4
```

Here, `m` represents the number of mantissa bits.

## The Experiment

In LinearCosine, I implemented the L-Mul algorithm and applied it to cosine similarity calculations. I then conducted extensive benchmarking to compare its performance and accuracy against traditional floating-point multiplication.

## Results: Efficiency Meets Accuracy

**Here's one of the tables from the experiments:** The table below illustrates the performance of cosine similarity calculations using different levels of mantissa precision. The L-Mul approach achieves significant time reduction while maintaining an acceptable mean squared error (_depends on your usecase_), making it a fast and efficient approximation for high-speed computations.

| Mantissa Bits | L-Mul MSE     | L-Mul Time (ns) | Mul MSE       | Mul Time (ns) | Time Reduction (%) |
|---------------|---------------|-----------------|---------------|---------------|--------------------|
| 2             | 9.45862e-06   | 507             | 4.07024e-08   | 670           | 24.3284%           |
| 3             | 1.84347e-05   | 459             | 4.07024e-08   | 597           | 23.1156%           |
| 4             | 1.84347e-05   | 383             | 4.07024e-08   | 516           | 25.7752%           |
| 5             | 4.44707e-05   | 477             | 1.93169e-06   | 629           | 24.1653%           |
| 6             | 1.78036e-05   | 477             | 6.42665e-08   | 625           | 23.68%             |
| 7             | 2.11236e-05   | 498             | 2.21875e-08   | 654           | 23.8532%           |
| 8             | 2.0259e-05    | 442             | 1.28071e-09   | 589           | 24.9576%           |
| 9             | 1.9652e-05    | 460             | 6.87609e-09   | 607           | 24.2175%           |
| 10            | 1.84467e-05   | 444             | 7.4093e-10    | 588           | 24.4898%           |
| 11            | 1.82827e-05   | 506             | 6.00017e-10   | 669           | 24.3647%           |
| 12            | 1.80075e-05   | 433             | 1.11101e-10   | 572           | 24.3007%           |
| 13            | 1.79965e-05   | 487             | 1.01831e-10   | 643           | 24.2613%           |
| 14            | 1.79698e-05   | 494             | 2.53293e-11   | 657           | 24.8097%           |
| 15            | 1.79675e-05   | 460             | 1.13981e-12   | 609           | 24.4663%           |
| 16            | 1.79675e-05   | 386             | 1.02339e-12   | 520           | 25.7692%           |
| 17            | 1.79673e-05   | 435             | 3.19744e-14   | 580           | 25.0%              |
| 18            | 1.79673e-05   | 396             | 3.19744e-14   | 532           | 25.5639%           |
| 19            | 1.79673e-05   | 402             | 1.42109e-14   | 539           | 25.4174%           |
| 20            | 1.79673e-05   | 496             | 0             | 651           | 23.8095%           |
| 21            | 1.79673e-05   | 477             | 0             | 627           | 23.9234%           |
| 22            | 1.79673e-05   | 431             | 0             | 572           | 24.6503%           |
| 23            | 1.79673e-05   | 415             | 0             | 555           | 25.2252%           |

Here's what I found:

1. **1D to 1D Cosine Similarity**: 
   - Average time reduction: 24.55%
   - Average L-Mul MSE: 1.9184e-05

2. **1D to 2D Cosine Similarity**:
   - Time reductions ranging from 16.8% to 18.6%

3. **2D to 2D Cosine Similarity**:
   - Time reductions between 21.88% and 23.12%

## Implications and Potential

These results open up some interesting possibilities:

1. **Performance Boost**: The consistent speed improvements could translate to significant efficiency gains in large-scale AI operations.
2. **Accuracy-Efficiency Trade-off**: While there's a slight loss in accuracy, it's within acceptable limits for many AI applications.
3. **Energy Efficiency Potential**: The reduced computation time hints at possible energy savings, though this would need further investigation on actual hardware.

## Limitations and Considerations

It's important to acknowledge the limitations of this experiment:

1. This is a simplified implementation and may not capture all nuances of the original proposal.
2. The benchmarks focus on raw computation time, which doesn't directly translate to energy efficiency in real-world scenarios.
3. Results may vary significantly in different hardware environments or more complex applications.

## Future Directions

This experiment opens up several exciting avenues for further exploration:

1. **Hardware-Specific Implementation**: Testing on specialized hardware could provide more realistic efficiency metrics and potentially uncover even greater performance gains.
2. **Detailed Energy Consumption Analysis**: Conducting studies with actual energy consumption measurements to quantify the potential energy savings in real-world scenarios.
3. **Optimization for Different Precision Levels**: Investigating how the L-Mul algorithm performs under different precision requirements and optimizing it for specific use cases.
4. **Integration with Existing AI Frameworks**: Exploring ways to integrate this approach into popular AI frameworks to make it more accessible to researchers and practitioners.

## Acknowledgments

All credit for the L-Mul algorithm and its theoretical foundations goes to Hongyin Luo and Wei Sun, as presented in their 2024 paper. This benchmark is an independent exploration of their concepts and should not be considered an official implementation or extension of their work.

## Citation

```
@misc{luo2024additionneedenergyefficientlanguage,
    title={Addition is All You Need for Energy-efficient Language Models}, 
    author={Hongyin Luo and Wei Sun},
    year={2024},
    eprint={2410.00907},
    archivePrefix={arXiv},
    primaryClass={cs.CL},
    url={https://arxiv.org/abs/2410.00907}, 
}
```

## Disclaimer

This project is for educational purposes only. It is not intended for production use and does not claim to accurately represent the full potential or limitations of the L-Mul algorithm as proposed by Luo and Sun. You are encouraged to refer to the original paper for authoritative information.

## Wrapping Up: A Step Towards Efficient AI

[LinearCosine](https://amanpriyanshu.github.io/LinearCosine/) represents a small but interesting step in the quest for more efficient AI computations. While it may not revolutionize the field overnight, it demonstrates the potential benefits of rethinking fundamental operations in AI models.

Is LinearCosine going to revolutionize the AI world? Probably not. But it's a fun exploration into the kinds of optimizations that could shape the future of energy-efficient AI. And if nothing else, it's a great story for your next hackathon.

---

*P.S. If you've explored similar optimization techniques or have insights on energy-efficient AI computations, I'd love to hear about it! Or if you have ideas on how to extend this experiment, feel free to reach out. You can contact me at amanpriyanshusms2001[at]gmail[dot]com. Let's continue pushing the boundaries of AI efficiency together!* 🚀🧠💡