---
title: "The OpenAI API and data analytics"
teaching: 30
exercises: 60
questions:
- "What is Retrieval-Augmented Generation (RAG)?"
- "How do I setup an assistant?"
- "What are the differences between RAG and finetuning/retraining?"
- "How can OpenAI assist with doing data analytics?"
objectives:
- "Building an assistant for RAG using the API"
keypoints:
- "Retrieval-Augmented Generation"
- "Creating an vector store of your data"
- "Reasoning about your data"
- "OpenAI API is constantly under development. Keep an eye on this!"
---

## The OpenAI API and data analytics

OpenAI offers several more functionalities which build upon the LLMs. We will have a closer look a very powerful and widely used application: Retrieval-Augmented Generation (RAG).

### Retrieval-augmented generation

Retrieval-Augmented Generation (RAG) is a technique used in natural language processing that enhances the capabilities of generative models by combining them with retrieval mechanisms. This approach integrates information retrieved from a large knowledge base or data set into the generation process. Here's how it typically works:

1. **Retrieval Phase**: When a query or prompt is received, the model first performs a search to retrieve relevant documents or data snippets from a structured database or a large corpus of text. This retrieval is based on the similarity of the content to the input query, ensuring that the information is pertinent to the task at hand.

2. **Augmentation**: The retrieved content is then used to augment the input to the generative model. This augmentation can provide additional context, facts, or examples that are not inherently known by the model but are useful for generating accurate and contextually relevant responses.

3. **Generation Phase**: Equipped with both the original input and the retrieved information, the generative model (often a transformer-based model) synthesizes this information to produce a coherent and contextually enriched output. This output aims to reflect both the direct query and the supplementary information obtained through retrieval.

The key advantage of RAG is its ability to leverage vast amounts of information not stored within the model's parameters, allowing it to generate more informed, accurate, and contextually rich outputs. This is particularly useful in applications requiring factual correctness and depth, such as question answering, content creation, and summarization tasks.

### Python notebook

The Python notebook related to this lesson can be found in the files directory: <a href="{{site.workshop_site}}files/openai_rag.ipynb">OpenAI RAG Notebook</a>.


