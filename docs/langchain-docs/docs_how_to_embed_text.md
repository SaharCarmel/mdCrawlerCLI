Skip to main content
**Join us at Interrupt: The Agent AI Conference by LangChain on May 13 & 14 in San Francisco!**
On this page
![Open on GitHub](https://img.shields.io/badge/Open%20on%20GitHub-grey?logo=github&logoColor=white)
info
Head to Integrations for documentation on built-in integrations with text embedding model providers.
The Embeddings class is a class designed for interfacing with text embedding models. There are lots of embedding model providers (OpenAI, Cohere, Hugging Face, etc) - this class is designed to provide a standard interface for all of them.
Embeddings create a vector representation of a piece of text. This is useful because it means we can think about text in the vector space, and do things like semantic search where we look for pieces of text that are most similar in the vector space.
The base Embeddings class in LangChain provides two methods: one for embedding documents and one for embedding a query. The former, `.embed_documents`, takes as input multiple texts, while the latter, `.embed_query`, takes a single text. The reason for having these as two separate methods is that some embedding providers have different embedding methods for documents (to be searched over) vs queries (the search query itself). `.embed_query` will return a list of floats, whereas `.embed_documents` returns a list of lists of floats.
## Get started​
### Setup​
Select embeddings model:
OpenAI▾
* OpenAI
* Azure
* Google
* AWS
* HuggingFace
* Ollama
* Cohere
* MistralAI
* Nomic
* NVIDIA
* Voyage AI
* IBM watsonx
* Fake
```
pip install -qU langchain-openai
```

```
import getpassimport osifnot os.environ.get("OPENAI_API_KEY"): os.environ["OPENAI_API_KEY"]= getpass.getpass("Enter API key for OpenAI: ")from langchain_openai import OpenAIEmbeddingsembeddings_model = OpenAIEmbeddings(model="text-embedding-3-large")
```

### `embed_documents`​
#### Embed list of texts​
Use `.embed_documents` to embed a list of strings, recovering a list of embeddings:
```
embeddings = embeddings_model.embed_documents(["Hi there!","Oh, hello!","What's your name?","My friends call me World","Hello World!"])len(embeddings),len(embeddings[0])
```

```
(5, 1536)
```

### `embed_query`​
#### Embed single query​
Use `.embed_query` to embed a single piece of text (e.g., for the purpose of comparing to other embedded pieces of texts).
```
embedded_query = embeddings_model.embed_query("What was the name mentioned in the conversation?")embedded_query[:5]
```

```
[0.0053587136790156364, -0.0004999046213924885, 0.038883671164512634, -0.003001077566295862, -0.00900818221271038]
```

#### Was this page helpful?
  * Get started
    * Setup
    * `embed_documents`
    * `embed_query`


