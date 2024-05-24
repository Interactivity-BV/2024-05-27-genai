---
title: "The OpenAI API"
teaching: 30
exercises: 60
questions:
- "What is the OpenAI API?"
- "What are the technical requirements to use the API"
- "Are there costs involved in using the API?"
objectives:
- "Introduction to the OpenAI API"
- "Connecting to and using the OpenAI API in a Python notebook"
keypoints:
- "Python notebook integration of the OpenAI API"
---

## The OpenAI API

During this session we will learn how to connect to the <a href="https://platform.openai.com/docs/introduction">OpenAI API</a>. This API provides much more flexibility in using, for example, ChatGPT. By using the API you can integrate LLMs in your own applications, automate interactions, create assistants, etc. 

Access to the API requires a key, which will be provided before the start of the session and will only work during the workshop. Usage of the API is charged separately from your ChatGPT account and you pay per token, both sent and received. 

You can use the Python notebook provided here with the basic Python code required to do the exercises, but you can also use this lesson to write the code step by step in an empty notebook or script. This way you can use your favorite IDE or text editor. VSCode by Microsoft has a plugin to run notebooks within the IDE. VSCode is available for many platforms, not only Windows. Google provides <a href="https://colab.research.google.com">free notebooks</a> as well.

<a href="{{site.workshop_site}}files/04_openai_api.pdf">PDF of the presentation belonging to this session.</a>

The OpenAI API provides access to advanced artificial intelligence models developed by OpenAI, including the GPT (Generative Pre-trained Transformer) series and other specialized models. This API allows developers to integrate state-of-the-art natural language processing capabilities into their applications and services. Here’s a brief overview of the key features and functionalities of the OpenAI API:

1. **Advanced Language Models**: The API offers access to powerful language models like GPT-3 and its successors, which are capable of understanding and generating human-like text. These models are pre-trained on diverse internet text and fine-tuned for various tasks, providing robust performance across a wide range of NLP applications.

2. **Versatility in Applications**: The API is designed to handle a variety of text-based tasks including but not limited to conversation simulation, content generation, summarization, translation, and more. This versatility makes it ideal for creating chatbots, enhancing customer support, automating content creation, and other business processes.

3. **Ease of Integration**: OpenAI provides a simple and straightforward API that can be integrated with minimal effort into existing systems and applications. The API supports various programming languages, and OpenAI offers detailed documentation and support to facilitate integration and usage.

4. **Scalability and Performance**: The API is built to scale, supporting requests from small to large volumes without compromising performance. This scalability ensures that applications using the API can handle varying levels of user demand.

5. **Continuous Improvements and Updates**: OpenAI continuously updates their models and API based on the latest research and user feedback, ensuring that users have access to the most advanced AI capabilities.

6. **Ethical Considerations and Usage Guidelines**: OpenAI places a strong emphasis on the ethical use of AI technologies. Users of the API are expected to adhere to guidelines that promote responsible and safe use, particularly when generating content that influences public opinion or decision-making.

### Python notebook

The Python notebook related to this lesson can be found in the files directory: <a href="{{site.workshop_site}}/files/openai_api.ipynb">OpenAI API Notebook</a>.

> ## Installing and connecting
> To make use of OpenAI, you need to install the OpenAI Python module
> ~~~ 
> pip install openai
> ~~~
> {: .bash}
> After this, you can use the module in python and start using the API. In Python, import the module and set the API key to a variable.
> > ## Using OpenAI API
> > ~~~
> > import openai
> > OPENAI_KEY = 'insert the key here'
> > ~~~
> > {: .python}
> {: .solution}
{: .challenge}