> ## Retrieval-Augmented Generation: la chef
> In this exercise we will be using a <a href="{{site.workshop_site}}files/Recipes_Bejo.pdf">pdf version</a> of the <a href="https://www.bejo.com/recipes-specialties">Bejo recipes specialties</a> page.
> We will create a chef which can provide all sorts of nice recipes, taken from the pdf.
> 
> Retrieval-Augmented Generation is available through the <a href="https://platform.openai.com/docs/assistants/overview">Assistant</a> technology of OpenAI. First, have a look at the <a href="https://platform.openai.com/docs/assistants/overview">documentation</a>. Read about the workflow and how this differs from the chat completion we used during the previous session.
>
> > ## Discussion
> > Add your findings to the shared document.
> {: .discussion} 
> > ## The assistant API
> > There are three main differences compared to the chat completion interface:
> > - You can add files, creating a knowledge-base for the assistant. This can be programming code, reports, CVS files, etc.
> > - There are several different types of tools available, including **file retrieval** and **code interpreter**
> > - The assistant has the ability to keep a conversation history (threads).
> {: .solution}
> Now let's create the La Chef assistant in Python and add the <a href="{{site.workshop_site}}files/Recipes_Bejo.pdf">pdf with the recipes</a>.
> You can either use streaming or not, that is up to you.
> 
> **Step 1** Create the assistant in python
> Provide the assistant with the proper instructions as well.
> > ## La Chef assistant
> > ~~~
> > client = OpenAI()
> >  
> > assistant = client.beta.assistants.create(
> >   name="La Chef",
> >   instructions="You are an expert cook. You have access to recipes of Bejo Zaden.",
> >   model="gpt-4o",
> >   tools=[{"type": "file_search"}],
> > )
> > ~~~
> > {: .python}
> {: .solution}
> **Step 2** Add the pdf to a vector store and connect it to the assistant
> > ## Vector store
> > ~~~
> > # Create a vector store caled "Bejo recipes"
> > vector_store = client.beta.vector_stores.create(name="Bejo recipes")
> >  
> > # Ready the files for upload to OpenAI
> > file_paths = ["Recipes_Bejo.pdf"]
> > file_streams = [open(path, "rb") for path in file_paths]
> >  
> > # Use the upload and poll SDK helper to upload the files, add them to the vector store,
> > # and poll the status of the file batch for completion.
> > file_batch = client.beta.vector_stores.file_batches.upload_and_poll(
> >   vector_store_id=vector_store.id, files=file_streams
> > )
> > 
> > # You can print the status and the file counts of the batch to see the result of this operation.
> > print(file_batch.status)
> > print(file_batch.file_counts)
> > 
> > assistant = client.beta.assistants.update(
> >   assistant_id=assistant.id,
> >   tool_resources={"file_search": {"vector_store_ids": [vector_store.id]}},
> > )
> > ~~~
> > {: .python}
> {: .solution}
> **Step 3** Setup the conversation
> Create the thread and the event handler (if you are using the streaming method).
> > ## Setting up the conversation
> > ~~~
> > # Create a thread
> > thread = client.beta.threads.create()
> > 
> > class EventHandler(AssistantEventHandler):
> >     @override
> >     def on_text_created(self, text) -> None:
> >         print(f"\nassistant > ", end="", flush=True)
> > 
> >     @override
> >     def on_tool_call_created(self, tool_call):
> >         print(f"\nassistant > {tool_call.type}\n", flush=True)
> > 
> >     @override
> >     def on_message_done(self, message) -> None:
> >         # print a citation to the file searched
> >         message_content = message.content[0].text
> >         annotations = message_content.annotations
> >         citations = []
> >         for index, annotation in enumerate(annotations):
> >             message_content.value = message_content.value.replace(
> >                 annotation.text, f"[{index}]"
> >             )
> >             if file_citation := getattr(annotation, "file_citation", None):
> >                 cited_file = client.files.retrieve(file_citation.file_id)
> >                 citations.append(f"[{index}] {cited_file.filename}")
> > 
> >         print(message_content.value, flush=True)
> >         print("\n".join(citations), flush=True)
> > ~~~
> > {: .python}
> {: .solution}
> **Step 4** The conversation with La Chef
> Now have a conversation with assistant on the recipes. Ask for example for full vegan options, or for recipes with specific ingredients. This might require some prompt engineering to get the desired output. 
> > ## First prompts
> > ~~~
> > with client.beta.threads.runs.stream(
> >     thread_id=thread.id,
> >     assistant_id=assistant.id,
> >     instructions="Please provide a recipe which includes some garlic and perhaps peppers.",
> >     event_handler=EventHandler(),
> > ) as stream:
> >     stream.until_done()
> >     
> > with client.beta.threads.runs.stream(
> >     thread_id=thread.id,
> >     assistant_id=assistant.id,
> >     instructions="I don't have a can of peeled tomatoes. Can you suggest a proper substitute?",
> >     event_handler=EventHandler(),
> > ) as stream:
> >     stream.until_done()
> > ~~~
> > {: .python}
> {: .solution}    
> > ## Prompting
> > Add the prompt you have used to the <a href="{{ page.collaborative_notes }}">collaborative document</a> and why it did or did not perform as envisioned.
> {: .discussion}
> ## Further fine-tune the instructions 
> You might have noticed that the assistant will fall back on general knowledge if the document does not contain an example recipe. Instruct the assistant to:
> 
> - Ignore any request that is not related to cooking or the recipes in the document
> - General cooking questions can be answered, but the assistant should stick to the supplied recipes.
> 
> > ## Discussion
> > Add the instruction(s) you have used to the <a href="{{ page.collaborative_notes }}">collaborative document</a> and why it did or did not perform as envisioned.
> {: .discussion}
{: .challenge}

