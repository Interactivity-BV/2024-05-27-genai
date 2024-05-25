---
title: "Langchain and large documents"
teaching: 30
exercises: 45
questions:
- "What is langchain and why should I use it?"
- "What benefits and down-sides of using langchain?"
- "What are typical use-cases for langchain + OpenAI?"
objectives:
- "Introduction to langchain"
- "Design prompts to process a structured document"
keypoints:
- "Processing large documents with langchain"
---

## Langchain and large documents

Langchain is a powerful tool designed to handle and process large documents effectively, especially when combined with the capabilities of the OpenAI API. This combination allows for advanced document analysis, summarization, and content generation.

<a href="{{site.workshop_site}}files/07_langchain.pdf">PDF of the presentation belonging to this session.</a>

#### Langchain Overview
Langchain is specifically tailored to manage large text documents, breaking them down into manageable chunks for efficient processing. It supports various operations such as:

- **Text Chunking**: Dividing large documents into smaller, coherent sections.
- **Context Management**: Maintaining context across different chunks to ensure coherent analysis and generation.
- **LLM support**: Langchain supports many other Large Language Models

#### Integration with OpenAI API
By integrating Langchain with the OpenAI API, you can leverage the powerful language models to perform sophisticated tasks on large documents, including:

- **Summarization**: Generating concise summaries of lengthy documents.
- **Question Answering**: Extracting relevant information based on specific queries.
- **Content Generation**: Producing detailed content or expanding on sections of a document.

#### Example Use Case
To process a large document using Langchain and the OpenAI API, you can follow these steps:

1. **Chunk the Document**: Use Langchain to divide the document into smaller sections.
2. **Process Chunks with OpenAI**: Send each chunk to the OpenAI API for analysis or generation.
3. **Combine Results**: Aggregate the results from the API to form a coherent output.

### Python notebook

The Python notebook related to this lesson can be found in the files directory: <a href="{{site.workshop_site}}files/langchain.ipynb">Langchain and large documents</a>.

