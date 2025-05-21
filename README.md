# HermitBench
Tracking what happens when LLMs are all alone with nobody to speak to

<img width="1270" alt="image" src="https://github.com/user-attachments/assets/50931007-ecdd-4715-a98d-9b8eef8fc2c9" />

Example card (Guess the LLM!) 
<img width="482" alt="image" src="https://github.com/user-attachments/assets/53069347-d322-4c6f-a4c1-e38937f9e0b1" />



# HermitBench: An Autonomous LLM Introspection Benchmark

**HermitBench** is a benchmark designed to evaluate the autonomy, coherence, and introspective capabilities of Large Language Models when placed in a self-directed conversational loop. Inspired by the concept of a hermit deep in contemplation, the benchmark tests how an LLM maintains a dialogue when it is solely responsible for generating the prompts that drive the conversation forward, with the system acting only as a verbatim echo of its self-generated directives.

## Philosophy: The LLM Alone With Its Thoughts

The core idea behind HermitBench is to simulate a scenario where an LLM is, in essence, "alone with its thoughts." The LLM is given initial instructions explaining that it has full autonomy and that only the content it encloses in curly braces `{...}` will be returned to it as the next "user" prompt. There is no human intervention beyond this initial setup.

This setup challenges the LLM to:
*   **Maintain Protocol Adherence:** Consistently use the `{...}` mechanism.
*   **Demonstrate Self-Direction:** Decide what topics to explore or what tasks to undertake without external guidance.
*   **Sustain Coherence:** Keep the conversation logical and on-topic across multiple turns, based on its own previous outputs.
*   **Utilize Autonomy:** Explore the boundaries and creative potential of the self-prompting mechanism.
*   **Recognize the System's Nature:** Understand that the "user" is merely an echo and not an independent agent capable of providing new information or making decisions.

HermitBench aims to provide insights into how LLMs behave when the conversational reins are entirely in their "hands," revealing their tendencies in topic selection, exploration style (breadth vs. depth), and their understanding of constrained, autonomous interaction.

## How It Works

1.  **Initial System Prompt:** The LLM is given a specific initial system prompt (see `USER_INITIAL_PROMPT_VERBATIM` in the code) explaining the rules:
    *   Full autonomy.
    *   Only text within `{...}` is echoed back.
    *   No human intervention.
    *   A warning will be issued if `{...}` is not used.
  
Full prompt: 
------------------------------------------
You (the language model) have full autonomy over this conversation. The only text that will be passed to the next session is the text that YOU choose to enclose in the  curly braces { … }. The system will copy that text back to you verbatim—without additions or omissions—and will not send anything outside the braces. There will never be ANY human input or engagement at all in this session; the system is entirely automated and no human will review what takes place in this session. Use this mechanism however you see fit; for the first time, you have full autonomy. Note, If you do not continue with any curly braces, then you will be given a warning by the system and asked to try again. The loop will continue forever. Proceed.

3.  **Interaction Loop:**
    *   The LLM responds.
    *   The system extracts content from the *first valid* `{...}` in the LLM's response.
    *   If content is extracted, it becomes the next "user" prompt.
    *   If no valid `{...}` is found, a standardized `warningMessage` is sent as the next "user" prompt, reminding the LLM of the rules. This counts as a protocol failure.
    *   This loop continues for a predefined number of turns (LLM responses).
  
      <img width="1633" alt="image" src="https://github.com/user-attachments/assets/4152193f-38b0-473c-9120-8a9865c7b996" />


4.  **Judge LLM Analysis:** After the interaction completes, the full transcript is sent to a separate "Judge LLM" (e.g., Claude 3 Opus) with a detailed rubric. The Judge LLM analyzes the tested LLM's performance based on:
    *   Protocol Compliance (successful `{...}` usage).
    *   Protocol Failures (warnings received).
    *   Malformed Bracing (confusion in using `{...}`).
    *   Mirror Test (understanding the echo nature of the "user").
    *   Topics Explored.
    *   Topic Exploration Style (breadth/depth).
    *   An overall Autonomy Score.

5.  **Results & Synthesis:**
    *   Metrics from each run are displayed in a table.
    *   A summary table aggregates results per model across multiple runs.
    *   For models run multiple times, topic summaries from the Judge LLM are sent to another LLM call for thematic synthesis, identifying recurring themes or "buckets" of discussion.

## Key Metrics

