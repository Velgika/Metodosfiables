<!-- This is the markdown template for the final project of the Building AI course, 
created by Reaktor Innovations and University of Helsinki. 
Copy the template, paste it to your GitHub README and edit! -->

# Expert reliability assessor

Final project for the Building AI course

## Summary

VeriFAI: Testing the experts! ⚖️

Judges aren't scientists, but they must guard the truth. VeriFAI bridges that gap. Our app evaluates expert testimony through the 4 pillars of reliability: testing, peer review, error rates, and consensus.

Empowering fair trials with clear, scientific standards. 


## Background

This app helps to solve: 
The "Black Box" of Expertise: Judges and lawyers often accept expert reports as absolute truth because they don't have the technical background to challenge them.

The Authority trap: Decisions are often based on a perito’s years of experience (prestige) rather than the scientific validity of their specific method.

Lack of standardized review: Without a clear checklist, the evaluation of evidence becomes subjective, inconsistent, and prone to judicial error.

How common or frequent is this problem?
This is a universal challenge in modern litigation. In almost every complex case—from DNA profiling and environmental damage to digital forensics—a judge must rely on an expert. Without a tool like this, "junk science" can easily slip into the courtroom, leading to wrongful convictions or unfair settlements.

My motivation comes from seeing the gap between law and science. I believe that justice shouldn't be a matter of opinion or who speaks the loudest or from the most ranked experts; it should be built on the most reliable information available. I want to build tools that help humans in the legal system make more objective, evidence-based decisions.

## How is it used?
The app follows a structured workflow to demystify complex technical reports:

Input: The user enters the core premise of the expert’s report and the specific methodology used.

Assessment: The user goes through a guided "Reliability Audit" based on the four parameters (Testing, Peer Review, Error Rate, and Consensus).

Scoring & Flagging: The app identifies "Red Flags" (e.g., a method that has never been peer-reviewed) and calculates a reliability index.

Output: A summary report is generated, providing the user with objective arguments to either support, challenge, or admit the evidence in a legal setting.

![image of a uu](/uu.png)

## Data sources and AI methods
**Data sources**
**Primary scientific databases are the backbone.** PubMed, Scopus, and Web of Science lets to cross-check whether a methodology has ever appeared in peer-reviewed literature. CrossRef and OpenCitations provide citation counts and journal metadata, which are direct proxies for the "peer review" and "general acceptance" pillars of the Daubert-style audit.
**Retraction Watch** is a particularly powerful and underused source. If an expert's foundational paper has been retracted, that's an automatic red flag — and it's machine-queryable.
**Standards body repositories** (NIST, ISO, ASTM, OSAC for forensics) gives a curated list of validated methodologies. Checking whether a method appears in an NIST or OSAC registry essentially answers the "testing" and "error rate" pillars with a single lookup.
**Court records and legal databases** (PACER in the US, similar in other jurisdictions) are valuable for building a training corpus — transcripts of past Daubert/Frye hearings where judges already evaluated expert reliability. This is gold for supervision.

**AI methods**
NLP + information extraction as an entry point. A fine-tuned model reads the expert report and extracts: (1) the methodology name, (2) the claimed error rate, (3) any references cited. This structured output feeds everything downstream.

Retrieval-Augmented Generation (RAG) is the core engine for the four pillars. Instead of relying on an LLM's parametric memory, it embeds the academic corpus and retrieve the top-k relevant documents for each methodology. The LLM then grounds its assessment on real citations — making the output auditable and hallucination-resistant. This is critical for a legal setting where you cannot afford fabricated references.

A hybrid scoring model works best for the reliability index: a transparent rule-based layer (e.g., "if no peer-reviewed paper exists → -30 points on pillar 2") combined with a fine-tuned classifier that handles ambiguous cases. The rule-based component makes the score explainable to judges, which is a non-negotiable requirement in court.

Semantic similarity search (via embeddings) handles the fuzzy matching problem: an expert may call their method "advanced spectral analysis" while the literature calls it "GCMS." Vector search bridges these surface-level naming differences.

Report generation via an LLM (prompted carefully) translates the numerical score and flagged issues into plain legal language — the kind a judge can cite in a ruling. The prompt should enforce citation of specific sources for every claim.

**Key design principle**
For a legal tool, explainability beats accuracy. Every score must trace back to a specific data point (a paper, a registry entry, a citation count). A black-box neural score of 73/100 is useless in court; a score of 73/100 because the method has 2 peer-reviewed validations, no known error rate, and appears in no NIST registry is actionable.Sonnet 4.6

## Challenges

Not all theories and techniques have a method. Consequently, when experts in art gives testimony, the app wont help much. 

**Absence of evidence vs. evidence of absence** If VeriFAI finds no peer-reviewed paper for a method, that could mean (a) the method is genuinely unvalidated junk science, or (b) the database coverage is incomplete, or (c) the method is so new it hasn't been indexed yet. Communicating this uncertainty honestly — without either alarming the user or giving a false pass — is one of the hardest UX problems in the project.

**Data freshness and coverage gaps** Scientific consensus changes. A method considered gold-standard in 2010 may be discredited by 2024 (bite mark analysis is a real example of this). The data pipeline needs continuous updates, and it needs to handle jurisdictional gaps — NIST covers US forensics well, but a Chilean judge evaluating environmental damage testimony faces a very different landscape.

## What next?

**Build and curate the training corpus**
Start collecting and annotating real expert testimony transcripts — especially cases where a court did rule on the admissibility of scientific evidence (Daubert hearings, Frye hearings, appeal rulings). These are the ground truth labels. Even 50–100 well-annotated examples will let test the extraction and scoring pipeline against known outcomes.

**Look specifically at cases where the expert was later discredited or the method was overturned** — bite mark analysis, hair microscopy, and blood spatter interpretation all have rich documented histories of exactly this.

## Acknowledgments

* Justice system
* Helsinky University