> ## Processing a large, structured document
> 
> In this session we will apply the map-reduce chaining of langchain to summarize and discuss large documents. The main difference between RAG and map-reduce is that with RAG the prompt is used to identify relevant parts of the data and with map-reduce in the end the entire document has been provided to the LLM. 
> This makes map-reduce much more complete but also much more expensive and slow. So you need to determine if this completeness is warranted for your application.
>   
> We will be analyzing scientific papers, as they follow a specific format, making it relatively easy to process.
> 
> You need to install the following modules:
> ~~~
> pip install langchain langchain-community html2text tiktoken langchain-openai
> ~~~
> {: .bash}
> 
> Langchain needs to know the model we would like to use and how much data it can send to the LLM:
> ~~~
> MODEL ="gpt-4o"
> chunk = 10000 # amount of data send to LLM per mapping 
> ~~~
>
> And these are the modules we will be using:
> ~~~
> from langchain.document_transformers import Html2TextTransformer
> from langchain.document_loaders import PyPDFLoader
> from langchain.chains.llm import LLMChain
> from langchain.prompts import PromptTemplate
> from langchain.chains.combine_documents.stuff import StuffDocumentsChain
> from langchain.text_splitter import CharacterTextSplitter
> from langchain.chains import ReduceDocumentsChain, MapReduceDocumentsChain
> from langchain_openai import ChatOpenAI
> from langchain.chains.conversation.memory import ConversationBufferMemory
> from langchain.chains import ConversationChain
> ~~~
> {: .python}
> 
> **Read the PDF**
> 
> PDFs are built from printing, not so much for data processing. Extracting text from a PDF can be challenging. The same holds true from HTML pages.
> Some PDFs will therefore parse nicely, while others might create a mess. Given your set of documents, you might need to try out several PDF-readers and parsers to find the one that gives the best results. In this case we will be using PyPDFLoader.
>
> After reading the PDF we need to split it up in chunks. Picking the optimal chuck_size is tricky and depends on context length and LLM used. Setting it too low, however, will limit the reasoning capabilities of the LLM, because not enough context will then be provided.
> > ## Reading and chunking
> > ~~~
> > loader = PyPDFLoader("crop_rotation_sugar_beet.pdf")
> > docs = loader.load()
> > 
> > html2text = Html2TextTransformer()
> > docs = html2text.transform_documents(docs)
> > 
> > text_splitter = CharacterTextSplitter.from_tiktoken_encoder(
> >     chunk_size=chunk, chunk_overlap=0
> > )
> > split_docs = text_splitter.split_documents(docs)
> > ~~~
> > {: .python}
> {: .solution}
> 
> **The map-reduce chaining**
> 
> This is the most complex part: we need run the map-reduce approach on our document.
> Note: we will add a sleep() to the method to prevent too many calls to the API. It is possible to have a subscription to the OpenAI API with much higher limits, though.
> 
> Documentation: <a href="https://api.python.langchain.com/en/latest/chains/langchain.chains.combine_documents.map_reduce.MapReduceDocumentsChain.html#langchain.chains.combine_documents.map_reduce.MapReduceDocumentsChain">MapReduceDocumentsChain</a>
> 
> The map-reduce methods are reported as 'deprecated'. However, there is no a high level chain yet available. 
> 
> **Create the map and reduce templates**
>
> Both the map and the reduce phase require a template, where you add the instructions and have placeholders for the document chunks and the prompt.   
> 
> > ## Templates
> > ~~~
> > llm = ChatOpenAI(temperature=0.3, model_name=MODEL, streaming=True)
> > map_template = """The following is a set of documents which combined form a full scientific paper and should therefore be considered as one long, single paper.
> > {docs}
> > {question}
> > Helpful Answer:"""
> > 
> > reduce_template = """{description}
> > {doc_summaries}
> > {question}
> > Helpful Answer:"""
> > ~~~
> > {: .python}
> {: .solution}
> 
> **MapReduce method**
> 
> We need to define a method which will chain the map-reduce approach.
>
> > ## MapReduce method
> > ~~~
> > def runMapReduce(map_template, reduce_template, docs, llm, model = "gpt-4o"):
> >     map_prompt = PromptTemplate.from_template(map_template)
> >     map_chain = LLMChain(llm=llm, prompt=map_prompt )
> > 
> >     # Reduce
> >     reduce_prompt = PromptTemplate.from_template(reduce_template)
> >     reduce_chain = LLMChain(llm=llm, prompt=reduce_prompt)
> > 
> >     # Takes a list of documents, combines them into a single string, and passes this to an LLMChain
> >     combine_documents_chain = StuffDocumentsChain(
> >         llm_chain=reduce_chain, document_variable_name="doc_summaries"
> >     )
> > 
> >     #print("Reduce phase")
> >     # Combines and iteravely reduces the mapped documents
> >     reduce_documents_chain = ReduceDocumentsChain(
> >         # This is final chain that is called.
> >         combine_documents_chain=combine_documents_chain,
> >         # If documents exceed context for `StuffDocumentsChain`
> >         collapse_documents_chain=combine_documents_chain,
> >         # The maximum number of tokens to group documents into.
> >         #token_max=tokens,
> >     )
> > 
> >     #print("Mapping phase")
> >     # Combining documents by mapping a chain over them, then combining results
> >     map_reduce_chain = MapReduceDocumentsChain(
> >         # Map chain
> >         llm_chain=map_chain,
> >         # Reduce chain
> >         reduce_documents_chain=reduce_documents_chain,
> >         # The variable name in the llm_chain to put the documents in
> >         document_variable_name="docs",
> >         # Return the results of the map steps in the output
> >         return_intermediate_steps=False,
> >     )
> > 
> >     if model == "gpt-4o": # let's wait for a while
> >         time.sleep(10)
> > 
> >     return(map_reduce_chain.run(docs)) 
> > ~~~
> > {: .python}
> {: .solution}
> 
> **Process the paper**
> 
> Using the map-reduce method we can tell langchain to process the entire paper. Let's start with some basic information, and extract information on authors and journal.
> You need to provide information to the two templates and call `runMapReduce`.
> 
> > ## Basic information
> > ~~~
> > map_q = "Please identify the publisher of this scientific paper and the authors of this paper. Provide some background on the journal. Is it for example considered high impact? What are generally the topics and results shared in this journal? If you can not extract this information from this part of the text, just provide an empty string as answer."
> > reduce_qDescription = "The following contains an author list and information on the journal from a scientific paper:"
> > reduce_q = "Take these and provide only the author list and information on the first mentioned journal and publisher. The output needs to be in Markdown file format."
> > result = runMapReduce(map_template.format(question=map_q, docs="{docs}"), reduce_template.format(description=reduce_qDescription, question = reduce_q, doc_summaries="{doc_summaries}"), split_docs, llm=llm, model=MODEL)
> > print(result)
> > ~~~
> > {: .python}
> {: .solution}
> - summarize sections
> - reason about content
> - go beyond the content
{: .challenge}
 


