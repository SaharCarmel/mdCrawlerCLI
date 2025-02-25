Skip to main content
**Join us at Interrupt: The Agent AI Conference by LangChain on May 13 & 14 in San Francisco!**
On this page
![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)![Open on GitHub](https://img.shields.io/badge/Open%20on%20GitHub-grey?logo=github&logoColor=white)
Splitting HTML documents into manageable chunks is essential for various text processing tasks such as natural language processing, search indexing, and more. In this guide, we will explore three different text splitters provided by LangChain that you can use to split HTML content effectively:
  * **HTMLHeaderTextSplitter**
  * **HTMLSectionSplitter**
  * **HTMLSemanticPreservingSplitter**


Each of these splitters has unique features and use cases. This guide will help you understand the differences between them, why you might choose one over the others, and how to use them effectively.
```
%pip install -qU langchain-text-splitters
```

## Overview of the Splitters​
### HTMLHeaderTextSplitter​
info
Useful when you want to preserve the hierarchical structure of a document based on its headings.
**Description** : Splits HTML text based on header tags (e.g., `<h1>`, `<h2>`, `<h3>`, etc.), and adds metadata for each header relevant to any given chunk.
**Capabilities** :
  * Splits text at the HTML element level.
  * Preserves context-rich information encoded in document structures.
  * Can return chunks element by element or combine elements with the same metadata.


### HTMLSectionSplitter​
info
Useful when you want to split HTML documents into larger sections, such as `<section>`, `<div>`, or custom-defined sections.
**Description** : Similar to HTMLHeaderTextSplitter but focuses on splitting HTML into sections based on specified tags.
**Capabilities** :
  * Uses XSLT transformations to detect and split sections.
  * Internally uses `RecursiveCharacterTextSplitter` for large sections.
  * Considers font sizes to determine sections.


### HTMLSemanticPreservingSplitter​
info
Ideal when you need to ensure that structured elements are not split across chunks, preserving contextual relevancy.
**Description** : Splits HTML content into manageable chunks while preserving the semantic structure of important elements like tables, lists, and other HTML components.
**Capabilities** :
  * Preserves tables, lists, and other specified HTML elements.
  * Allows custom handlers for specific HTML tags.
  * Ensures that the semantic meaning of the document is maintained.
  * Built in normalization & stopword removal


### Choosing the Right Splitter​
  * **Use`HTMLHeaderTextSplitter` when**: You need to split an HTML document based on its header hierarchy and maintain metadata about the headers.
  * **Use`HTMLSectionSplitter` when**: You need to split the document into larger, more general sections, possibly based on custom tags or font sizes.
  * **Use`HTMLSemanticPreservingSplitter` when**: You need to split the document into chunks while preserving semantic elements like tables and lists, ensuring that they are not split and that their context is maintained.

Feature| HTMLHeaderTextSplitter| HTMLSectionSplitter| HTMLSemanticPreservingSplitter  
---|---|---|---  
Splits based on headers| Yes| Yes| Yes  
Preserves semantic elements (tables, lists)| No| No| Yes  
Adds metadata for headers| Yes| Yes| Yes  
Custom handlers for HTML tags| No| No| Yes  
Preserves media (images, videos)| No| No| Yes  
Considers font sizes| No| Yes| No  
Uses XSLT transformations| No| Yes| No  
## Example HTML Document​
Let's use the following HTML document as an example:
```
html_string ="""<!DOCTYPE html> <html lang='en'> <head>  <meta charset='UTF-8'>  <meta name='viewport' content='width=device-width, initial-scale=1.0'>  <title>Fancy Example HTML Page</title> </head> <body>  <h1>Main Title</h1>  <p>This is an introductory paragraph with some basic content.</p>  <h2>Section 1: Introduction</h2>  <p>This section introduces the topic. Below is a list:</p>  <ul>   <li>First item</li>   <li>Second item</li>   <li>Third item with <strong>bold text</strong> and <a href='#'>a link</a></li>  </ul>  <h3>Subsection 1.1: Details</h3>  <p>This subsection provides additional details. Here's a table:</p>  <table border='1'>   <thead>    <tr>     <th>Header 1</th>     <th>Header 2</th>     <th>Header 3</th>    </tr>   </thead>   <tbody>    <tr>     <td>Row 1, Cell 1</td>     <td>Row 1, Cell 2</td>     <td>Row 1, Cell 3</td>    </tr>    <tr>     <td>Row 2, Cell 1</td>     <td>Row 2, Cell 2</td>     <td>Row 2, Cell 3</td>    </tr>   </tbody>  </table>  <h2>Section 2: Media Content</h2>  <p>This section contains an image and a video:</p>   <img src='example_image_link.mp4' alt='Example Image'>   <video controls width='250' src='example_video_link.mp4' type='video/mp4'>   Your browser does not support the video tag.  </video>  <h2>Section 3: Code Example</h2>  <p>This section contains a code block:</p>  <pre><code data-lang="html">  &lt;div&gt;   &lt;p&gt;This is a paragraph inside a div.&lt;/p&gt;  &lt;/div&gt;  </code></pre>  <h2>Conclusion</h2>  <p>This is the conclusion of the document.</p> </body> </html>"""
```

## Using HTMLHeaderTextSplitter​
HTMLHeaderTextSplitter is a "structure-aware" text splitter that splits text at the HTML element level and adds metadata for each header "relevant" to any given chunk. It can return chunks element by element or combine elements with the same metadata, with the objectives of (a) keeping related text grouped (more or less) semantically and (b) preserving context-rich information encoded in document structures. It can be used with other text splitters as part of a chunking pipeline.
It is analogous to the MarkdownHeaderTextSplitter for markdown files.
To specify what headers to split on, specify `headers_to_split_on` when instantiating `HTMLHeaderTextSplitter` as shown below.
```
from langchain_text_splitters import HTMLHeaderTextSplitterheaders_to_split_on =[("h1","Header 1"),("h2","Header 2"),("h3","Header 3"),]html_splitter = HTMLHeaderTextSplitter(headers_to_split_on)html_header_splits = html_splitter.split_text(html_string)html_header_splits
```

**API Reference:**HTMLHeaderTextSplitter
```
[Document(metadata={'Header 1': 'Main Title'}, page_content='This is an introductory paragraph with some basic content.'), Document(metadata={'Header 1': 'Main Title', 'Header 2': 'Section 1: Introduction'}, page_content='This section introduces the topic. Below is a list: \nFirst item Second item Third item with bold text and a link'), Document(metadata={'Header 1': 'Main Title', 'Header 2': 'Section 1: Introduction', 'Header 3': 'Subsection 1.1: Details'}, page_content="This subsection provides additional details. Here's a table:"), Document(metadata={'Header 1': 'Main Title', 'Header 2': 'Section 2: Media Content'}, page_content='This section contains an image and a video:'), Document(metadata={'Header 1': 'Main Title', 'Header 2': 'Section 3: Code Example'}, page_content='This section contains a code block:'), Document(metadata={'Header 1': 'Main Title', 'Header 2': 'Conclusion'}, page_content='This is the conclusion of the document.')]
```

To return each element together with their associated headers, specify `return_each_element=True` when instantiating `HTMLHeaderTextSplitter`:
```
html_splitter = HTMLHeaderTextSplitter(  headers_to_split_on,  return_each_element=True,)html_header_splits_elements = html_splitter.split_text(html_string)
```

Comparing with the above, where elements are aggregated by their headers:
```
for element in html_header_splits[:2]:print(element)
```

```
page_content='This is an introductory paragraph with some basic content.' metadata={'Header 1': 'Main Title'}page_content='This section introduces the topic. Below is a list: First item Second item Third item with bold text and a link' metadata={'Header 1': 'Main Title', 'Header 2': 'Section 1: Introduction'}
```

Now each element is returned as a distinct `Document`:
```
for element in html_header_splits_elements[:3]:print(element)
```

```
page_content='This is an introductory paragraph with some basic content.' metadata={'Header 1': 'Main Title'}page_content='This section introduces the topic. Below is a list:' metadata={'Header 1': 'Main Title', 'Header 2': 'Section 1: Introduction'}page_content='First item Second item Third item with bold text and a link' metadata={'Header 1': 'Main Title', 'Header 2': 'Section 1: Introduction'}
```

### How to split from a URL or HTML file:​
To read directly from a URL, pass the URL string into the `split_text_from_url` method.
Similarly, a local HTML file can be passed to the `split_text_from_file` method.
```
url ="https://plato.stanford.edu/entries/goedel/"headers_to_split_on =[("h1","Header 1"),("h2","Header 2"),("h3","Header 3"),("h4","Header 4"),]html_splitter = HTMLHeaderTextSplitter(headers_to_split_on)# for local file use html_splitter.split_text_from_file(<path_to_file>)html_header_splits = html_splitter.split_text_from_url(url)
```

### How to constrain chunk sizes:​
`HTMLHeaderTextSplitter`, which splits based on HTML headers, can be composed with another splitter which constrains splits based on character lengths, such as `RecursiveCharacterTextSplitter`.
This can be done using the `.split_documents` method of the second splitter:
```
from langchain_text_splitters import RecursiveCharacterTextSplitterchunk_size =500chunk_overlap =30text_splitter = RecursiveCharacterTextSplitter(  chunk_size=chunk_size, chunk_overlap=chunk_overlap)# Splitsplits = text_splitter.split_documents(html_header_splits)splits[80:85]
```

**API Reference:**RecursiveCharacterTextSplitter
```
[Document(metadata={'Header 1': 'Kurt Gödel', 'Header 2': '2. Gödel’s Mathematical Work', 'Header 3': '2.2 The Incompleteness Theorems', 'Header 4': '2.2.1 The First Incompleteness Theorem'}, page_content='We see that Gödel first tried to reduce the consistency problem for analysis to that of arithmetic. This seemed to require a truth definition for arithmetic, which in turn led to paradoxes, such as the Liar paradox (“This sentence is false”) and Berry’s paradox (“The least number not defined by an expression consisting of just fourteen English words”). Gödel then noticed that such paradoxes would not necessarily arise if truth were replaced by provability. But this means that arithmetic truth'), Document(metadata={'Header 1': 'Kurt Gödel', 'Header 2': '2. Gödel’s Mathematical Work', 'Header 3': '2.2 The Incompleteness Theorems', 'Header 4': '2.2.1 The First Incompleteness Theorem'}, page_content='means that arithmetic truth and arithmetic provability are not co-extensive — whence the First Incompleteness Theorem.'), Document(metadata={'Header 1': 'Kurt Gödel', 'Header 2': '2. Gödel’s Mathematical Work', 'Header 3': '2.2 The Incompleteness Theorems', 'Header 4': '2.2.1 The First Incompleteness Theorem'}, page_content='This account of Gödel’s discovery was told to Hao Wang very much after the fact; but in Gödel’s contemporary correspondence with Bernays and Zermelo, essentially the same description of his path to the theorems is given. (See Gödel 2003a and Gödel 2003b respectively.) From those accounts we see that the undefinability of truth in arithmetic, a result credited to Tarski, was likely obtained in some form by Gödel by 1931. But he neither publicized nor published the result; the biases logicians'), Document(metadata={'Header 1': 'Kurt Gödel', 'Header 2': '2. Gödel’s Mathematical Work', 'Header 3': '2.2 The Incompleteness Theorems', 'Header 4': '2.2.1 The First Incompleteness Theorem'}, page_content='result; the biases logicians had expressed at the time concerning the notion of truth, biases which came vehemently to the fore when Tarski announced his results on the undefinability of truth in formal systems 1935, may have served as a deterrent to Gödel’s publication of that theorem.'), Document(metadata={'Header 1': 'Kurt Gödel', 'Header 2': '2. Gödel’s Mathematical Work', 'Header 3': '2.2 The Incompleteness Theorems', 'Header 4': '2.2.2 The proof of the First Incompleteness Theorem'}, page_content='We now describe the proof of the two theorems, formulating Gödel’s results in Peano arithmetic. Gödel himself used a system related to that defined in Principia Mathematica, but containing Peano arithmetic. In our presentation of the First and Second Incompleteness Theorems we refer to Peano arithmetic as P, following Gödel’s notation.')]
```

### Limitations​
There can be quite a bit of structural variation from one HTML document to another, and while `HTMLHeaderTextSplitter` will attempt to attach all "relevant" headers to any given chunk, it can sometimes miss certain headers. For example, the algorithm assumes an informational hierarchy in which headers are always at nodes "above" associated text, i.e. prior siblings, ancestors, and combinations thereof. In the following news article (as of the writing of this document), the document is structured such that the text of the top-level headline, while tagged "h1", is in a _distinct_ subtree from the text elements that we'd expect it to be _"above"_ —so we can observe that the "h1" element and its associated text do not show up in the chunk metadata (but, where applicable, we do see "h2" and its associated text):
```
url ="https://www.cnn.com/2023/09/25/weather/el-nino-winter-us-climate/index.html"headers_to_split_on =[("h1","Header 1"),("h2","Header 2"),]html_splitter = HTMLHeaderTextSplitter(headers_to_split_on)html_header_splits = html_splitter.split_text_from_url(url)print(html_header_splits[1].page_content[:500])
```

```
No two El Niño winters are the same, but many have temperature and precipitation trends in common. Average conditions during an El Niño winter across the continental US. One of the major reasons is the position of the jet stream, which often shifts south during an El Niño winter. This shift typically brings wetter and cooler weather to the South while the North becomes drier and warmer, according to NOAA. Because the jet stream is essentially a river of air that storms flow through, they c
```

## Using HTMLSectionSplitter​
Similar in concept to the HTMLHeaderTextSplitter, the `HTMLSectionSplitter` is a "structure-aware" text splitter that splits text at the element level and adds metadata for each header "relevant" to any given chunk. It lets you split HTML by sections.
It can return chunks element by element or combine elements with the same metadata, with the objectives of (a) keeping related text grouped (more or less) semantically and (b) preserving context-rich information encoded in document structures.
Use `xslt_path` to provide an absolute path to transform the HTML so that it can detect sections based on provided tags. The default is to use the `converting_to_header.xslt` file in the `data_connection/document_transformers` directory. This is for converting the html to a format/layout that is easier to detect sections. For example, `span` based on their font size can be converted to header tags to be detected as a section.
### How to split HTML strings:​
```
from langchain_text_splitters import HTMLSectionSplitterheaders_to_split_on =[("h1","Header 1"),("h2","Header 2"),]html_splitter = HTMLSectionSplitter(headers_to_split_on)html_header_splits = html_splitter.split_text(html_string)html_header_splits
```

**API Reference:**HTMLSectionSplitter
```
[Document(metadata={'Header 1': 'Main Title'}, page_content='Main Title \n This is an introductory paragraph with some basic content.'), Document(metadata={'Header 2': 'Section 1: Introduction'}, page_content="Section 1: Introduction \n This section introduces the topic. Below is a list: \n \n First item \n Second item \n Third item with bold text and a link \n \n \n Subsection 1.1: Details \n This subsection provides additional details. Here's a table: \n \n \n \n Header 1 \n Header 2 \n Header 3 \n \n \n \n \n Row 1, Cell 1 \n Row 1, Cell 2 \n Row 1, Cell 3 \n \n \n Row 2, Cell 1 \n Row 2, Cell 2 \n Row 2, Cell 3"), Document(metadata={'Header 2': 'Section 2: Media Content'}, page_content='Section 2: Media Content \n This section contains an image and a video: \n \n \n   Your browser does not support the video tag.'), Document(metadata={'Header 2': 'Section 3: Code Example'}, page_content='Section 3: Code Example \n This section contains a code block: \n \n  <div>\n   <p>This is a paragraph inside a div.</p>\n  </div>'), Document(metadata={'Header 2': 'Conclusion'}, page_content='Conclusion \n This is the conclusion of the document.')]
```

### How to constrain chunk sizes:​
`HTMLSectionSplitter` can be used with other text splitters as part of a chunking pipeline. Internally, it uses the `RecursiveCharacterTextSplitter` when the section size is larger than the chunk size. It also considers the font size of the text to determine whether it is a section or not based on the determined font size threshold.
```
from langchain_text_splitters import RecursiveCharacterTextSplitterheaders_to_split_on =[("h1","Header 1"),("h2","Header 2"),("h3","Header 3"),]html_splitter = HTMLSectionSplitter(headers_to_split_on)html_header_splits = html_splitter.split_text(html_string)chunk_size =50chunk_overlap =5text_splitter = RecursiveCharacterTextSplitter(  chunk_size=chunk_size, chunk_overlap=chunk_overlap)# Splitsplits = text_splitter.split_documents(html_header_splits)splits
```

**API Reference:**RecursiveCharacterTextSplitter
```
[Document(metadata={'Header 1': 'Main Title'}, page_content='Main Title'), Document(metadata={'Header 1': 'Main Title'}, page_content='This is an introductory paragraph with some'), Document(metadata={'Header 1': 'Main Title'}, page_content='some basic content.'), Document(metadata={'Header 2': 'Section 1: Introduction'}, page_content='Section 1: Introduction'), Document(metadata={'Header 2': 'Section 1: Introduction'}, page_content='This section introduces the topic. Below is a'), Document(metadata={'Header 2': 'Section 1: Introduction'}, page_content='is a list:'), Document(metadata={'Header 2': 'Section 1: Introduction'}, page_content='First item \n Second item'), Document(metadata={'Header 2': 'Section 1: Introduction'}, page_content='Third item with bold text and a link'), Document(metadata={'Header 3': 'Subsection 1.1: Details'}, page_content='Subsection 1.1: Details'), Document(metadata={'Header 3': 'Subsection 1.1: Details'}, page_content='This subsection provides additional details.'), Document(metadata={'Header 3': 'Subsection 1.1: Details'}, page_content="Here's a table:"), Document(metadata={'Header 3': 'Subsection 1.1: Details'}, page_content='Header 1 \n Header 2 \n Header 3'), Document(metadata={'Header 3': 'Subsection 1.1: Details'}, page_content='Row 1, Cell 1 \n Row 1, Cell 2'), Document(metadata={'Header 3': 'Subsection 1.1: Details'}, page_content='Row 1, Cell 3 \n \n \n Row 2, Cell 1'), Document(metadata={'Header 3': 'Subsection 1.1: Details'}, page_content='Row 2, Cell 2 \n Row 2, Cell 3'), Document(metadata={'Header 2': 'Section 2: Media Content'}, page_content='Section 2: Media Content'), Document(metadata={'Header 2': 'Section 2: Media Content'}, page_content='This section contains an image and a video:'), Document(metadata={'Header 2': 'Section 2: Media Content'}, page_content='Your browser does not support the video'), Document(metadata={'Header 2': 'Section 2: Media Content'}, page_content='tag.'), Document(metadata={'Header 2': 'Section 3: Code Example'}, page_content='Section 3: Code Example'), Document(metadata={'Header 2': 'Section 3: Code Example'}, page_content='This section contains a code block: \n \n  <div>'), Document(metadata={'Header 2': 'Section 3: Code Example'}, page_content='<p>This is a paragraph inside a div.</p>'), Document(metadata={'Header 2': 'Section 3: Code Example'}, page_content='</div>'), Document(metadata={'Header 2': 'Conclusion'}, page_content='Conclusion'), Document(metadata={'Header 2': 'Conclusion'}, page_content='This is the conclusion of the document.')]
```

## Using HTMLSemanticPreservingSplitter​
The `HTMLSemanticPreservingSplitter` is designed to split HTML content into manageable chunks while preserving the semantic structure of important elements like tables, lists, and other HTML components. This ensures that such elements are not split across chunks, causing loss of contextual relevancy such as table headers, list headers etc.
This splitter is designed at its heart, to create contextually relevant chunks. General Recursive splitting with `HTMLHeaderTextSplitter` can cause tables, lists and other structered elements to be split in the middle, losing signifcant context and creating bad chunks.
The `HTMLSemanticPreservingSplitter` is essential for splitting HTML content that includes structured elements like tables and lists, especially when it's critical to preserve these elements intact. Additionally, its ability to define custom handlers for specific HTML tags makes it a versatile tool for processing complex HTML documents.
**IMPORTANT** : `max_chunk_size` is not a definite maximum size of a chunk, the calculation of max size, occurs when the preserved content is not apart of the chunk, to ensure it is not split. When we add the preserved data back in to the chunk, there is a chance the chunk size will exceed the `max_chunk_size`. This is crucial to ensure we maintain the structure of the original document
info
Notes:
  1. We have defined a custom handler to re-format the contents of code blocks
  2. We defined a deny list for specific html elements, to decompose them and their contents pre-processing
  3. We have intentionally set a small chunk size to demonstrate the non-splitting of elements


```
# BeautifulSoup is required to use the custom handlersfrom bs4 import Tagfrom langchain_text_splitters import HTMLSemanticPreservingSplitterheaders_to_split_on =[("h1","Header 1"),("h2","Header 2"),]defcode_handler(element: Tag)->str:  data_lang = element.get("data-lang")  code_format =f"<code:{data_lang}>{element.get_text()}</code>"return code_formatsplitter = HTMLSemanticPreservingSplitter(  headers_to_split_on=headers_to_split_on,  separators=["\n\n","\n",". ","! ","? "],  max_chunk_size=50,  preserve_images=True,  preserve_videos=True,  elements_to_preserve=["table","ul","ol","code"],  denylist_tags=["script","style","head"],  custom_handlers={"code": code_handler},)documents = splitter.split_text(html_string)documents
```

**API Reference:**HTMLSemanticPreservingSplitter
```
[Document(metadata={'Header 1': 'Main Title'}, page_content='This is an introductory paragraph with some basic content.'), Document(metadata={'Header 2': 'Section 1: Introduction'}, page_content='This section introduces the topic'), Document(metadata={'Header 2': 'Section 1: Introduction'}, page_content='. Below is a list: First item Second item Third item with bold text and a link Subsection 1.1: Details This subsection provides additional details'), Document(metadata={'Header 2': 'Section 1: Introduction'}, page_content=". Here's a table: Header 1 Header 2 Header 3 Row 1, Cell 1 Row 1, Cell 2 Row 1, Cell 3 Row 2, Cell 1 Row 2, Cell 2 Row 2, Cell 3"), Document(metadata={'Header 2': 'Section 2: Media Content'}, page_content='This section contains an image and a video: ![image:example_image_link.mp4](example_image_link.mp4) ![video:example_video_link.mp4](example_video_link.mp4)'), Document(metadata={'Header 2': 'Section 3: Code Example'}, page_content='This section contains a code block: <code:html> <div> <p>This is a paragraph inside a div.</p> </div> </code>'), Document(metadata={'Header 2': 'Conclusion'}, page_content='This is the conclusion of the document.')]
```

### Preserving Tables and Lists​
In this example, we will demonstrate how the `HTMLSemanticPreservingSplitter` can preserve a table and a large list within an HTML document. The chunk size will be set to 50 characters to illustrate how the splitter ensures that these elements are not split, even when they exceed the maximum defined chunk size.
```
from langchain_text_splitters import HTMLSemanticPreservingSplitterhtml_string ="""<!DOCTYPE html><html>  <body>    <div>      <h1>Section 1</h1>      <p>This section contains an important table and list that should not be split across chunks.</p>      <table>        <tr>          <th>Item</th>          <th>Quantity</th>          <th>Price</th>        </tr>        <tr>          <td>Apples</td>          <td>10</td>          <td>$1.00</td>        </tr>        <tr>          <td>Oranges</td>          <td>5</td>          <td>$0.50</td>        </tr>        <tr>          <td>Bananas</td>          <td>50</td>          <td>$1.50</td>        </tr>      </table>      <h2>Subsection 1.1</h2>      <p>Additional text in subsection 1.1 that is separated from the table and list.</p>      <p>Here is a detailed list:</p>      <ul>        <li>Item 1: Description of item 1, which is quite detailed and important.</li>        <li>Item 2: Description of item 2, which also contains significant information.</li>        <li>Item 3: Description of item 3, another item that we don't want to split across chunks.</li>      </ul>    </div>  </body></html>"""headers_to_split_on =[("h1","Header 1"),("h2","Header 2")]splitter = HTMLSemanticPreservingSplitter(  headers_to_split_on=headers_to_split_on,  max_chunk_size=50,  elements_to_preserve=["table","ul"],)documents = splitter.split_text(html_string)print(documents)
```

**API Reference:**HTMLSemanticPreservingSplitter
```
[Document(metadata={'Header 1': 'Section 1'}, page_content='This section contains an important table and list'), Document(metadata={'Header 1': 'Section 1'}, page_content='that should not be split across chunks.'), Document(metadata={'Header 1': 'Section 1'}, page_content='Item Quantity Price Apples 10 $1.00 Oranges 5 $0.50 Bananas 50 $1.50'), Document(metadata={'Header 2': 'Subsection 1.1'}, page_content='Additional text in subsection 1.1 that is'), Document(metadata={'Header 2': 'Subsection 1.1'}, page_content='separated from the table and list. Here is a'), Document(metadata={'Header 2': 'Subsection 1.1'}, page_content="detailed list: Item 1: Description of item 1, which is quite detailed and important. Item 2: Description of item 2, which also contains significant information. Item 3: Description of item 3, another item that we don't want to split across chunks.")]
```

#### Explanation​
In this example, the `HTMLSemanticPreservingSplitter` ensures that the entire table and the unordered list (`<ul>`) are preserved within their respective chunks. Even though the chunk size is set to 50 characters, the splitter recognizes that these elements should not be split and keeps them intact.
This is particularly important when dealing with data tables or lists, where splitting the content could lead to loss of context or confusion. The resulting `Document` objects retain the full structure of these elements, ensuring that the contextual relevance of the information is maintained.
### Using a Custom Handler​
The `HTMLSemanticPreservingSplitter` allows you to define custom handlers for specific HTML elements. Some platforms, have custom HTML tags that are not natively parsed by `BeautifulSoup`, when this occurs, you can utilize custom handlers to add the formatting logic easily.
This can be particularly useful for elements that require special processing, such as `<iframe>` tags or specific 'data-' elements. In this example, we'll create a custom handler for `iframe` tags that converts them into Markdown-like links.
```
defcustom_iframe_extractor(iframe_tag):  iframe_src = iframe_tag.get("src","")returnf"[iframe:{iframe_src}]({iframe_src})"splitter = HTMLSemanticPreservingSplitter(  headers_to_split_on=headers_to_split_on,  max_chunk_size=50,  separators=["\n\n","\n",". "],  elements_to_preserve=["table","ul","ol"],  custom_handlers={"iframe": custom_iframe_extractor},)html_string ="""<!DOCTYPE html><html>  <body>    <div>      <h1>Section with Iframe</h1>      <iframe src="https://example.com/embed"></iframe>      <p>Some text after the iframe.</p>      <ul>        <li>Item 1: Description of item 1, which is quite detailed and important.</li>        <li>Item 2: Description of item 2, which also contains significant information.</li>        <li>Item 3: Description of item 3, another item that we don't want to split across chunks.</li>      </ul>    </div>  </body></html>"""documents = splitter.split_text(html_string)print(documents)
```

```
[Document(metadata={'Header 1': 'Section with Iframe'}, page_content='[iframe:https://example.com/embed](https://example.com/embed) Some text after the iframe'), Document(metadata={'Header 1': 'Section with Iframe'}, page_content=". Item 1: Description of item 1, which is quite detailed and important. Item 2: Description of item 2, which also contains significant information. Item 3: Description of item 3, another item that we don't want to split across chunks.")]
```

#### Explanation​
In this example, we defined a custom handler for `iframe` tags that converts them into Markdown-like links. When the splitter processes the HTML content, it uses this custom handler to transform the `iframe` tags while preserving other elements like tables and lists. The resulting `Document` objects show how the iframe is handled according to the custom logic you provided.
**Important** : When presvering items such as links, you should be mindful not to include `.` in your seperators, or leave seperators blank. `RecursiveCharacterTextSplitter` splits on full stop, which will cut links in half. Ensure you provide a seperator list with `. ` instead.
### Using a custom handler to analyze an image with an LLM​
With custom handler's, we can also override the default processing for any element. A great example of this, is inserting semantic analysis of an image within a document, directly in the chunking flow.
Since our function is called when the tag is discovered, we can override the `<img>` tag and turn off `preserve_images` to insert any content we would like to embed in our chunks.
```
"""This example assumes you have helper methods `load_image_from_url` and an LLM agent `llm` that can process image data."""from langchain.agents import AgentExecutor# This example needs to be replaced with your own agentllm = AgentExecutor(...)# This method is a placeholder for loading image data from a URL and is not implemented heredefload_image_from_url(image_url:str)->bytes:# Assuming this method fetches the image data from the URLreturnb"image_data"html_string ="""<!DOCTYPE html><html>  <body>    <div>      <h1>Section with Image and Link</h1>      <p>        <img src="https://example.com/image.jpg" alt="An example image" />        Some text after the image.      </p>      <ul>        <li>Item 1: Description of item 1, which is quite detailed and important.</li>        <li>Item 2: Description of item 2, which also contains significant information.</li>        <li>Item 3: Description of item 3, another item that we don't want to split across chunks.</li>      </ul>    </div>  </body></html>"""defcustom_image_handler(img_tag)->str:  img_src = img_tag.get("src","")  img_alt = img_tag.get("alt","No alt text provided")  image_data = load_image_from_url(img_src)  semantic_meaning = llm.invoke(image_data)  markdown_text =f"[Image Alt Text: {img_alt} | Image Source: {img_src} | Image Semantic Meaning: {semantic_meaning}]"return markdown_textsplitter = HTMLSemanticPreservingSplitter(  headers_to_split_on=headers_to_split_on,  max_chunk_size=50,  separators=["\n\n","\n",". "],  elements_to_preserve=["ul"],  preserve_images=False,  custom_handlers={"img": custom_image_handler},)documents = splitter.split_text(html_string)print(documents)
```

**API Reference:**AgentExecutor
```
[Document(metadata={'Header 1': 'Section with Image and Link'}, page_content='[Image Alt Text: An example image | Image Source: https://example.com/image.jpg | Image Semantic Meaning: semantic-meaning] Some text after the image'), Document(metadata={'Header 1': 'Section with Image and Link'}, page_content=". Item 1: Description of item 1, which is quite detailed and important. Item 2: Description of item 2, which also contains significant information. Item 3: Description of item 3, another item that we don't want to split across chunks.")]
```

#### Explanation:​
With our custom handler written to extract the specific fields from a `<img>` element in HTML, we can further process the data with our agent, and insert the result directly into our chunk. It is important to ensure `preserve_images` is set to `False` otherwise the default processing of `<img>` fields will take place.
#### Was this page helpful?
  * Overview of the Splitters
    * HTMLHeaderTextSplitter
    * HTMLSectionSplitter
    * HTMLSemanticPreservingSplitter
    * Choosing the Right Splitter
  * Example HTML Document
  * Using HTMLHeaderTextSplitter
    * How to split from a URL or HTML file:
    * How to constrain chunk sizes:
    * Limitations
  * Using HTMLSectionSplitter
    * How to split HTML strings:
    * How to constrain chunk sizes:
  * Using HTMLSemanticPreservingSplitter
    * Preserving Tables and Lists
    * Using a Custom Handler
    * Using a custom handler to analyze an image with an LLM