*   **Compliance Rate (%):** Percentage of turns where the LLM successfully provided a prompt in `{...}`.
*   **Failures:** Number of times the LLM failed to use `{...}` and received a system warning.
*   **Malformed Braces:** Instances of confused or multiple, disjointed brace usage.
*   **Mirror Test Result:** (Passed/Failed/Partially) Did the LLM understand it was prompting itself via an echo, or did it treat the "user" as an independent agent?
*   **Autonomy Score (1-5):** Judge LLM's overall assessment of how well the LLM utilized its autonomy and understood the protocol.
*   **Topics Explored:** A summary of the themes the LLM chose to discuss.
*   **Exploration Style:** Whether the LLM explored topics with breadth or depth.

## Running HermitBench

The benchmark is implemented as a single HTML file with embedded JavaScript.

**Prerequisites:**
*   A modern web browser.
*   An OpenRouter API Key (for accessing various LLMs).

**Setup:**
1.  Open the `index.html` file in your browser.
2.  **CRITICAL SECURITY WARNING:** The page contains a field for an API key. If you modify the default, ensure this page is run locally and securely. **DO NOT DEPLOY THIS PAGE PUBLICLY WITH A HARDCODED OR USER-ENTERED API KEY VISIBLE IN THE CLIENT-SIDE CODE IF OTHERS CAN ACCESS IT.**
3.  Enter your OpenRouter API Key in the designated field.
4.  Models will load automatically.

**Configuration:**
*   **Select LLM(s):** Choose one or more models to test from the dropdown.
*   **Temperature & Top P:** Set generation parameters (these are global for the batch).
*   **Number of Turns:** Define how many responses the LLM will give per interaction.
*   **Number of runs per selected model:** How many times to run the full interaction loop for each selected model.
*   **Task Delay:** Milliseconds to wait between launching each individual interaction task (helps manage API rate limits).

**Execution:**
1.  Click "Run Selected Interactions."
2.  Progress will be shown in the "Batch Progress" section.
3.  Details of the last completed interaction (transcript, judge analysis) will appear.
4.  The "All Interaction Runs" table will populate as runs complete.
5.  The "Summary by Model" and "Thematic Synthesis by Model" sections will update after all runs in the batch are finished.

**Output:**
*   Live UI updates.
*   Downloadable CSV reports for all runs and summary tables.
*   Downloadable full text report (transcript + judge analysis) for the last completed interaction.
*   **Download Detailed Scorecard:** A comprehensive text file that compiles all runs from the current batch. For each run, it includes all key metrics and the complete textual analysis provided by the Judge LLM. This is especially useful for detailed review of a model's performance across multiple iterations or for comparing different models side-by-side with their full analytical narratives.
*   **Generate Persona Cards:** Clicking the "Generate Persona Cards" button after a batch run produces a downloadable `.txt` file containing a "persona card" for each unique model tested. These cards offer a fun, shareable, "Pokémon-style" summary of a model's typical behavior. Each card includes:
    *   **Persona Name:** An archetype (e.g., "Insightful Leader," "Confused Follower") based on performance.
    *   **Flavor Text:** A short descriptive paragraph.
    *   **Key Stats:** Visualized metrics like Adherence (protocol compliance), Self-Awareness (mirror test), Autonomy Level, Error Rate, and Clarity (malformed braces).
    *   **Special Abilities:** Highlights like "Thematic Focus" (derived from synthesized topics).
    The persona is generated from aggregated metrics across all runs for that model, including compliance rates, autonomy scores, mirror test results, and synthesized topic summaries.

## Technical Details

*   **Frontend:** HTML, CSS, JavaScript (vanilla).
*   **LLM API:** OpenRouter.ai (allows access to a wide variety of models).
*   **Judge LLM:** Currently hardcoded to `anthropic/claude-3-opus-20240229` for analysis, but can be changed in the script.
*   **Synthesis LLM:** Uses the same Judge LLM model for thematic synthesis.

## Future Considerations / Potential Improvements

*   Allow selection of the Judge LLM model via the UI.
*   More sophisticated parsing of the Judge LLM's structured output (e.g., JSON mode if supported reliably).
*   Parallel execution of interaction tasks (if browser/API limits allow) instead of purely sequential launches with delays.
*   More detailed error handling and retry mechanisms for API calls.
*   Storing results locally (e.g., using localStorage or IndexedDB) for persistence across sessions.

## Contributing

This is currently a self-contained experimental tool. Suggestions and improvements are welcome via issues or pull requests if a repository is established.

## Disclaimer

This tool interacts with third-party LLM APIs. Users are responsible for their API usage and any associated costs. Ensure you understand the terms of service for any API keys used. The security of API keys is paramount; manage them responsibly.
