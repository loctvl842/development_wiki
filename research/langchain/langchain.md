# Langchain

## Interface

### Input Schema

Best way to get `variables` of a prompt:

```python
general_chain = (
    PromptTemplate.from_template(template=GENERAL_CHAT_TEMPLATE)
    | ChatOpenAI(model="gpt-3.5-turbo", temperature=0)
    | StrOutputParser()
)

def _get_variables(self):
    """
    Retrieve the list of variables from the input schema of the general chain.
    """
    schema = self.general_chain.input_schema.schema()
    properties = schema["properties"]
    return list(properties.keys())
```

## How to

### RunnablePassthrough

`RunnablePassthrough` allows the passing of inputs either unchanged or with the addition of extra keys.

Best usage for making syntax simple:

```python
chain = (
    | "request": RunnablePassthrough()
    | PromptTemplate.from_template(template=GENERAL_CHAT_TEMPLATE)
    | ChatOpenAI(model="gpt-3.5-turbo", temperature=0)
    | StrOutputParser()
)
# Instead of:
chain.invoke({"request": "Hi"})
# we only pass:
chain.invoke("Hi")

```

## Cookbook

### Prompt & LLM

The simplest way to create a chain is combining a `prompt` and `model`

```python
prompt = PromptTemplate.from_template(template=GENERAL_CHAT_TEMPLATE)
model = ChatOpenAI(model="gpt-3.5-turbo", temperature=0)
general_chain = (
    prompt
    | model
    | StrOutputParser()
)
```

**NOTE:** `StrOutputParser` is used to parse the output of the LLM to string.

#### Attaching Function Call information

This maybe better way to extract information from the LLM

1. Prepare the functions

```python
functions = [
    {
        "name": "joke",
        "description": "A joke",
        "parameters": {
            "type": "object",
            "properties": {
                "setup": {"type": "string", "description": "The setup for the joke"},
                "punchline": {
                    "type": "string",
                    "description": "The punchline for the joke",
                },
            },
            "required": ["setup", "punchline"],
        },
    }
]
chain = prompt | model.bind(function_call={"name": "joke"}, functions=functions)
```

2. Invoke the chain

```python
chain.invoke({"foo": "bears"}, config={})
```

Here is the output:

```sh
AIMessage(content='', additional_kwargs={'function_call': {'name': 'joke', 'arguments': '{\n  "setup": "Why don\'t bears wear shoes?",\n  "punchline": "Because they have bear feet!"\n}'}}, example=False)
```

3. Parse the information

- Use `JsonOutputFunctionsParser`

```python
from langchain.output_parsers.openai_functions import JsonOutputFunctionsParser

chain = (
    prompt
    | model.bind(function_call={"name": "joke"}, functions=functions)
    | JsonOutputFunctionsParser()
)
```

When invoke:

```python
chain.invoke({"foo": "bears"})
```

The result will be a JSON object:

```sh
{'setup': "Why don't bears like fast food?",
 'punchline': "Because they can't catch it!"}
```

Parse the result by key:

```python
from langchain.output_parsers.openai_functions import JsonKeyOutputFunctionsParser

chain = (
    prompt
    | model.bind(function_call={"name": "joke"}, functions=functions)
    | JsonKeyOutputFunctionsParser(key_name="setup")
)
```

```python
chain.invoke({"foo": "bears"})
```

```sh
"Why don't bears wear shoes?"
```

### Memory

```python
from langchain import OpenAI
from langchain.chains import ConversationChain

# first initialize the large language model
llm = OpenAI(
	temperature=0,
	openai_api_key="OPENAI_API_KEY",
	model_name="text-davinci-003"
)

# now initialize the conversation chain
conversation = ConversationChain(llm=llm)
```

### Agents

