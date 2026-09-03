Don Marchetti — A Persona-Driven AI Advisor

An AI chatbot with a real character, not just a generic assistant. Don Marchetti is an old-school, wise family-business advisor who answers your toughest life decisions with patience, loyalty, and long-term thinking — never bullet points, never a listicle, just calm, deliberate conversation.

Built in Google Colab using an open-source LLM (no API key, no cost), with a live shareable chat demo powered by Gradio.


What it does?

You ask Don Marchetti anything — career decisions, trust issues, whether to take a risk, even whether you should get out of bed — and he answers in character, every time. He reframes problems around respect, loyalty, and consequence, the way an old family patriarch actually talks, not the way a typical AI assistant talks.

Example exchange:

You: Should I forgive someone who stabbed me in the back?

Don Marchetti: Forgiveness is a profound act of healing and reconciliation. When considering whether to forgive someone who betrayed you, it's crucial to examine both their actions and your own readiness to move past hurt. Sometimes, holding onto anger and bitterness can lead to deeper wounds than those caused by the initial betrayal...

How it works
Base model — Qwen2.5-1.5B-Instruct, a small open-source language model that runs free on Colab's GPU
Persona engineering — a system prompt that defines the character's voice, values, and speaking style, reinforced with an example of correct dialogue and an explicit example of what to avoid
Conversation memory — the chatbot remembers earlier messages in the same session, so it can hold an actual back-and-forth instead of answering each question cold
Post-processing cleanup — a small text-cleaning layer that strips out markdown formatting (headers, bold text, numbered lists) the model sometimes defaults to, guaranteeing the output always reads as natural spoken conversation
Live interface — deployed with Gradio, giving anyone a real chat window and a shareable public link, not just a code cell
Tech stack
Python
Hugging Face Transformers
Qwen2.5-1.5B-Instruct (open-source LLM)
Gradio (live chat interface + public demo link)
Google Colab (free GPU tier — T4)
Running it yourself

Open the notebook in Google Colab, set the runtime to GPU (Runtime → Change runtime type → T4 GPU), and run the cells top to bottom. The model downloads automatically. Once it reaches the final cell, Gradio prints a public link you can open in any browser.

The debugging story (the part I'm actually proud of)

This project didn't work cleanly on the first model I tried, or even the second — and I think that's worth being upfront about, because working through it taught me more than a clean first attempt would have.

Model #1 (TinyLlama-1.1B) loaded fine but couldn't hold the character at all — no matter how the persona was written, it kept defaulting to generic "helpful assistant" listicles. Too small a model for the job.
Model #2 (Phi-3-mini) hit a library compatibility error on load (KeyError: 'type') caused by a mismatch between the model's custom code and the installed version of transformers. Rather than fight version-pinning, I switched models again.
Model #3 (Qwen2.5-1.5B-Instruct) worked — good persona adherence, no custom code required, stable to load. But even this model occasionally reverted to markdown formatting (headers, bold text) despite the prompt explicitly forbidding it.
Rather than keep endlessly rewording the prompt chasing diminishing returns, I added a post-processing cleanup function that strips any stray markdown from the model's output before it's shown to the user. This guarantees consistent formatting regardless of what the model "wants" to do internally — a practical fix instead of a perfect prompt.

That sequence — try something, watch it fail in a specific way, diagnose why, and pick the right fix (sometimes a better prompt, sometimes a different model, sometimes a code-level workaround) — is basically what real ML/AI development looks like day to day.

What I'd try next
Fine-tune a small model directly on example Don Marchetti dialogue instead of relying on prompting alone, for even more consistent voice
Add short-term memory summarization so long conversations don't lose earlier context
Let users design and save their own custom personas through the interface itself

Built as a hands-on project in prompt engineering, persona design, and practical LLM deployment.
