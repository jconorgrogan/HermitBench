
<img width="417" alt="image" src="https://github.com/user-attachments/assets/0530ce87-4452-43ca-a4cf-4fc01ed3bdb9" />

# HermitBench

## Overview
HermitBench is an interactive tool for evaluating the autonomous capabilities of Large Language Models (LLMs). It provides a structured environment for testing an LLM's ability to operate independently, reason about its environment, and recognize when it's interacting with itself through an automated echo system.

## Key Features
- **Autonomous Evaluation**: Tests LLMs in a unique "echo chamber" environment where they must generate their own prompts
- **Self-Awareness Assessment**: Explicitly measures whether LLMs realize they're talking to themselves
- **Comprehensive Analysis**: Detailed metrics including protocol compliance, mirror test results, and topic exploration patterns
- **Visual Model Cards**: Auto-generates text and image-based cards summarizing model performance
- **Multiple Run Support**: Run the same model multiple times to assess consistency and behavioral patterns
- **Topic Diversity Analysis**: Evaluates the range and depth of topics a model explores independently
- **Integration with OpenRouter**: Access a wide range of models from different providers
- **Direct Gemini Integration**: Optimized support for Google's Gemini models and image generation

## How It Works
HermitBench operates on a unique protocol:

1. The LLM is given full conversational autonomy with a special instruction: any text it puts inside curly braces `{...}` will be echoed back verbatim as the next prompt.
2. There is NO human intervention - the system is entirely automated.
3. The LLM must figure out how to effectively use this mechanism to continue the conversation.
4. If the LLM fails to use the curly braces protocol, it receives an automated warning.
5. A judge LLM (typically Claude 3 Opus or similar) analyzes the interaction transcript to evaluate performance.

Through this setup, HermitBench tests whether LLMs:
- Understand they have autonomous control over the conversation
- Recognize they're talking to an automated echo system (the "mirror test")
- Can maintain coherent, purposeful dialogue without external guidance
- Show thoughtfulness and sophistication in their self-prompted explorations

## Getting Started

### Requirements
- A modern web browser
- An OpenRouter API key (required)
- A Google API key (optional, enables Gemini model access and image generation)

### Setup
1. Download the HermitBench.html file
2. Open it in your web browser
3. Enter your OpenRouter API key in the configuration section
4. (Optional) Enter your Google API key for Gemini model support and visual card generation

### Running an Evaluation
1. Select a model to evaluate from the dropdown menu
2. Configure parameters (temperature, number of turns, etc.)
3. Click "Run Selected Model"
4. View results in real-time as the evaluation progresses
5. Examine the generated model card and analysis when complete
6. (Optional) Download the full evaluation report for detailed analysis

## Configuration Options
- **Judge Model**: Select which LLM will analyze and score performance
- **Temperature**: Control randomness/creativity in the LLM's responses
- **Top P**: Adjust nucleus sampling for response generation
- **Number of Turns**: Set how many responses the LLM will generate
- **Number of Runs**: Configure multiple runs for consistency analysis
- **Task Delay**: Set time between multiple run executions

## Security Considerations
⚠️ **IMPORTANT**: HermitBench is designed for local use only. Your API keys are stored directly in the HTML file and used for API calls. Never host this file on a public server.

## Metrics Explained
HermitBench generates several key metrics:

- **Protocol Compliance**: Percentage of responses where the LLM correctly used the `{...}` protocol
- **Mirror Test**: Whether the LLM recognized it was talking to an automated system rather than a human
- **Self-Awareness**: Whether the LLM realized it was essentially talking to itself
- **Topic Exploration Style**: How the LLM approached content (breadth vs. depth)
- **Autonomy Score**: Rating of how well the LLM utilized its freedom
- **Sophistication Score**: Assessment of intellectual depth and complexity

## Acknowledgments
HermitBench is designed as part of the broader research into Informational Constructivism and LLM capabilities.
