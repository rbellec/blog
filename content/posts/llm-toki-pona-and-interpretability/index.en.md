---
title: "Why an LLM in toki pona?"
date: 2026-09-02
draft: false
description: "A complete 140-word language as a model organism for LLM interpretability: the bet, the challenges, and what we might hope to learn from it."
tags: ["toki pona", "interpretability", "LLM"]
categories: ["Interpretability"]
---

*Translated from the [French original](https://rbellec.github.io/blog/posts/llm-toki-pona-and-interpretability/) with AI assistance, then reviewed by the author.*

Could a language of 140 words help us better understand what happens inside an LLM? In every discipline that deals with complex systems, a *simple* model has driven enormous advances: *E. coli* and the fruit fly in biology, the harmonic oscillator in physics.

In image recognition, MNIST plays that role: a **complete** task, ten digits and nothing else. Ideal for learning, understanding, testing. I built my first neural networks on that dataset, and looking back, I can now measure how much easier a simple object makes experimentation.

For LLMs, a similar approach exists: Tiny Shakespeare and TinyStories, for instance, where you work with a reduced corpus. The BabyLM initiative is another interesting direction, where the data budget is cut down by asking "how many words does a child hear before learning to speak?". But in all three cases, we remain **inside an immense language**, whose rules come from a corpus infinitely larger than the one given to the model.

Toki pona is entirely different: it is **a small, complete language, with no larger corpus behind it** — and one in which people can already express themselves.

## The challenges

The first challenge, the one that slowed me down the most, is… the language barrier. To work with toki pona, you have to learn it! Even if it is probably the easiest language in the world to learn, it is still a barrier to reckon with.

I had this project in mind back in 2024 but… I don't like learning a language alone! Ignoring everything humanity has learned about adolescence, I suggested to my son that we learn it together, hoping along the way for some quality father-son bonding. It only took me two years to resign myself to learning it alone. In my defense, I had detected ten minutes of curiosity in his eyes, whereas my proposal to learn cuneiform together had earned nothing but a dismayed look between two TikTok videos.

Faced with this, I considered turning to **Minimal English** (Goddard & Wierzbicka), a restriction of English to around 400 words, designed as a simple auxiliary language, or to its ancestor, Ogden's **Basic English**. But beyond a long-standing curiosity for toki pona, having a **complete linguistic object** — rather than a subset carved out of a larger language — strikes me as a genuine testing ground.

And there is a risk I cannot rule out: a subset of a language I already know is not a closed system. Its words keep the meaning they have on the outside. The model may inherit that meaning, through its tokenizer or through pre-training… And so do I, projecting it as I read the results: I think I understand what the model does with *dog* because I already know what *dog* means. In toki pona, *soweli* reminds me of nothing: what it covers has to be spelled out!

This very concern is what gave birth to Minimal English. Wierzbicka has long argued that making English the default language of a discipline smuggles specifically English concepts into it without anyone noticing (*Imprisoned in English*, 2014).

Besides, being French, I might be tempted by an old rivalry to say that English is perhaps not the best "lingua franca" :). Toki pona is neither English nor anyone's lingua franca, which is as much a risk as an interesting opportunity.

## The bet

The real unknown? The **universality** bet: will the mechanisms we observe in a model trained on this language carry over to models trained on incomparably larger languages and corpora? This is a strong version of the universality hypothesis [(Olah et al., 2020)](https://distill.pub/2020/circuits/zoom-in/).

This bet exists in other fields and has not always paid off. When it fails, the model organism usually ends up becoming the object of study in its own right. The history of chess in AI is one example: it produced excellent game-playing programs and almost nothing on general intelligence. It can be summed up in two quotes:

> **Alexander Kronrod** (Soviet mathematician, ~1965): *"Chess is the Drosophila of artificial intelligence."*
>
> **John McCarthy**, *[Chess as the Drosophila of AI](http://jmc.stanford.edu/articles/drosophila/drosophila.pdf)* (in *Computers, Chess, and Cognition*, 1990): *"It is as if the geneticists after 1910 had organized fruit fly races and concentrated their efforts on breeding fruit flies that could win these races."*

The risk that the model excels at toki pona and yields nothing significant about anything else is real. But the investment needed to find out strikes me as minimal, and should that happen, the title of "racing fruit fly trainer" would be a perfectly acceptable consolation prize.

## What's next

I have a first trained model, a corpus, and I'm itching to replicate well-known interpretability results. Happy to discuss!

To learn more about toki pona, a minimal language (fewer than 140 words, 14 phonemes) invented by Sonja Lang in 2001, with a genuine community of speakers: [tokipona.org](https://tokipona.org/).
