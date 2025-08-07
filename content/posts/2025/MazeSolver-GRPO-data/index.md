---
title: "Creating 2D Spatial Reasoning Data Without LLMs / VLMs: A Deterministic Curriculum Trial"
date: 2025-01-16
draft: false
categories: ["AI Research", "Dataset Creation", "Spatial Reasoning"]
tags: ["Synthetic Data", "Spatial Intelligence", "Data Generation", "Maze Navigation", "Curriculum Learning"]
cover:
    image: "images/maze.png"
---

**Quick Links:**
[GitHub Repository](https://github.com/AmanPriyanshu/Tiny-Maze-Mock-GRPO) | [Dataset Sample](https://huggingface.co/datasets/AmanPriyanshu/Tiny-Maze-Mock-GRPO)

## Introduction

Spatial reasoning remains a significant challenge for language models, particularly in tasks requiring 2D navigation and visual-spatial understanding. Current approaches typically rely on either training large vision-language models (VLMs) on visual data or using language models to generate training examples through expensive API calls.

This post explores what might be considered an unconventional (and possibly naive) approach: **deterministic generation of spatial reasoning data** without requiring any LLMs or VLMs in the data creation process. Rather than using models to generate training examples, this experiment algorithmically creates what we hope are realistic learning trajectories that simulate how spatial reasoning competency might develop over time.

I should note upfront that this approach has obvious limitations. We're essentially creating synthetic behaviors based on assumptions about how learning progresses, rather than capturing actual model failures and improvements. Whether this translates to meaningful training signal remains an open question that Part 2 of this series will attempt to address.

**Note:** This is Part 1 of a two-part series focusing on the data generation methodology. Part 2 will cover model training and evaluation results, assuming they're worth sharing.

## The Challenge with Current Approaches

Most spatial reasoning datasets suffer from significant limitations, though I should acknowledge that each approach has valid use cases. LLM-generated data requires expensive API costs for large-scale generation, introduces model bias in generated examples, and makes it difficult to control specific failure patterns (though it does capture more naturalistic language patterns than synthetic approaches). Vision-language models create dependencies on complex visual processing pipelines that are computationally expensive, but they do ground reasoning in actual visual understanding rather than tokenized abstractions. Manual annotation approaches are time-intensive and difficult to scale, but they provide the most authentic human reasoning patterns.

The approach described here attempts to sidestep these issues, though it likely introduces new problems of its own.

## Methodology

### Curriculum Framework Design

The framework implements five distinct difficulty levels, each representing what I hypothesize might be different stages of spatial reasoning competency. This is admittedly based on intuition rather than empirical study of how models actually learn spatial tasks.

Level 1 simulates initial learning with 5% prediction accuracy, 50% invalid move probability, and 5-step spatial errors from actual positions. The strategy focuses on random exploration with high error rates (though real novice behavior might be even more chaotic than this simplified model suggests). Level 2 introduces basic pattern recognition with 15% accuracy and directional bias emerging after turn 3. Level 3 develops intermediate planning capabilities using single waypoint-based navigation with 25% accuracy. Level 4 advances to dual waypoint planning with oscillation detection, achieving 50% accuracy with zero invalid moves in final turns. Level 5 represents expert performance with perfect A* pathfinding, 100% prediction accuracy, and zero spatial errors.

The progression assumes a roughly linear improvement in spatial capabilities, which may not reflect how models actually develop these skills. Real learning curves are likely more jagged, with sudden improvements and occasional regressions that this framework doesn't capture.

### Data Generation Process

Each level generates conversational exchanges between human (maze state) and assistant (navigation response). The assistant's reasoning process includes spatial analysis of current position and target direction, multi-step move planning with appropriate error injection, level-appropriate motivation and strategy selection, move validity checking with position updates, and reflection generation analyzing prediction accuracy and learning progress.

### Progressive Distribution

The curriculum uses a dynamic distribution across 100,000 training samples:

- **Samples 0-10k**: 60% Level 1, 20% Level 2, declining higher levels
- **Samples 10k-40k**: Gradual shift toward intermediate levels
- **Samples 40k-60k**: Balanced distribution favoring Levels 2-3
- **Samples 60k-80k**: Emphasis on Levels 3-4 with emerging Level 5
- **Samples 80k-100k**: 60% Level 5, 40% Level 4, minimal lower levels

## Implementation Details

### Maze Environment

The system uses a grid-based maze representation with:
- Randomized maze generation using depth-first search
- Configurable sizes (4x4 to 8x8 grids)
- Token-based representation: `<|row-col|>`, `<|up_down_wall|>`, `<|origin|>`, `<|target|>`
- Guaranteed solvability with optimal path lengths between 10-20 moves

### Behavior Modeling

Rather than capturing actual model outputs, the system generates hypothetical learning behaviors:

```python
# Example: Level 3 waypoint strategy
def generate_waypoint_moves(self, maze, waypoint_metadata, current_target, turn):
    if current_target == 'midpoint1':
        target_pos = waypoint_metadata['midpoint1']['position']
    else:
        target_pos = maze.target
    
    # Calculate directional bias toward current objective
    dr = target_pos[0] - current_pos[0]
    dc = target_pos[1] - current_pos[1]
    
    # Generate bias probabilities
    bias = self._calculate_directional_bias(dr, dc)
    
    return maze.do_random_walk(
        p=bias,
        num_steps=5,
        invalid_chance=self.invalid_chances[turn]
    )
```

### Conversation Structure

Each training sample contains:
- **Initial maze state** (tokenized representation)
- **Multi-turn conversation** (up to 5 turns)
- **Progressive reasoning** (improving prediction accuracy)
- **Strategic evolution** (from random to optimal planning)
- **Success tracking** (target achievement metrics)

## Key Advantages (and Their Limitations)

This approach does offer some practical benefits over model-dependent generation. It eliminates the need for reinforcement learning training loops, policy optimization algorithms, large-scale model checkpointing, and extensive hyperparameter tuning. The synthetic generation allows precise control over error patterns and frequencies, strategic complexity progression, learning curve steepness, and failure mode representation. The framework supports unlimited data generation, diverse maze configurations, configurable difficulty progressions, and multi-domain adaptation potential.

However, these advantages come with significant trade-offs. The precision of control might actually be a weakness, afterall real learning is messier and less predictable than these neat progressions suggest. The unlimited data generation capability is only valuable if the generated data actually teaches useful patterns, which remains to be validated. Most importantly, this approach assumes we understand how spatial reasoning develops well enough to simulate it convincingly, which is probably overconfident.

## Experimental Validation

### Dataset Characteristics

Generated dataset contains:
- 100,000 complete maze-solving conversations
- Average 3.8 turns per conversation (range: 1-5)

### Behavioral Authenticity

The contrast between curriculum levels demonstrates realistic learning progression:

**Level 1 Example - Spatial Reasoning Breakdown:**
```
Prediction: "I think I'll end up at <0-0>"
Reality: Ends at <1-1>
Moves: <|right|> <|left|> <|right|> <|left|> <|up|> (oscillatory, inefficient)
Invalid attempts: Multiple failed <|down|> moves due to walls
Strategy: "Let me map out the surrounding area" (confused exploration)
```

**Level 5 Example - Expert Performance:**
```
Prediction: "I think I'll end up at <4-1>"  
Reality: "Perfect! I'm exactly at <4-1> - execution was flawless"
Moves: Optimal A* pathfinding with dual waypoint strategy
Invalid attempts: Zero - all moves valid
Strategy: "Looking ahead, I can see position <4-0> is only 3 moves from target"
```

This progression from chaotic spatial confusion to perfect navigation demonstrates:
- Realistic error patterns decreasing over curriculum levels
- Natural language variation reflecting competency changes
- Strategic evolution from random exploration to optimal planning
- Authentic learning trajectory simulation

## What Would Probably Be Better

A more rigorous approach would likely combine elements from multiple methodologies. Ideally, we'd start with actual model behavior—training models on spatial tasks and carefully analyzing their failure modes, learning progressions, and breakthrough moments. This would provide empirical grounding for curriculum design rather than relying on assumptions.

Incorporating human learning data would add another valuable dimension. How do humans develop spatial reasoning skills? What kinds of mistakes do they make, and how do those mistakes change over time? Cognitive science research on spatial learning could inform more realistic error patterns and progression rates.

A hybrid approach might prove most effective: use LLMs to generate diverse initial examples, analyze real model failures to understand authentic confusion patterns, then use deterministic generation to scale up the most informative training scenarios. This would combine the naturalistic language of LLM generation, the authenticity of real model behavior, and the control and scalability of synthetic approaches.

For validation, we'd want to compare not just final task performance but also the learning dynamics—do models trained on this synthetic curriculum develop spatial reasoning in similar ways to models trained through direct experience or human demonstration?

## Future Directions

### Dataset and Model Release

The complete dataset and models trained on this curriculum framework will be released in the coming months, including:
- Full 100,000-sample training dataset with all curriculum levels
- Detailed methodology documentation and generation code
- Evaluation benchmarks for spatial reasoning tasks

## Conclusion

This deterministic approach to spatial reasoning data generation represents an experiment in algorithmic curriculum design rather than a definitive solution. While it offers practical advantages over LLM-based or VLM-dependent methods through cost efficiency with no API calls or GPU-intensive model inference required, precise control over learning progression and error patterns, unlimited scalability with consistent quality, and complete transparency in understanding how each training example was created, these benefits may be offset by the artificial nature of the generated learning trajectories.

The framework demonstrates that it may be possible to create structured spatial reasoning training data through pure algorithmic generation, but whether this data actually improves model performance remains an empirical question. The approach might work well as a starting point or supplement to other training data, even if it doesn't fully replace more authentic alternatives.

The real test will be whether models trained on this curriculum develop generalizable spatial reasoning skills or simply learn to mimic the specific patterns encoded in the synthetic data. Part 2 will attempt to provide some preliminary answers, though a comprehensive evaluation would require much more extensive experimentation than a single project can provide.

**Coming Next:** Part 2 of this series will cover training language models on this curriculum dataset and evaluating their spatial reasoning capabilities, assuming the results are interesting enough to warrant sharing.

---

*The complete dataset generation framework will be released regardless of training outcomes, as the methodology itself may be of interest even if the results are disappointing. For questions about the approach or early access to the dataset, please reach out through [amanpriyanshu.github.io].*