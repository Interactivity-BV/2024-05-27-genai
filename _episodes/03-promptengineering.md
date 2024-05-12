---
title: "Prompt engineering using the ChatGPT web interface"
teaching: 15
exercises: 30
questions:
- "What is the difference between 'googling' and prompting?"
- "What is prompt engineering?"
- "How do prompts influence LLM outputs?"
- "How to customize prompts for different applications?" 
- "How does ChatGPT handle privacy, data security and sensitive / illegal topics?"
objectives:
- "Understanding the workings of a LLM prompt"
- "Learning effective prompting strategies"
- "Knowing how to produce different types of output, such as Python code, flow diagrams and JSON files"
keypoints:
- "Effective prompt engineering"
- "Does and don'ts of prompt engineer with ChatGPT"
- "Practical use of the ChatGPT prompt"
---

## Prompt engineering using the ChatGPT web interface

During this hands-on session we will dive further into the workings of LLMs by designing prompt and see what strategies are effective.

> ## Prompt Crafting Exercise
> Historically, we've relied on search engines like Google to find relevant websites for our queries. Now, let's explore how asking a similar question differs when using a Large Language Model (LLM) like ChatGPT.
> 
> **Step 1:** Use Google to search for information on the following topic:
> ~~~
> Cell membrane function
> ~~~
> {: .source}
> 
> **Step 2:** After reviewing the top results from Google, ask ChatGPT the same question.
> > ## ChatGPT response
> > 
> > The cell membrane, also known as the plasma membrane, plays a critical role in maintaining the integrity and functionality of cells. Here are some of its key functions:
> > 
> > 1. **Barrier and Protection**: The cell membrane acts as a selective barrier that regulates the movement of substances in and out of the cell. It protects the internal contents of the cell from the external environment and maintains the cellular environment.
> > 
> > 2. **Selective Permeability**: It allows selective passage of materials such as ions, nutrients, and wastes. This selective permeability is crucial for maintaining an appropriate balance of substances within the cell, which is essential for normal cellular function.
> > 
> > 3. **Communication**: The membrane contains receptors that can recognize and respond to chemical signals from other cells or the environment. This allows cells to communicate with each other and adapt to changes in their surroundings.
> > 
> > 4. **Adhesion and Structure**: Cell membranes also help cells maintain their shape and adhere to other cells and the extracellular matrix, forming tissues and fulfilling their roles in the organism.
> > 
> > 5. **Energy Transfer and Metabolism**: Certain cell membranes, especially in eukaryotic cells, are involved in energy transfer and the metabolic processes necessary for life. For example, the membranes of mitochondria in animal cells and chloroplasts in plant cells are crucial for energy production.
> > 
6. **Endocytosis and Exocytosis**: The cell membrane can engulf substances from the outside via endocytosis, forming vesicles that bring substances into the cell. Similarly, it can expel substances in vesicles via exocytosis, which is essential for processes like hormone secretion and the removal of waste.
> > 
> > Overall, the cell membrane is vital for the survival and proper functioning of the cell, helping to coordinate multiple processes that sustain life.
> {: .solution}
> > ## Discussion
> > Discuss and write down the key differences between the output by Google and the output produced by ChatGPT.
> {: .discussion}
> **Step 3:** Now let's try to be a little more specific:
> ~~~
> cell membrane function disease resistance
> ~~~
> {: .source}
> > ## ChatGPT response
> > The cell membrane plays a crucial role in disease resistance across various organisms, from plants to humans. It functions as the first line of defense against pathogens such as viruses, bacteria, and parasites. Here’s how the cell membrane contributes to disease resistance:
> > 
> > 1. **Barrier Function**: As a physical barrier, the cell membrane prevents the entry of pathogens into the cell. Its structure can be selectively permeable, allowing only specific substances to pass through and keeping potentially harmful agents out.
> > 
> > 2. **Immune Signaling**: Cell membranes contain various receptors that recognize pathogenic components or stress signals. For instance, in human immune cells, these receptors can detect foreign molecules typical of bacteria and viruses, triggering an immune response.
> > 
> > 3. **Transport and Secretion of Immune Molecules**: The cell membrane is involved in the transport and secretion of molecules critical for immune responses, such as antibodies, cytokines, and chemokines. These molecules help in coordinating a systemic response against pathogens.
> > 
> > 4. **Endocytosis and Phagocytosis**: Cells can ingest and destroy pathogens through processes like endocytosis and phagocytosis, which are mediated by the cell membrane. This involves the membrane engulfing the pathogen, which is then digested by enzymes within lysosomes.
> > 
> > 5. **Membrane Fluidity and Repair**: The ability of the cell membrane to maintain its integrity and repair itself is vital for resisting infection. Changes in membrane fluidity can affect the entry of viruses and other pathogens. Cells can often alter their membrane composition to become less permeable and more resistant to attacks.
> > 
> > 6. **Activation of Pathogen-Destroying Enzymes**: Some cells, particularly plant cells, have adaptations where their membranes activate enzymes that lead to the destruction of the pathogen or the signaling of other cells to initiate a broader immune response.
> > 
> > In diseases where the cell membrane function is compromised, such as in certain genetic disorders or due to environmental stress, the effectiveness of these defense mechanisms can be significantly reduced, increasing susceptibility to infections. For example, in cystic fibrosis, a defect in the CFTR channel (a protein in the cell membrane) affects the fluid balance in the lungs, making them more susceptible to bacterial infections. This highlights the importance of the cell membrane not just in normal cellular functions but also in protecting against diseases.
> {: .solution}
> > ## Discussion
> > Do you see the same key differences as before? 
> {: .discussion}
> **step 4:** Asking a question
> 
> LLMs are much better in providing answers to fully constructed sentences. When generating the answer, the question will provide context for the model and provide a much better and to the point answer. Try out for example:
> ~~~
> How does the cell membrane function contribute to a cell's resistance to pathogens, and what are the implications for treating diseases related to compromised cellular barriers?
> ~~~
> {: .source}
> > ## ChatGPT answer
> > 
> >The cell membrane's functions are fundamental in providing resistance to pathogens. Understanding these functions can enhance our approaches to treating diseases where cellular barriers are compromised. Here’s how the cell membrane contributes to pathogen resistance and the potential implications for treatment:
> >
> > ### 1. **Pathogen Recognition and Immune Activation**
> >   - **Mechanism**: The cell membrane contains specialized receptors that recognize pathogen-associated molecular patterns (PAMPs), such as lipopolysaccharides on bacteria and glycoproteins on viruses. This recognition triggers immune responses, including the activation of signaling pathways that result in the production of cytokines and chemokines, mobilizing an immune response.
> >   - **Implications**: Enhancing these receptor functions through genetic engineering or pharmacological agents could boost the immune system's ability to detect pathogens early, potentially preventing infections or reducing their severity.
> >
> > ### 2. **Physical Barrier and Controlled Permeability**
> >   - **Mechanism**: The lipid bilayer of the cell membrane acts as a selective barrier, preventing the entry of harmful substances and pathogens while allowing the influx of nutrients and efflux of waste products. Tight junctions between cells further reinforce this barrier, particularly in epithelial layers like those in the gut and respiratory tracts.
> >   - **Implications**: Strengthening these barriers, possibly through nutrients that enhance membrane integrity or drugs that reinforce tight junctions, could be beneficial in treating diseases like inflammatory bowel disease (IBD) where barrier function is compromised.
> >
> > ### 3. **Endocytosis and Phagocytosis**
> >   - **Mechanism**: The cell membrane can engulf pathogens through endocytosis and phagocytosis, effectively isolating and destroying them within vesicles that fuse with lysosomes. This is particularly important in immune cells like macrophages and neutrophils.
> >   - **Implications**: Therapies that enhance the phagocytic capabilities of immune cells could improve outcomes in diseases where pathogen clearance is impaired. For example, in sepsis or chronic infections, boosting the endocytic and phagocytic activities could clear pathogens more effectively.
> >
> > ### 4. **Signal Transduction**
> >   - **Mechanism**: Cell membranes facilitate the transduction of signals critical for activating immune defenses, orchestrating responses that involve multiple cell types across different tissues.
> >   - **Implications**: Drugs that modulate signaling pathways can be used to regulate immune responses, potentially enhancing resistance to pathogens or dampening excessive inflammation seen in autoimmune diseases.
> >
> > ### 5. **Membrane Fluidity and Repair**
> >   - **Mechanism**: Membrane fluidity affects how easily pathogens can penetrate cells. Cells can modify their membrane composition in response to threats, increasing their resistance to penetration.
> >   - **Implications**: Treatments that influence membrane fluidity could potentially prevent viral entry. For example, modifying cholesterol content in membranes can affect the ability of viruses like HIV and influenza to fuse with host cells.
> >
> > ### 6. **Exocytosis of Antimicrobial Peptides**
> >   - **Mechanism**: Cells can secrete antimicrobial peptides and other defensive molecules through exocytosis, targeting and neutralizing pathogens externally before they breach the cell membrane.
> >   - **Implications**: Enhancing the secretion of antimicrobial peptides could serve as a strategy to prevent infections, especially in at-risk patient populations such as those with compromised immune systems.
> >
> > By focusing on these cellular mechanisms, research and clinical strategies can be developed to either bolster these natural defenses or correct the underlying defects that compromise them in disease states. This could lead to targeted therapies that are more effective and have fewer side effects compared to broader approaches like antibiotics and anti-inflammatory drugs.  
> {: .solution}
> > ## Discussion
> > What are you observations about the answer provided by ChatGPT? What should you take into consideration with such detail answers?
> {: .discussion}
{: .challenge}

> ## LLM as an assistant
> What usually works best is to view an LLM as human assistant and write the prompt as if you are addressing a co-worker: **Could you elaborate on ...**, or **I have written the following lines of Python code. It should connect to the API, but I'm getting an `internal server error`: `my code`**. It takes a bit getting used to, but it will increase the quality and appropriateness of the answer for sure.
{: .callout}  

> ## Elaborating, discussing and specifying output
> With this exercise we will look further into the **Chat** in **ChatGPT**, asking it to elaborate, be more specific and help with producing output such as Mermaid format and JSON
{: .challenge}