> ## Security advisory
> The API key is directly linked to your account and, more importantly, to your credit card. So there are some security issues you need to take into account:
>
> * Never add a key to your code which you will push to a git repository. Once it is in there, it is very difficult to remove, due to the nature of the version control software. Also, public repositories are actively scanned by hackers for passwords, keys, etc.
> * Create separate keys for each of your projects. This way you can not only track usage much better (and detect potential misuse), but in the event your key has been exposed or hacked, you only need to update that single project.
> * Read keys from the environment or from a (secrets) file. In cloud infrastructure you can set these environment variables outside your code, function or VM, for example.
> * By using the API you are providing access to advanced models for which you normally have to pay when using the ChatGPT web interface. This means that users might try to use your (web) interface to access these models for free.   
>
{: .callout}   

> ## Writing your first prompt
> OpenAI has several options available to use for your LLM application. Generally speaking, newer models work better than older ones but are usually also more expensive. Check the documentation for the most recent models and costs. For now we can select:
> ~~~
> MODELS = ["gpt-3.5-turbo-1106", "gpt-4", "gpt-4o", "gpt-3.5-turbo-16k"]
> MODEL = MODELS[0] 
> ~~~
> {: .python}
> We are now set to send our first prompt through the API. Check the manual of OpenAI how this is done (recommended) and write the code in a notebook. After this, ask ChatGPT to provide this code for you. Add your prompt and the result to the <a href="{{ page.collaborative_notes }}">collaborative document</a> so we can evaluate this with the group.
> > ## First prompt
> > ~~~
> > import openai
> > 
> > openai.api_key = OPENAI_KEY
> > MODELS = ["gpt-3.5-turbo-1106", "gpt-4", "gpt-3.5-turbo-16k"]
> > MODEL = MODELS[0]
> > 
> > def query_openai(prompt, model):
> >     try:
> >         response = openai.Completion.create(
> >             engine=model,
> >             prompt=prompt,
> >             max_tokens=150
> >         )
> >         return response.choices[0].text.strip()
> >     except Exception as e:
> >         print(f"An error occurred: {e}")
> >         return None
> > 
> > # Example usage
> > prompt_text = "Provide some Perl code to write `Hello world` in five different ways."
> > response_text = query_openai(prompt_text, MODEL)
> > print("Response from OpenAI:", response_text)
> > ~~~
> > {: .python}
> {: .solution}
> > ## Comparing your code to that of ChatGPT
> > Do you see any significant differences between your code and that of ChatGPT? Are these mostly cosmetic or is the code structured differently? If your neighbor used a different version of ChatGPT, did it also produce different results? And, does it work?
> {: .discussion} 
{: .challenge}

