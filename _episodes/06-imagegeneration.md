---
title: "Image generation and processing through the API"
teaching: 15
exercises: 30
questions:
- "How can you analyze and generate images through the API?"
objectives:
- "Image analyzes through the API"
- "Generating images based on data"
keypoints:
- "Automated processing of visual information"
---

## Image generation and processing through the API

OpenAI provides powerful tools for image analysis and generation through its API, enabling developers to integrate advanced AI capabilities into their applications. Here’s a brief overview:

### Image Analysis
OpenAI's models can analyze images to extract meaningful information, perform object recognition, and even describe the contents of an image in natural language. This functionality can be used in various applications such as:

- **Object Detection**: Identifying and classifying objects within an image.
- **Image Captioning**: Generating descriptive captions for images.
- **Content Moderation**: Detecting inappropriate or sensitive content in images.

### Image Generation
The API also supports image generation, allowing users to create new images from textual descriptions or modify existing images. This can be used for:

- **Creative Content Creation**: Generating art, illustrations, or design elements based on specific prompts.
- **Image Editing**: Modifying parts of an image according to given instructions.
- **Prototyping**: Quickly visualizing concepts and ideas without the need for manual drawing or design.

### Python notebook

The Python notebook related to this lesson can be found in the files directory: <a href="{{site.workshop_site}}files/image_analysis.ipynb">Generating and analyzing images through the API</a>.


> ## Image generation
> In this exercise will be generating several different types of images.
> **Step 1** Image based on a recipe
> Use the La Chef Assistant to provide a recipe and then ask DALL-E 3 to create an image based on this recipe. You need to change the event handler for this: in stead of printing the message, you need to store it. Place the resulting image in the <a href="{{ page.collaborative_notes }}">collaborative document</a>.
> > ## Recipe image
> > ~~~
> > class EventHandler(AssistantEventHandler):
> >     @override
> >     def on_text_created(self, text) -> None:
> >         self.message = ""
> >         print(f"\nassistant running ", end="", flush=True)
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
> >         self.message += message_content.value + "\n"
> >         self.message += "\n".join(citations)
> > event_handler = EventHandler()
> > with client.beta.threads.runs.stream(
> >     thread_id=thread.id,
> >     assistant_id=assistant.id,
> >     instructions="Please provide a recipe which includes some garlic and perhaps peppers.",
> >     event_handler=event_handler,
> > ) as stream:
> >     stream.until_done()
> > 
> > print(event_handler.message)
> > response = client.images.generate(
> >   model="dall-e-3",
> >   prompt="Please provide a photo-realistic plate of food, given the following description and ingredients: `{event_handler.message}`",
> >   size="1024x1024",
> >   quality="standard",
> >   n=1,
> > )
> > 
> > image_url = response.data[0].url
> > display(HTML(f'<img src="{image_url}" alt="Image" />'))
> > ~~~
> >{: .python}
> {: .solution}
> ## Image generation of a plant
> For a presentation on drought stress in tomato plants we would like to have a cartoon-like image of such an unhappy plant. Can you generate one?
> > ## Some example prompts
> > ~~~
> > response = client.images.generate(
> >   model="dall-e-3",
> >   prompt="Please provide a cartoon-like image of an unhappy tomato plant due to drought. The tomato is growing in between other, more happy, green tomato plants.",
> >   size="1024x1024",
> >   quality="standard",
> >   n=1,
> > )
> > 
> > image_url = response.data[0].url
> > display(HTML(f'<img src="{image_url}" alt="Image" />'))
> > 
> > response = client.images.generate(
> >   model="dall-e-3",
> >   prompt="Please provide a cartoon-like image of tomato plant with clear signs of drought.",
> >   size="1024x1024",
> >   quality="standard",
> >   n=1,
> > )
> > 
> > image_url = response.data[0].url
> > display(HTML(f'<img src="{image_url}" alt="Image" />'))
> > ~~~
> >{: .python}
> {: .solution}
> > ## Discussion on the resulting images
> > Please add the generated images to the document. How do they look? Any other comments you would like to add?
> {: .discussion}
> 
{: .challenge}

> ## Image analysis
> Next generating images, you can also ask GPT-4 and GPT-4o to provide a description of an image (image-to-text). OpenAi calls this Vision.
> Through the API you can provide the image(s) and a prompt. Can you ask GTP-4o to describe this image: <a href="https://visuals.rijkzwaan.com/m/15a977f5cedde13b/original/XX-Hero-L-VI24-Solutions-Industry.webp">https://visuals.rijkzwaan.com/m/15a977f5cedde13b/original/XX-Hero-L-VI24-Solutions-Industry.webp</a> ?
> > ## Getting the description
> > ~~~
> > response = client.chat.completions.create(
> >   model="gpt-4o",
> >   messages=[
> >     {
> >       "role": "user",
> >       "content": [
> >         {"type": "text", "text": "What’s in this image?"},
> >         {
> >           "type": "image_url",
> >           "image_url": {
> >             "url": "https://visuals.rijkzwaan.com/m/15a977f5cedde13b/original/XX-Hero-L-VI24-Solutions-Industry.webp",
> >           },
> >         },
> >       ],
> >     }
> >   ],
> >   max_tokens=300,
> > )
> > 
> > print(response.choices[0])
> > ~~~
> > {: .python}
> {: .solution}
{: .challenge} 