[Custom Agents](https://blog.langchain.dev/custom-agents/)

#### Cookbook

##### Create `Agent`:

Use `LLMSingleActionAgent`, This is an implementation of `BaseSingleActionAgent`, which serves as the most fundamental abstraction but is also highghly customizable.

1. Create a `chain`:

- Prepare `LLM`:

```python
llm = ChatOpenAI(model="gpt-3.5-turbo", temperature=0.0)
```

- Prepare `prompt`:

```python
DEFAULT_PROMPT_PREFIX = """
You are an intelligent AI assistant that pass TURING test.
You respond to the user as a friendly manner.
If you don't know the answer, just tell the truth.
"""

DEFAULT_PROMPT_SUFFIX = """Begin!

Previous conversation history:
{chat_history}

Human: {input}
AI:
"""

prompt = PromptTemplate(
    template="\n\n".join(
        [DEFAULT_PROMPT_PREFIX, DEFAULT_PROMPT_SUFFIX]
    ),
    input_variables=["input", "chat_history"],
)
```

2. Create an `output_parser`:

```python
class CustomOutputParser(AgentOutputParser):
    def parse(self, text: str) -> Union[AgentAction, AgentFinish]:
        return AgentFinish(return_values={"output": text}, log="")
```

3. Create `agent`:

```python
agent = LLMSingleActionAgent(
    llm_chain=chain,
    output_parser=output_parser,
    stop=["\nObservation:"],
    allowed_tools=["get_today_date"],
    verbose=True,
)
```

Agent's mission is to take the input and decide which is the next action (using tool `get_today_date` or not?).

4. Create `agent_executor`:

- Create tool `get_today_date`:

```python
class GetTodayDate(BaseTool):
    def _run(self, query, run_manager: Optional[AsyncCallbackManagerForToolRun] = None):
        """Use the tool"""
        return datetime.today()

    async def _arun(
        self, query: str, run_manager: Optional[AsyncCallbackManagerForToolRun] = None
    ):
        """Use the tool asynchronously"""
        raise NotImplementedError("custom_search does not support async")


tools = [GetTodayDate(name="get_today_date", description="Get today's date")]
```

- Now, we already prepared `agent` and `tools`, `agent` decide which tool to use. After choosing the tool, `agent_executor` execute that tool. If you look carefully at the definition of `agent`, I only pass the name of the tools. `agent_executor`'s mission is to provide logic of those tools.

```python
agent_executor = AgentExecutor.from_agent_and_tools(
    agent=agent,
    tools=tools,
    return_intermediate_steps=True,
    verbose=True,
)
```

5. Test it out:

Up to now, we should get this error:

```sh
Agent stopped due to iteration limit or time limit.
```

6. Troubleshoot

The tool `get_today_date` was actually executed. But it wasn't saved. While `LLM` respond based on the prompt. And the prompt stay the same after `AgentAction` executed.

**Therefore, We need a more dynamic prompt.**

- Idea is the same as how `shorterm memory` works.

- Let's add `{agent_scratchpad}` to save the thought of `agent_executor` after parsing the input.

Edit `DEFAULT_PROMPT_SUFFIX`:

```python
DEFAULT_PROMPT_SUFFIX = """Begin!

Previous conversation history:
{chat_history}

Human: {input}
AI:

{agent_scratchpad}
"""
```

Let's custom new PromptTemplate, we don't want to use `PromptTemplate` because it's not flexible enough. Because all variables defined in the template must be provided, while some variables are not always available. For example, `agent_scratchpad` is only available after `agent_executor` parse the input.

```python
class CustomPromptTemplate(StringPromptTemplate):
    template: str
    input_variables: list[str]
    tools: List[BaseTool]

    def format(self, **kwargs) -> str:
        intermediate_steps = kwargs.pop("intermediate_steps")
        thoughts = ""
        print(intermediate_steps)
        for action, observation in intermediate_steps:
            thoughts += action.log
            thoughts += f"\nObservation: {observation}\n"
        kwargs["agent_scratchpad"] = thoughts
        kwargs["tools"] = "\n".join(
            [f"{tool.name}: {tool.description}" for tool in self.tools]
        )
        kwargs["tool_names"] = ", ".join([tool.name for tool in self.tools])
        return self.template.format(**kwargs)
```

Let's create another `prompt`:

```python
FORMAT_INSTRUCTIONS = """To use a tool, please use the following format:

\\```
Thought: Do I need to use a tool? Yes
Action: the action to take, should be one of [`get_today_date`]
Action Input: the input to the action
Observation: the result of the action
\\```

When you have a response to say to the Human, or if you do not need to use a tool, you MUST use the format:

\\```
Thought: Do I need to use a tool? No
AI: [your response here]
\\```"""

prompt = CustomPromptTemplate(
    template="\n\n".join(
        [DEFAULT_PROMPT_PREFIX, FORMAT_INSTRUCTIONS, DEFAULT_PROMPT_SUFFIX]
    ),
    input_variables=["input", "chat_history", "intermediate_steps"],
    tools=tools,
)
```

Now, we done. Let's test again:

```python
agent_executor("What is the date today?")
```


# API

- [format_document](https://api.python.langchain.com/en/latest/prompts/langchain_core.prompts.base.format_document.html)
- [get_buffer_string](https://api.python.langchain.com/en/v0.0.339/schema/langchain.schema.messages.get_buffer_string.html)
