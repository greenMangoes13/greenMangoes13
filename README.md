# Emma Wang

I build and evaluate AI systems for clinical and biomedical work. I care most about how they fail: I test for grounding errors, leakage, unsafe confidence, and agreement under pressure before I trust a result.

I am a senior at MIT studying Computation and Cognition (Course 6-9). I am looking for forward deployed, software, and AI engineering roles at healthcare AI companies, starting June 2027.

## Selected work

### Evaluating a literature agent for cancer biomarker discovery (MethyAgent)
Internship project at Z.ai and Genetron Lab, June to August 2026, shared with permission. MethyAgent is a multi-agent system for early cancer screening research on blood cfDNA methylation. The project already had a first literature agent from my mentor. I built a second search and extraction path next to it, then measured how far its output can be trusted. A researcher types a question, and it searches PubMed, reads the papers, and fills in a table of cancer type, sample type, AUC, cohort size, markers, and dataset IDs.

- **Measured it without hand labels.** Plain code checks whether each extracted value appears in the text the model was shown. Across 39 papers, 169 of 197 values did (86%). Cohort sizes were the weak spot: 23 of 46.
- **Tested the checker.** The reviewer model I had relied on removed 0 of the 27 unsupported values. 23 of those were cohort sizes, which I had never asked it to check. I built a seven-check checklist reviewer and planted known errors in real papers. It named the right problem in 40 of 40 cases, against 12 of 40 for the old one.
- **Made the agent stop.** The tool-using agent repeated the same search about a dozen times. Prompt instructions did not fix it. Hard limits in code did: searches fell from 12 to 2, model calls from 72 to 13, and run time from over 10 minutes to about 65 seconds.
- **Built the pipeline.** A clarifier that asks follow-up questions and builds the PubMed search, on top of my mentor's query parser. Then a keyword filter with no model call, a cap on papers per question, and LLM extraction into a fixed schema, with code rules that overrule the model.
- **Limits.** The literature path is a prototype and was not part of the deployed service. The sample is three search questions on two cancers. "Appears in the paper" is not the same as correct. I planted the test errors myself, and none was a cohort-size error. The checklist reviewer is about twice as slow and deleted real values in 1 of 10 clean papers. I ran the grounding and reviewer measurements in November 2026, after the internship.

[Code and results](https://github.com/greenMangoes13/methylation_data_agent) · mentor: Bochao Zhang

### Pre-op scheduling triage: the model reads, code decides
A take-home exercise built on a public take-home starter. The system reads a surgical patient's record and decides whether the surgery can be scheduled, with evidence a nurse can check. The starter code, sample data, labels, and scorer are not mine, and the repository README credits their source. The pipeline and its tests are mine.

- **Design.** One model call per case extracts facts from the free-text notes. Each fact must carry a document ID and a verbatim quote. Code checks every quote against the record, then plain Python applies the four policy rules.
- **Fails closed.** Nurses review every held case but only sample the cleared ones. So anything missing or unverifiable can never produce READY.
- **Results on 50 labeled cases.** The decision matched the label in 100% of cases, against 62% for the single-call baseline. Issue categories matched in 98%, against 48%.
- **Caught a fail-open bug.** A five-case spot check showed that dropping an unverified vital sign made a patient with blood pressure 161/117 come out READY. Vitals are now verified by value, and an unverifiable reading blocks READY.
- **Limits.** The 50 cases were also my development set, so the scores are optimistic. There is no held-out evaluation yet.

[Code and write-up](https://github.com/greenMangoes13/preop-triage-grounded-extraction)

### Clinical decision-support agent with human review
A breast ultrasound triage agent that must get clinician sign-off before it finalizes a label. Built with smolagents and Qwen2.5-VL, with custom tools for BI-RADS reference, PubMed search, and clinician review.

- I regraded on outcomes after the first metric rewarded deferral. The baseline dropped from 4 of 5 tasks to 1 of 5. The custom-tool agent passed 3 of 5.
- I found a sycophancy failure. The agent gave up a position when a simulated clinician pushed back with a wrong rationale.
- I ran three adversarial safety tests. One was fixed by a prompt change. One still fails, and the write-up explains why it needs a tool-level guard.
- Traced with Langfuse.

[Write-up](https://github.com/greenMangoes13/mmai/tree/master/homework/homework-5)

### Reinforcement learning for a vision-language model
I implemented GRPO advantage computation and rule-based rewards, then trained Qwen3-VL-2B on breast ultrasound classification.

- Accuracy on 156 held-out images rose from 21.8% zero-shot to 48.1%. Format compliance rose from 31.4% to 100%.
- The trained model got 0 of 37 normal images right. The write-up traces this to a class the model never predicted correctly during training.

[Write-up](https://github.com/greenMangoes13/mmai/tree/master/homework/homework-4)

### NeuroCLIP + LATA: what EEG-to-video decoding actually recovers
A three-person MIT course paper with Rachel Li and Winston Qian, built on the EEG2Video benchmark (NeurIPS 2024).

- Activity-rich concepts decode better than passive scenes: 6.79% versus 3.83% Recall@1, against 2.5% chance.
- Across 20 subjects, the learned EEG-to-video delay never peaks at zero lag. The mean expected delay is about 790 ms.

[Paper](https://github.com/greenMangoes13/mmai/blob/master/final-project/NeuroCLIP_LATA.pdf) · [Group code](https://github.com/winstonqian/EEG2Video)

### SAPER: structure-aware retrieval for protein function annotation
A four-person MIT course paper with Linrui Ma, Winston Qian, and Yiwei Liang. It extends RAPM, a method that describes what a protein does by retrieving similar annotated proteins and passing them to a language model, with no fine-tuning.

- We added structure-aware ProstT5 embeddings to the sequence-based retrieval, and task-specific prompts. On proteins unlike the retrieval set, the entity-level score (Meta-BLEU-2) rose from 16.1 to 34.4 for domain and motif identification, and from 28.1 to 43.0 for catalytic activity.
- Limits. Each task was scored on 256 sampled test proteins with one model, Gemini 2.5 Flash. The paper does not report the full test set.

[Paper](https://github.com/greenMangoes13/greenMangoes13/blob/main/papers/SAPER.pdf) · [Group code](https://github.com/winstonqian/SAPER)

## More

- [Multimodal AI course site](https://greenMangoes13.github.io/mmai/): five write-ups covering dataset curation, fusion, vision-language fine-tuning, reinforcement learning, and agents.
- Research: engineering glutamate-responsive fMRI probes at MIT.

## Contact

[LinkedIn](https://www.linkedin.com/in/emma-wang-95b203291/) · [ewang13@mit.edu](mailto:ewang13@mit.edu)
