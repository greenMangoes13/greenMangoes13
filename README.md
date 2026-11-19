# Emma Wang

I build and evaluate AI systems for clinical and biomedical work. I care most about how they fail: I test for grounding errors, leakage, unsafe confidence, and agreement under pressure before I trust a result.

I am a senior at MIT studying Computation and Cognition (Course 6-9). I am looking for forward deployed, software, and AI engineering roles at healthcare AI companies, starting June 2027.

## Selected work

### Literature agent for cancer biomarker discovery (MethyAgent)
Internship project at Genetron, June to August 2026, shared with permission. MethyAgent is a multi-agent system for early cancer screening research on blood cfDNA methylation. I owned the literature agent. A researcher types a question, and it searches PubMed, reads the papers, and fills in a table of cancer type, sample type, AUC, cohort size, markers, and dataset IDs.

- **Built it.** A query parser that asks follow-up questions, then two-stage extraction. A keyword filter drops about 70% of papers before any model call.
- **Made it stop.** The tool-using agent repeated the same search about a dozen times. Prompt instructions did not fix it. Hard limits in code did: searches fell from 12 to 2, model calls from 72 to 13, and run time from over 10 minutes to about 65 seconds.
- **Measured it without hand labels.** Plain code checks whether each extracted value appears in the source paper. Across 39 papers, 86% of values did (169 of 197). Cohort sizes were the weak spot at 50%.
- **Tested the checker.** The reviewer model I had relied on caught 0 of 27 unsupported values. I built a seven-check checklist reviewer and planted known errors in real papers. It named the right problem in 40 of 40 cases, against 12 of 40 for the old one.
- **Limits.** The sample is three search questions on two cancers. "Appears in the paper" is not the same as correct. The checklist reviewer is about twice as slow and raised 1 false alarm in 10 clean papers.

[Code and results](https://github.com/greenMangoes13/methylation_data_agent) · mentor: Bochao Zhang

### Pre-op scheduling triage: the model reads, code decides
A take-home exercise built on [Cadence's public starter](https://github.com/cadencerpm/preop-exercise). The system reads a surgical patient's record and decides whether the surgery can be scheduled, with evidence a nurse can check. The starter code, sample data, labels, and scorer are theirs. The pipeline and its tests are mine.

- **Design.** One model call per case extracts facts from the free-text notes. Each fact must carry a document ID and a verbatim quote. Code checks every quote against the record, then plain Python applies the four policy rules.
- **Fails closed.** Nurses review every held case but only sample the cleared ones. So anything missing or unverifiable can never produce READY.
- **Results on 50 labeled cases.** The decision matched the label in 100% of cases, against 62% for the single-call baseline. Issue categories matched in 98%, against 48%.
- **Caught a fail-open bug.** A five-case spot check showed that dropping an unverified vital sign made a patient with blood pressure 161/117 come out READY. Vitals are now verified by value, and an unverifiable reading blocks READY.
- **Limits.** The 50 cases were also my development set, so the scores are optimistic. There is no held-out evaluation yet.

[Code and write-up](https://github.com/greenMangoes13/preop-exercise)

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

## More

- [Multimodal AI course site](https://greenMangoes13.github.io/mmai/): five write-ups covering dataset curation, fusion, vision-language fine-tuning, reinforcement learning, and agents.
- Research: engineering glutamate-responsive fMRI probes at MIT.

## Contact

[LinkedIn](https://www.linkedin.com/in/emma-wang-95b203291/) · [ewang13@mit.edu](mailto:ewang13@mit.edu)