> ## Processing data through the API
> Now we have access to the API we can start sending data to OpenAI. The basic feature of the API is to send prompts, creating a programming interface similar to the ChatGPT web interface. And although it can be very handy if you'd like to integrate a chat-like function to your application.
> More importantly, we can send all sorts of information to OpenAI, such as files, CSVs and JSONs. Later in the session we will have a look at Retrieval Augmented Generation (RAG), for now we will focus on using data from a live source.
> 
> As you might have noticed, the code provided by ChatGPT was outdated. This can happen when information on the internet, such as API documentation, has changed after training of the LLM. It can get even worse, when there is very limited information available on an API: the LLM will start hallucinating API calls.
> > ## Proper API call
> > ~~~
> > def query_openai(client, prompt, model):
> >     response = client.chat.completions.create(
> >         model=model,
> >             messages=[{
> >                 "role": "system",
> >                 "content": "You will be asked to help with programming questions."
> >             },
> >             {
> >                 "role": "user",
> >                 "content": prompt
> >             }],
> >         max_tokens=256
> >         )
> >     return(response.choices[0].message.content)
> > ~~~
> > {: .python}
> {: .solution}
> > ## The 'messages' object
> > Could you explain the structure and the meaning of the `messages` object? This is not something you normally see when using the ChatGPT interface.
> {: .discussion}
>
> We will query the Gene Ontology API for information on GO terms, for example GO:0030445. Using the API, ask the LLM to produce code to query the GO API.
> > ## Quering the GO API
> > ~~~
> > prompt_text = "I need to query the gene ontology database through the API. Can you provide code which gives information on GO:0030445?"
> > response_text = query_openai(client, prompt_text, MODEL)
> > print(response_text)
> > ~~~
> > {: .python}
> {: .solution}
> 
> The LLM has given you a way to connect the GO API. Please share the output in the Google doc. 
> To be able to query any GO term, we have to wrap the code in a function.
> > ## GO API function
> > ~~~
> > import requests
> > 
> > def get_go(GOterm):
> >     # Make a GET request to the gene ontology API
> >     response = requests.get("http://www.ebi.ac.uk/QuickGO/services/ontology/go/terms/{}".format(GOterm))
> > 
> >     # Check if the request was successful
> >     if response.status_code == 200:
> >         # Convert the response to JSON
> >         data = response.json()
> >         return data
> >     else:
> >         print("Failed to get information for {}".format(GOterm))
> > ~~~
> > {: .python}
> {: .solution}
> 
> And we have to make a more generalized version of the OpenAI API function. Could you add a variable to the function definition so you can provide system prompts?
> > ## Generalized OpenAI function
> > ~~~
> > def query_openai(client, system, prompt, model):
> >     response = client.chat.completions.create(
> >         model=model,
> >             messages=[{
> >                 "role": "system",
> >                 "content": system
> >             },
> >             {
> >                 "role": "user",
> >                 "content": prompt
> >             }],
> >         max_tokens=256
> >         )
> >     return(response.choices[0].message.content)
> > ~~~
> > {: .python}
> {: .solution}
> Using both functions we can now ask the LLM to provide a summary of the data in the JSON. This might require some prompt engineering to get the results you like.
> Please share the prompts in our <a href="{{ page.collaborative_notes }}">collaborative document</a> and add a small explanation of why you are (un)happy with the prompt(s).
> > ## Summarizing data
> > ~~~
> > my_go_term = "GO:0030445"
> > system_prompt = "You are a skilled biologist and a good lecturer."
> > 
> > go_json = get_go(my_go_term)
> > summary = query_openai(client=client, 
> >                        system=system_prompt, 
> >                        prompt="I have this json from the Gene Ontology database. Could you create a nice summary of the biological data? There is no need to comment on the structure of the JSON`{}`".format(go_json), 
> >                        model=MODEL)
> > print(summary)
> > ~~~
> > {: .python}
> {: .solution}
> The LLM can provide nice summaries and many other types of analyzes of data from many different source, being it the GO database, a weather API or any other online source or file. 
> But why stop there? The LLM can provide addition information as well. Using the OpenAI prompt, ask the LLM to produce a report in markdown format on the data in the summary.
> > ## Markdown report
> > ~~~
> > report = query_openai(client=client,
> >                       system=system_prompt,
> >                       prompt="Given the summary of this GO term, could you provide a markdown report on this GO term with more background information? Please only markdown, no other comments or explainations. `{}`".format(summary),
> >                       model=MODEL)
> > print(report)
> > ~~~
> > {: .python}
> > ~~~
> > # GO Term Report: GO:0030445
> > 
> > ## Background Information
> > The GO term GO:0030445 represents the yeast-form cell wall, which is a part of the Gene Ontology database under the "cellular_component" aspect. It is the wall surrounding a cell of a dimorphic fungus growing in the single-cell budding yeast form, in contrast to the filamentous or hyphal form. This structure is not obsolete and is essential for the functioning and survival of the cell.
> > 
> > ## Related Terms
> > The comment associated with this GO term suggests consulting the Fungal Anatomy Ontology term 'vegetative cell ; FAO:0000032' for further information. This indicates that there is likely additional related information in the Fungal Anatomy Ontology that can provide a more comprehensive understanding of the yeast-form cell wall and its context within the fungal organism.
> > 
> > ## Usage
> > The usage of the information related to GO:0030445 is unrestricted, meaning that it can be freely utilized for research, analysis, and any other scientific purposes.
> > ~~~
> > {: .code}
> {: .solution}
{: .challenge}