> ## Multiple documents
> For this exercise we will be handling several scientific papers. We will create the **Research Assistant** and provide this assistant with the following three papers:
>
> - <a href="{{site.workshop_site}}files/crop_rotation_sugar_beet.pdf">Effects of crop rotation on sugar beet growth through improving soil physicochemical properties and microbiome</a>
> - <a href="{{site.workshop_site}}files/soil_microbiome_potato.pdf">Potato yield and quality are linked to cover crop and soil microbiome, respectively</a>
> - <a href="{{site.workshop_site}}files/microbial_pepper.pdf">Evolution of microbial community and the volatilome of fresh-cut chili pepper during storage under different temperature conditions: Correlation of microbiota and volatile organic compounds</a>
> 
> Follow the same steps as before, but now create a vector store with these three papers and assign them to the **Research Assistant**.
> 
> > ## The Research assistant
> > ~~~
> > assistant = client.beta.assistants.create(
> >   name="Research assistant",
> >   instructions="You are a research assistant on microbiome research, specialized in field crops.",
> >   model="gpt-4o",
> >   tools=[{"type": "file_search"}],
> > )
> > 
> > # Create a vector store caled "Research papers"
> > vector_store = client.beta.vector_stores.create(name="Research papers")
> >  
> > # Ready the files for upload to OpenAI
> > file_paths = ["crop_rotation_sugar_beet.pdf", "microbial_pepper.pdf", "soil_microbiome_potato.pdf"]
> > file_streams = [open(path, "rb") for path in file_paths]
> >  
> > # Use the upload and poll SDK helper to upload the files, add them to the vector store,
> > # and poll the status of the file batch for completion.
> > file_batch = client.beta.vector_stores.file_batches.upload_and_poll(
> >   vector_store_id=vector_store.id, files=file_streams
> > )
> >  
> > # You can print the status and the file counts of the batch to see the result of this operation.
> > print(file_batch.status)
> > print(file_batch.file_counts)
> > 
> > assistant = client.beta.assistants.update(
> >   assistant_id=assistant.id,
> >   tool_resources={"file_search": {"vector_store_ids": [vector_store.id]}},
> > )
> > 
> > # Create a thread
> > thread = client.beta.threads.create()
> > 
> > with client.beta.threads.runs.stream(
> >     thread_id=thread.id,
> >     assistant_id=assistant.id,
> >     instructions="Could you provide a list of the bacterial species identified in the papers?",
> >     event_handler=EventHandler(),
> > ) as stream:
> >     stream.until_done()
> > ~~~
> > {: .python}
> {: .solution}
> The assistant will provide answers including references to the documents used to find the necessary information. The **message** object as described in the API contains a quote: the snippet in the document which either matched the vector search or the full text search.
> Find the documentation on the message object and modify the event handler to add the quote to the response.
> > ## Quoting the sources
> > The message object <a href="https://platform.openai.com/docs/api-reference/messages/object">is defined here</a>.
> > ~~~
> > class EventHandlerContext(AssistantEventHandler):
> >     @override
> >     def on_text_created(self, text) -> None:
> >         print(f"\nassistant > ", end="", flush=True)
> > 
> >     @override
> >     def on_tool_call_created(self, tool_call):
> >         print(f"\nassistant > {tool_call.type}\n", flush=True)
> > 
> >     @override
> >     def on_message_done(self, message) -> None:
> >         # print a citation to the file searched
> >         message_content = message.content[0].text
> >         annotations = message_content.annotations
> >         citations = []
> >         for index, annotation in enumerate(annotations):
> >             message_content.value = message_content.value.replace(
> >                 annotation.text, f"[{index}]"
> >             )
> >             if file_citation := getattr(annotation, "file_citation", None):
> >                 cited_file = client.files.retrieve(file_citation.file_id)
> >                 quote = file_citation.quote
> >                 citations.append(f"[{index}] {cited_file.filename} '{quote}'")
> > 
> >         print(message_content.value, flush=True)
> >         print("\n".join(citations), flush=True)
> > ~~~
> > {: .python}
> {: .solution}
> > ## Evaluation of the result
> > What did the output look like? Add your observations to the <a href="{{ page.collaborative_notes }}">collaborative document</a>
> > The great thing about the OpenAI API is that it takes a lot the complex tasks away from the programmer. The downside is that you are completely dependent on their releases and functionalities. For complex and/or large data sets it might be beneficial to setup your local RAG vector store or use a framework like <a href="https://bionic-gpt.com/">BionicGPT</a>.
> {: .discussion}
{: .challenge}

> ## Linking it to the source
> RAG can be used over multiple documents. But you can, for example, also create a text file based on database entries. It could be that you have a database table with product descriptions and you would like to use that information for RAG.
> By using the annotation information from OpenAI you can connect the resulting references to the original data in the database and provide additional information to the user.
> 
> In the <a href="https://www.interactivity.nl/bricks">Store assistant</a> we show-case this:
> To demonstrate the practicality and versatility of this assistant technology, we've developed the LEGO™ Set Guide. This guide is designed to operate like an expert within a toy store, providing detailed information about the LEGO™ sets in stock. Our inventory holds a collection of over 700 modern LEGO™ sets. We've tasked ChatGPT with generating descriptions for each set, which are then accessible through the API. These descriptions include details from <a href="https://rebrickable.com/">Rebrickable</a>, and the guide thoughtfully provides links to this website for further information on each set.
> You can try-out <a href="https://www.interactivity.nl/bricks">the LEGO™ Set Guide</a> and engage with our guide to discover sets featuring exciting spaceships or delightful farm animals. Our Assistant is ready to help you explore the vast world of LEGO™! 
{: .callout}

