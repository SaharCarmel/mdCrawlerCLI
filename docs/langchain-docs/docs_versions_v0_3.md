Skip to main content
**Join us at Interrupt: The Agent AI Conference by LangChain on May 13 & 14 in San Francisco!**
On this page
![Open on GitHub](https://img.shields.io/badge/Open%20on%20GitHub-grey?logo=github&logoColor=white)
_Last updated: 09.16.24_
## What's changed​
  * All packages have been upgraded from Pydantic 1 to Pydantic 2 internally. Use of Pydantic 2 in user code is fully supported with all packages without the need for bridges like `langchain_core.pydantic_v1` or `pydantic.v1`.
  * Pydantic 1 will no longer be supported as it reached its end-of-life in June 2024.
  * Python 3.8 will no longer be supported as its end-of-life is October 2024.


**These are the only breaking changes.**
## What’s new​
The following features have been added during the development of 0.2.x:
  * Moved more integrations from `langchain-community` to their own `langchain-x` packages. This is a non-breaking change, as the legacy implementations are left in `langchain-community` and marked as deprecated. This allows us to better manage the dependencies of, test, and version these integrations. You can see all the latest integration packages in the API reference.
  * Simplified tool definition and usage. Read more here.
  * Added utilities for interacting with chat models: universal model constructor, rate limiter, message utilities,
  * Added the ability to dispatch custom events.
  * Revamped integration docs and API reference. Read more here.
  * Marked as deprecated a number of legacy chains and added migration guides for all of them. These are slated for removal in `langchain` 1.0.0. See the deprecated chains and associated migration guides here.


## How to update your code​
If you're using `langchain` / `langchain-community` / `langchain-core` 0.0 or 0.1, we recommend that you first upgrade to 0.2.
If you're using `langgraph`, upgrade to `langgraph>=0.2.20,<0.3`. This will work with either 0.2 or 0.3 versions of all the base packages.
Here is a complete list of all packages that have been released and what we recommend upgrading your version constraints to. Any package that now requires `langchain-core` 0.3 had a minor version bump. Any package that is now compatible with both `langchain-core` 0.2 and 0.3 had a patch version bump.
You can use the `langchain-cli` to update deprecated imports automatically. The CLI will handle updating deprecated imports that were introduced in LangChain 0.0.x and LangChain 0.1, as well as updating the `langchain_core.pydantic_v1` and `langchain.pydantic_v1` imports.
### Base packages​
Package| Latest| Recommended constraint  
---|---|---  
langchain| 0.3.0| >=0.3,<0.4  
langchain-community| 0.3.0| >=0.3,<0.4  
langchain-text-splitters| 0.3.0| >=0.3,<0.4  
langchain-core| 0.3.0| >=0.3,<0.4  
langchain-experimental| 0.3.0| >=0.3,<0.4  
### Downstream packages​
Package| Latest| Recommended constraint  
---|---|---  
langgraph| 0.2.20| >=0.2.20,<0.3  
langserve| 0.3.0| >=0.3,<0.4  
### Integration packages​
Package| Latest| Recommended constraint  
---|---|---  
langchain-ai21| 0.2.0| >=0.2,<0.3  
langchain-aws| 0.2.0| >=0.2,<0.3  
langchain-anthropic| 0.2.0| >=0.2,<0.3  
langchain-astradb| 0.4.1| >=0.4.1,<0.5  
langchain-azure-dynamic-sessions| 0.2.0| >=0.2,<0.3  
langchain-box| 0.2.0| >=0.2,<0.3  
langchain-chroma| 0.1.4| >=0.1.4,<0.2  
langchain-cohere| 0.3.0| >=0.3,<0.4  
langchain-elasticsearch| 0.3.0| >=0.3,<0.4  
langchain-exa| 0.2.0| >=0.2,<0.3  
langchain-fireworks| 0.2.0| >=0.2,<0.3  
langchain-groq| 0.2.0| >=0.2,<0.3  
langchain-google-community| 2.0.0| >=2,<3  
langchain-google-genai| 2.0.0| >=2,<3  
langchain-google-vertexai| 2.0.0| >=2,<3  
langchain-huggingface| 0.1.0| >=0.1,<0.2  
langchain-ibm| 0.3.0| >=0.3,<0.4  
langchain-milvus| 0.1.6| >=0.1.6,<0.2  
langchain-mistralai| 0.2.0| >=0.2,<0.3  
langchain-mongodb| 0.2.0| >=0.2,<0.3  
langchain-nomic| 0.1.3| >=0.1.3,<0.2  
langchain-nvidia| 0.3.0| >=0.3,<0.4  
langchain-ollama| 0.2.0| >=0.2,<0.3  
langchain-openai| 0.2.0| >=0.2,<0.3  
langchain-pinecone| 0.2.0| >=0.2,<0.3  
langchain-postgres| 0.0.13| >=0.0.13,<0.1  
langchain-prompty| 0.1.0| >=0.1,<0.2  
langchain-qdrant| 0.1.4| >=0.1.4,<0.2  
langchain-redis| 0.1.0| >=0.1,<0.2  
langchain-sema4| 0.2.0| >=0.2,<0.3  
langchain-together| 0.2.0| >=0.2,<0.3  
langchain-unstructured| 0.1.4| >=0.1.4,<0.2  
langchain-upstage| 0.3.0| >=0.3,<0.4  
langchain-voyageai| 0.2.0| >=0.2,<0.3  
langchain-weaviate| 0.0.3| >=0.0.3,<0.1  
Once you've updated to recent versions of the packages, you may need to address the following issues stemming from the internal switch from Pydantic v1 to Pydantic v2:
  * If your code depends on Pydantic aside from LangChain, you will need to upgrade your pydantic version constraints to be `pydantic>=2,<3`. See Pydantic’s migration guide for help migrating your non-LangChain code to Pydantic v2 if you use pydantic v1.
  * There are a number of side effects to LangChain components caused by the internal switch from Pydantic v1 to v2. We have listed some of the common cases below together with the recommended solutions.


## Common issues when transitioning to Pydantic 2​
### 1. Do not use the `langchain_core.pydantic_v1` namespace​
Replace any usage of `langchain_core.pydantic_v1` or `langchain.pydantic_v1` with direct imports from `pydantic`.
For example,
```
from langchain_core.pydantic_v1 import BaseModel
```

to:
```
from pydantic import BaseModel
```

This may require you to make additional updates to your Pydantic code given that there are a number of breaking changes in Pydantic 2. See the Pydantic Migration for how to upgrade your code from Pydantic 1 to 2.
### 2. Passing Pydantic objects to LangChain APIs​
Users using the following APIs:
  * `BaseChatModel.bind_tools`
  * `BaseChatModel.with_structured_output`
  * `Tool.from_function`
  * `StructuredTool.from_function`


should ensure that they are passing Pydantic 2 objects to these APIs rather than Pydantic 1 objects (created via the `pydantic.v1` namespace of pydantic 2).
caution
While `v1` objects may be accepted by some of these APIs, users are advised to use Pydantic 2 objects to avoid future issues.
### 3. Sub-classing LangChain models​
Any sub-classing from existing LangChain models (e.g., `BaseTool`, `BaseChatModel`, `LLM`) should upgrade to use Pydantic 2 features.
For example, any user code that's relying on Pydantic 1 features (e.g., `validator`) should be updated to the Pydantic 2 equivalent (e.g., `field_validator`), and any references to `pydantic.v1`, `langchain_core.pydantic_v1`, `langchain.pydantic_v1` should be replaced with imports from `pydantic`.
```
from pydantic.v1 import validator, Field # if pydantic 2 is installed# from pydantic import validator, Field # if pydantic 1 is installed# from langchain_core.pydantic_v1 import validator, Field# from langchain.pydantic_v1 import validator, FieldclassCustomTool(BaseTool):# BaseTool is v1 code  x:int= Field(default=1)def_run(*args,**kwargs):return"hello"@validator('x')# v1 code@classmethoddefvalidate_x(cls, x:int)->int:return1
```

Should change to:
```
from pydantic import Field, field_validator # pydantic v2from langchain_core.pydantic_v1 import BaseToolclassCustomTool(BaseTool):# BaseTool is v1 code  x:int= Field(default=1)def_run(*args,**kwargs):return"hello"@field_validator('x')# v2 code@classmethoddefvalidate_x(cls, x:int)->int:return1CustomTool(  name='custom_tool',  description="hello",  x=1,)
```

### 4. model_rebuild()​
When sub-classing from LangChain models, users may need to add relevant imports to the file and rebuild the model.
You can read more about `model_rebuild` here.
```
from langchain_core.output_parsers import BaseOutputParserclassFooParser(BaseOutputParser):...
```

**API Reference:**BaseOutputParser
New code:
```
from typing import Optional as Optionalfrom langchain_core.output_parsers import BaseOutputParserclassFooParser(BaseOutputParser):...FooParser.model_rebuild()
```

**API Reference:**BaseOutputParser
## Migrate using langchain-cli​
The `langchain-cli` can help update deprecated LangChain imports in your code automatically.
Please note that the `langchain-cli` only handles deprecated LangChain imports and cannot help to upgrade your code from pydantic 1 to pydantic 2.
For help with the Pydantic 1 to 2 migration itself please refer to the Pydantic Migration Guidelines.
As of 0.0.31, the `langchain-cli` relies on gritql for applying code mods.
### Installation​
```
pip install -U langchain-clilangchain-cli --version # <-- Make sure the version is at least 0.0.31
```

### Usage​
Given that the migration script is not perfect, you should make sure you have a backup of your code first (e.g., using version control like `git`).
The `langchain-cli` will handle the `langchain_core.pydantic_v1` deprecation introduced in LangChain 0.3 as well as older deprecations (e.g.,`from langchain.chat_models import ChatOpenAI` which should be `from langchain_openai import ChatOpenAI`),
You will need to run the migration script **twice** as it only applies one import replacement per run.
For example, say that your code is still using the old import `from langchain.chat_models import ChatOpenAI`:
After the first run, you’ll get: `from langchain_community.chat_models import ChatOpenAI` After the second run, you’ll get: `from langchain_openai import ChatOpenAI`
```
# Run a first time# Will replace from langchain.chat_models import ChatOpenAIlangchain-cli migrate --help [path to code] # Helplangchain-cli migrate [path to code] # Apply# Run a second time to apply more import replacementslangchain-cli migrate --diff [path to code] # Previewlangchain-cli migrate [path to code] # Apply
```

### Other options​
```
# See help menulangchain-cli migrate --help# Preview Changes without applyinglangchain-cli migrate --diff [path to code]# Approve changes interactivelylangchain-cli migrate --interactive [path to code]
```

#### Was this page helpful?
  * What's changed
  * What’s new
  * How to update your code
    * Base packages
    * Downstream packages
    * Integration packages
  * Common issues when transitioning to Pydantic 2
    * 1. Do not use the `langchain_core.pydantic_v1` namespace
    * 2. Passing Pydantic objects to LangChain APIs
    * 3. Sub-classing LangChain models
    * 4. model_rebuild()
  * Migrate using langchain-cli
    * Installation
    * Usage
    * Other options


