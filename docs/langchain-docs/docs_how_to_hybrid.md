Skip to main content
**Join us at Interrupt: The Agent AI Conference by LangChain on May 13 & 14 in San Francisco!**
On this page
![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)![Open on GitHub](https://img.shields.io/badge/Open%20on%20GitHub-grey?logo=github&logoColor=white)
The standard search in LangChain is done by vector similarity. However, a number of vector store implementations (Astra DB, ElasticSearch, Neo4J, AzureSearch, Qdrant...) also support more advanced search combining vector similarity search and other search techniques (full-text, BM25, and so on). This is generally referred to as "Hybrid" search.
**Step 1: Make sure the vectorstore you are using supports hybrid search**
At the moment, there is no unified way to perform hybrid search in LangChain. Each vectorstore may have their own way to do it. This is generally exposed as a keyword argument that is passed in during `similarity_search`.
By reading the documentation or source code, figure out whether the vectorstore you are using supports hybrid search, and, if so, how to use it.
**Step 2: Add that parameter as a configurable field for the chain**
This will let you easily call the chain and configure any relevant flags at runtime. See this documentation for more information on configuration.
**Step 3: Call the chain with that configurable field**
Now, at runtime you can call this chain with configurable field.
## Code Example​
Let's see a concrete example of what this looks like in code. We will use the Cassandra/CQL interface of Astra DB for this example.
Install the following Python package:
```
!pip install "cassio>=0.1.7"
```

Get the connection secrets.
Initialize cassio:
```
import cassiocassio.init(  database_id="Your database ID",  token="Your application token",  keyspace="Your key space",)
```

Create the Cassandra VectorStore with a standard index analyzer. The index analyzer is needed to enable term matching.
```
from cassio.table.cql import STANDARD_ANALYZERfrom langchain_community.vectorstores import Cassandrafrom langchain_openai import OpenAIEmbeddingsembeddings = OpenAIEmbeddings()vectorstore = Cassandra(  embedding=embeddings,  table_name="test_hybrid",  body_index_options=[STANDARD_ANALYZER],  session=None,  keyspace=None,)vectorstore.add_texts(["In 2023, I visited Paris","In 2022, I visited New York","In 2021, I visited New Orleans",])
```

**API Reference:**Cassandra | OpenAIEmbeddings
If we do a standard similarity search, we get all the documents:
```
vectorstore.as_retriever().invoke("What city did I visit last?")
```

```
[Document(page_content='In 2022, I visited New York'),Document(page_content='In 2023, I visited Paris'),Document(page_content='In 2021, I visited New Orleans')]
```

The Astra DB vectorstore `body_search` argument can be used to filter the search on the term `new`.
```
vectorstore.as_retriever(search_kwargs={"body_search":"new"}).invoke("What city did I visit last?")
```

```
[Document(page_content='In 2022, I visited New York'),Document(page_content='In 2021, I visited New Orleans')]
```

We can now create the chain that we will use to do question-answering over
```
from langchain_core.output_parsers import StrOutputParserfrom langchain_core.prompts import ChatPromptTemplatefrom langchain_core.runnables import(  ConfigurableField,  RunnablePassthrough,)from langchain_openai import ChatOpenAI
```

**API Reference:**StrOutputParser | ChatPromptTemplate | ConfigurableField | RunnablePassthrough | ChatOpenAI
This is basic question-answering chain set up.
```
template ="""Answer the question based only on the following context:{context}Question: {question}"""prompt = ChatPromptTemplate.from_template(template)model = ChatOpenAI()retriever = vectorstore.as_retriever()
```

Here we mark the retriever as having a configurable field. All vectorstore retrievers have `search_kwargs` as a field. This is just a dictionary, with vectorstore specific fields
```
configurable_retriever = retriever.configurable_fields(  search_kwargs=ConfigurableField(id="search_kwargs",    name="Search Kwargs",    description="The search kwargs to use",))
```

We can now create the chain using our configurable retriever
```
chain =({"context": configurable_retriever,"question": RunnablePassthrough()}| prompt| model| StrOutputParser())
```

```
chain.invoke("What city did I visit last?")
```

```
Paris
```

We can now invoke the chain with configurable options. `search_kwargs` is the id of the configurable field. The value is the search kwargs to use for Astra DB.
```
chain.invoke("What city did I visit last?",  config={"configurable":{"search_kwargs":{"body_search":"new"}}},)
```

```
New York
```

#### Was this page helpful?
  * Code Example


