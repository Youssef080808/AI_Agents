# AI Agents

My notebooks from working through the [Hugging Face AI Agents Course](https://huggingface.co/learn/agents-course), built with [smolagents](https://github.com/huggingface/smolagents) and run in Google Colab.

Both notebooks run a small open model (`Qwen/Qwen2.5-Coder-3B-Instruct`) locally on Colab's free T4 GPU through `TransformersModel`, so they don't need paid inference credits.

## Notebooks

### `smolAgent.ipynb` — First agent with custom tools and a chat UI

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/Youssef080808/AI_Agents/blob/main/smolAgent.ipynb)

Builds on the course's `First_agent_template`. It defines a few custom tools, gives them to a `CodeAgent`, and launches a Gradio chat interface so you can talk to the agent in the browser.

Tools:

| Tool | What it does |
|---|---|
| `get_current_time_in_timezone` | Returns the current time in a given time zone (e.g. `America/New_York`) |
| `get_weather` | Gets the current weather for a location from [wttr.in](https://wttr.in) |
| `get_distance_from_the_pyramids` | Dummy tool that always returns a fixed distance, used to practice writing tools |
| `image_generation_tool` | Text-to-image tool loaded from the Hub (`agents-course/text-to-image`) |
| `final_answer` | Built-in tool the agent uses to return its answer |

### `codeAgent.ipynb` — Alfred's party planning agents

Follows the course's "Alfred the butler" examples, with one section per agent:

- **Song agent**: searches the web (DuckDuckGo) and visits a page to build a party playlist.
- **Menu agent**: uses a custom `suggest_menu` tool to suggest a menu for an occasion.
- **Ready time agent**: adds up task durations and works out when the party will be ready, using the current time in Toronto (`datetime` + `zoneinfo`).
- **Full agent**: combines web search, page visits, and three custom tools (`suggest_menu`, `catering_service_tool`, `SuperheroPartyThemeTool`) in one agent.
- **Observability**: traces agent runs to [Langfuse](https://langfuse.com) with OpenTelemetry (`openinference-instrumentation-smolagents`).
- **Hub publishing**: notes on pushing an agent to the Hugging Face Hub and loading it back.

## Running the notebooks

1. Open a notebook in Colab and switch to a GPU: **Runtime → Change runtime type → T4 GPU**.
2. Run the install cell. For the web-page tool in `codeAgent.ipynb`, also install:
   ```python
   !pip install markdownify
   ```
3. Log in to Hugging Face when prompted (or add `HF_TOKEN` under Colab **Secrets**).
4. For the Langfuse section, add these Colab secrets and turn on notebook access for them:
   - `LANGFUSE_PUBLIC_KEY`
   - `LANGFUSE_SECRET_KEY`

   The notebook uses the EU host (`https://cloud.langfuse.com`). Change `LANGFUSE_BASE_URL` if your Langfuse project is in another region.

The first run downloads the model (about 6 GB), which takes a minute or two. Load the model once per session and reuse it across agents.

## Notes

- **The model is small.** A 3B model sometimes invents tools, misreads tool output, or forgets to call `final_answer`. The prompts in these notebooks are more explicit than the course's for that reason. A larger model (e.g. a 4-bit 7B model, or one served through an inference provider) gives much more reliable results.
- **GPU memory is limited.** Agent history grows with every step, so the agents use `max_steps`, `max_results`, and `max_output_length` to stay within the T4's 15 GB. If you hit `CUDA out of memory`, restart the session (**Runtime → Restart session**) and run the model cell once.
- **No secrets in the code.** Keys are read from Colab Secrets with `userdata.get()` and are never stored in the notebooks.
