Skip to main content
**Join us at Interrupt: The Agent AI Conference by LangChain on May 13 & 14 in San Francisco!**
On this page
![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)![Open on GitHub](https://img.shields.io/badge/Open%20on%20GitHub-grey?logo=github&logoColor=white)
Prerequisites
This guide assumes familiarity with the following concepts:
  * Chat models
  * Output parsers
  * Prompt templates
  * Structured output
  * Chaining runnables together


LLMs from different providers often have different strengths depending on the specific data they are trained on. This also means that some may be "better" and more reliable at generating output in formats other than JSON.
This output parser allows users to specify an arbitrary schema and query LLMs for outputs that conform to that schema, using YAML to format their response.
note
Keep in mind that large language models are leaky abstractions! You'll have to use an LLM with sufficient capacity to generate well-formed YAML.
```
%pip install -qU langchain langchain-openaiimport osfrom getpass import getpassif"OPENAI_API_KEY"notin os.environ:  os.environ["OPENAI_API_KEY"]= getpass()
```

We use Pydantic with the `YamlOutputParser` to declare our data model and give the model more context as to what type of YAML it should generate:
```
from langchain.output_parsers import YamlOutputParserfrom langchain_core.prompts import PromptTemplatefrom langchain_openai import ChatOpenAIfrom pydantic import BaseModel, Field# Define your desired data structure.classJoke(BaseModel):  setup:str= Field(description="question to set up a joke")  punchline:str= Field(description="answer to resolve the joke")model = ChatOpenAI(temperature=0)# And a query intented to prompt a language model to populate the data structure.joke_query ="Tell me a joke."# Set up a parser + inject instructions into the prompt template.parser = YamlOutputParser(pydantic_object=Joke)prompt = PromptTemplate(  template="Answer the user query.\n{format_instructions}\n{query}\n",  input_variables=["query"],  partial_variables={"format_instructions": parser.get_format_instructions()},)chain = prompt | model | parserchain.invoke({"query": joke_query})
```

**API Reference:**YamlOutputParser | PromptTemplate | ChatOpenAI
```
Joke(setup="Why couldn't the bicycle find its way home?", punchline='Because it lost its bearings!')
```

The parser will automatically parse the output YAML and create a Pydantic model with the data. We can see the parser's `format_instructions`, which get added to the prompt:
```
parser.get_format_instructions()
```

```
'The output should be formatted as a YAML instance that conforms to the given JSON schema below.\n\n# Examples\n## Schema\n\`\`\`\n{"title": "Players", "description": "A list of players", "type": "array", "items": {"$ref": "#/definitions/Player"}, "definitions": {"Player": {"title": "Player", "type": "object", "properties": {"name": {"title": "Name", "description": "Player name", "type": "string"}, "avg": {"title": "Avg", "description": "Batting average", "type": "number"}}, "required": ["name", "avg"]}}}\n\`\`\`\n## Well formatted instance\n\`\`\`\n- name: John Doe\n avg: 0.3\n- name: Jane Maxfield\n avg: 1.4\n\`\`\`\n\n## Schema\n\`\`\`\n{"properties": {"habit": { "description": "A common daily habit", "type": "string" }, "sustainable_alternative": { "description": "An environmentally friendly alternative to the habit", "type": "string"}}, "required": ["habit", "sustainable_alternative"]}\n\`\`\`\n## Well formatted instance\n\`\`\`\nhabit: Using disposable water bottles for daily hydration.\nsustainable_alternative: Switch to a reusable water bottle to reduce plastic waste and decrease your environmental footprint.\n\`\`\` \n\nPlease follow the standard YAML formatting conventions with an indent of 2 spaces and make sure that the data types adhere strictly to the following JSON schema: \n\`\`\`\n{"properties": {"setup": {"title": "Setup", "description": "question to set up a joke", "type": "string"}, "punchline": {"title": "Punchline", "description": "answer to resolve the joke", "type": "string"}}, "required": ["setup", "punchline"]}\n\`\`\`\n\nMake sure to always enclose the YAML output in triple backticks (\`\`\`). Please do not add anything other than valid YAML output!'
```

You can and should experiment with adding your own formatting hints in the other parts of your prompt to either augment or replace the default instructions.
## Next steps​
You've now learned how to prompt a model to return YAML. Next, check out the broader guide on obtaining structured output for other related techniques.
#### Was this page helpful?
  * Next steps


