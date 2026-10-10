# smolAgent

A small Colab notebook for experimenting with custom tools in [smolagents](https://github.com/huggingface/smolagents), built on the Hugging Face Agents Course [First Agent Template](https://huggingface.co/spaces/agents-course/First_agent_template).

## Setup

Run the first cells of `smolAgent.ipynb` to:

1. Clone the `First_agent_template` Space (skipped if it's already present).
2. Install its requirements plus the latest `smolagents`.
3. Import `CodeAgent`, `DuckDuckGoSearchTool`, `FinalAnswerTool`, `InferenceClientModel`, and the `@tool` decorator.

## Tools

| Tool | What it does |
|------|--------------|
| `get_distance_from_the_pyramids(location)` | Dummy tool for testing — always returns 8700 km. |
| `get_weather(location)` | Fetches a one-line weather summary from [wttr.in](https://wttr.in). |
| `get_current_time_in_timezone(timezone)` | Returns the current local time for a timezone like `America/New_York`, using `pytz`. |

Each tool is a plain Python function wrapped with `@tool`, with type hints and a docstring describing its arguments so the agent knows how to call it.

## Status

The tools are defined, but they aren't wired into an agent yet. A typical next step:

```python
model = InferenceClientModel()
agent = CodeAgent(
    tools=[get_weather, get_current_time_in_timezone,
           get_distance_from_the_pyramids, FinalAnswerTool()],
    model=model,
)
agent.run("What's the weather and local time in Tokyo?")
```

The notebook also has a markdown note describing a `calculator` tool (multiply two integers) that hasn't been implemented.
