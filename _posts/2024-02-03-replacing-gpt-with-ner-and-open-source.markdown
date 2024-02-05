---
layout: post
title: Optimizing AI Pipelines' Cost & Runtime (How GPT Could Kill Your Product)
date: 2024-02-03 00:00:00 +0300
description: If you are utilizing one of the AI tools available online today such as GPT, this blog post is for you! Your AI pipeline could be costing you much more than it should and the user-experience in terms of runtime could also be hurting your product. Join me in my journey to optimize one of my client's AI pipeline to save them money and runtime.
img: optime-ai-pipelines.jpg
fig-caption: # Add figcaption (optional)
tags: [Applied Data Science, AI Pipelines, AI Costs, AI Runtime, Scalable AI Products]
---
# Optimizing AI Pipelines: From POC to Real-World Product

In the realm of AI-driven solutions, leveraging powerful language models like GPT-4 has become a cornerstone for rapid Proof of Concept (POC) development. During my work at [Webiks](https://webiks.com/), we worked with a client that initially embraced this approach, incorporating GPT-4 API calls within their pipeline to tackle a complex task characterized by intricate and substantial prompts.

The challenge at hand involved processing vast strings for identifying matching entities within a predefined database, with the added nuance of considering contextual information for enhanced accuracy. While the GPT-4 API provided a robust foundation, the monthly cost estimations soared to a staggering $100,000 due to the sheer complexity of the task and the scale at which it operated. Moreover, taking into account the time it takes to send a request and wait for respond, adding up the time required for cooldowns between requests, it appears to be a very slow and not scalable solution for this task.

## Evolution of POC Development with LLM

In this technical exploration, we delve into the journey of reverse engineering this AI pipeline, driven by the imperative to optimize both speed and cost. We will unveil how, in a landscape rich with Language Model (LLM) capabilities, the ease of building POCs has evolved, particularly with the accessibility of GPT-4 API calls. Join us as we dissect the intricacies of disassembling, reevaluating, and ultimately reconstructing the pipeline to harness faster and more cost-effective AI models.

<div style="text-align:center;">
  <img src="../assets/img/Ciclo-Lean-Startup.png" alt="lean startup cycle" width="600"/>
  <p style="color:grey;">Lean Statrup Evolution cycle</p>
</div>

## Anatomy of the Client's AI Pipeline

The client's pipeline was predominantly composed of a two-step process. Initially, a straightforward and somewhat naive search using regular expressions identified potential matches across the database. However, the real complexity unfolded in the subsequent step, where the original string and all potential matches were amalgamated into a predefined prompt. This extensive prompt was then forwarded to GPT-4 for processing.

The first part, the regular expression search, posed minimal challenges. The real operational intricacies surfaced in the second part, where the prompt's size would often balloon due to numerous potential matches and their accompanying context. The initial length of the prompt was already considerable, primarily to convey to GPT-4 the desired task.

In our journey to optimize this pipeline, addressing the explosion in prompt size and the subsequent cost implications became focal points of our reverse engineering efforts.

## Unraveling the Matching Problem: From Exact Matches to Entity Recognition

The initial phase of our optimization journey involved dissecting the nature of the matching problem our client grappled with. Contrary to traditional exact or partial match problems, our client faced the formidable challenge of extracting specific entities from a lengthy and diverse string. This distinctive task drove them to employ GPT-4 initially.

To comprehend the underlying issues, I began by establishing a baseline using a straightforward comparison between the original string and the candidate names, assessing performance in terms of precision and recall. This analysis exposed deficiencies, revealing the inherent difficulty of rigorously solving a problem involving matching given names within a broad context.

Recognizing the nuanced nature of the problem, I pivoted towards an [entity recognition](https://en.wikipedia.org/wiki/Named-entity_recognition) approach. Leveraging the [fuzzywuzzy](https://pypi.org/project/fuzzywuzzy/) library, I initiated a process of extracting entities from the original string and comparing them to candidate names using fuzzy matching. This strategic shift significantly improved our baseline, bringing precision and recall closer to the levels achieved by GPT-4.

Here is an example code for the mentioned pipeline:

```python
# Entity recognition with fuzzy matching
from fuzzywuzzy import fuzz
from transformers import AutoTokenizer, AutoModelForTokenClassification
from transformers import pipeline
import re

# Regular expression search for potential matches
candidates = re.findall(pattern, input_string)

tokenizer = AutoTokenizer.from_pretrained("dslim/bert-base-NER")
model = AutoModelForTokenClassification.from_pretrained("dslim/bert-base-NER")

nlp = pipeline("ner", model=model, tokenizer=tokenizer)
example = "My name is Wolfgang and I live in Berlin"

ner_results = nlp(example)

entities = postprocess_ner(ner_results)

fuzzy_match_entities(entities, candidates)
```

The next steps involved fine-tuning hyperparameters to reach a point where the performance rivaled that of GPT-4, presenting a potential cost-saving avenue for our client. Yet, my pursuit for optimization went further, leading me to explore the [FLAN-T5](https://huggingface.co/docs/transformers/model_doc/flan-t5) Language Model by Google, accessible through the [Hugging Face](https://huggingface.co/) library.

With the introduction of FLAN-T5, I carefully engineered a specialized prompt, embedding entities and context to minimize Language Model hallucinations. This intricate integration propelled our model into the green zone, achieving performance comparable to GPT-4, while simultaneously sparing our client the hefty $100,000 monthly expenditure.

```python
# Using FLAN-T5 with a special prompt for entity recognition
from transformers import FlanT5ForConditionalGeneration, FlanT5Tokenizer

# Example code snippet for FLAN-T5
tokenizer = FlanT5Tokenizer.from_pretrained("google/flan-t5-xl")
model = FlanT5ForConditionalGeneration.from_pretrained("google/flan-t5-xl")

input_text = "Special prompt engineered for entity recognition..."
encoded_input = tokenizer(input_text, return_tensors="pt")
output = model(**encoded_input)
```
## Crafting the Optimized AI Pipeline: Balancing Runtime, Recall, and Precision

The assembly of our new AI pipeline involved a meticulous orchestration of the selected models - a combination of Named Entity Recognition (NER), fuzzy matching, and the FLAN-T5 Language Model. The goal was to strike a delicate balance between runtime efficiency and maintaining high levels of recall and precision.

The process commenced by integrating the NER model, responsible for extracting entities from the original string. Leveraging its output, we then employed fuzzy matching, utilizing the confidence scores provided by the fuzzywuzzy library to gauge the quality of matches. This crucial step allowed us to set two critical boundaries: an upper boundary where matches were accepted outright based on high confidence scores, and a lower boundary where matches with lower confidence scores triggered a query to the FLAN-T5 model for validation.

```python
# Integration of NER model, fuzzy matching, and FLAN-T5
entities = extract_entities(original_string)
entity_matches = fuzzy_match_entities(entities, candidates)

for match in entity_matches:
    if match['confidence'] >= upper_boundary:
        accept_match(match)
    elif lower_boundary <= match['confidence'] < upper_boundary:
        validate_with_flan_t5(match)
    else:
        reject_match(match)
```

This dynamic approach allowed us to efficiently handle matches with varying degrees of confidence, optimizing the runtime for high-confidence matches and utilizing FLAN-T5 to validate and enhance precision for lower-confidence matches. The careful customization of these integration steps paved the way for a streamlined and cost-effective AI pipeline, providing a comprehensive solution that effectively addressed the specific challenges posed by the client's matching problem.

## Conclusion: Elevating Your AI Infrastructure for Unmatched Efficiency and Cost Savings

In the culmination of this transformative project, we've not just addressed a unique matching challenge but have successfully revolutionized the landscape of your AI pipeline. Through the strategic integration of cutting-edge technologies, our tailored solution goes beyond conventional boundaries, delivering unparalleled efficiency and substantial cost savings. The optimized pipeline doesn't merely meet expectations; it redefines what's possible in the realm of AI-driven solutions.

## Elevate Your AI: Future-Ready Solutions Await

Looking ahead, our commitment to delivering excellence extends to the future. Our services offer a gateway to continuous innovation, leveraging the latest advancements in language models and fine-tuning techniques. Elevate your AI infrastructure, ensuring it stays at the forefront of technological prowess. With our applied data science and AI engineering expertise, your system is not just optimized; it's future-ready, poised to adapt and thrive in the ever-evolving landscape of AI technologies. Let's embark on this journey together and unleash the full potential of your AI endeavors.