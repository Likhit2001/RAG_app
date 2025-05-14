🔍 Project Overview

RECAP is a Retrieval-Augmented Generation (RAG) based question answering system built on the RAG-12000 dataset, which consists of context-question-answer triplets. This project focuses on improving the retriever component using fine-tuning strategies and evaluating how enhanced retrieval impacts answer quality.

🧠 Key Components

📖 Dataset
	•	RAG-12000: A structured dataset with triplets: context, question, answer.

🔗 Model Architecture
	•	Retriever: Sentence Transformers (all-MiniLM-L6-v2) enhanced with a projection head.
	•	Fine-Tuning Strategies:
	•	LoRA (Low-Rank Adaptation)
	•	Partial Layer Freezing
	•	Generator: T5 model with retrieved context + question as input.


 ⚙️ Methodology

 
	1.	Retriever-Generator Pipeline:
  	•	The context passages from the RAG-12000 dataset are preprocessed and broken into smaller chunks (approx. 300 characters each) to improve the granularity of retrieval.
  	•	These chunks are embedded using a Sentence Transformer (all-MiniLM-L6-v2) to better align with the question embeddings.
  	•	Given a question, the retriever searches over the encoded chunks using FAISS to find the most relevant context.
  	•	The retrieved chunk and the corresponding question are passed as input to a T5 model to generate the final answer.
	2.	Retriever Fine-Tuning Techniques:
  	•	LoRA: Adds small trainable low-rank matrices to reduce the number of trainable parameters and improve efficiency.
  	•	Partial Layer Freezing: Unfreezes only the bottom layers of the Sentence Transformer, allowing task-specific adaptation while keeping most of the pretrained model fixed.
	3.	Training Configurations:
  	•	LoRA: Trained for 3 epochs with a learning rate of 5e-5 and batch size of 16.
  	•	Partial Freezing: Trained for 10 epochs with a learning rate of 2e-5 and batch size of 64.


Results

  Model Accuracy retrival for Top-5.
  
  Partial Layer Freezing : 0.93
  LoRA Fine-Tuning : 0.90
  Baseline (MiniLM) : 0.89
  Projection Head Only : 0.002
