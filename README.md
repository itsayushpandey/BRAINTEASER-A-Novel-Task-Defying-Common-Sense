# BRAINTEASER-A-Novel-Task-Defying-Common-Sense (https://brainteasersem.github.io/)

## Background

The BRAINTEASER task centers around examining elements of speech that are not immediately apparent. The input is a question and a list of choices. Notable about the question is how it takes advantage of unconscious assumptions and irrelevant information. The dataset is in English, and provided by the task organizers. 
For the brainteaser task, there are two subtasks: sentence puzzle and word puzzle. Sentence puzzles focus on unstated, typical, and wrong assumptions about the question/answer, while word puzzles defy common sense since they hinge on a word's actual alphabetical makeup rather than its actual denotation. We chose to focus exclusively on the sentence puzzle. 
We made use of BERT, XLNet, BART,T5, yoso and Roberta models specifically the base HuggingFace implementations of each (https://huggingface.co/facebook/bart-base, https://huggingface.co/xlnet-base-cased, https://huggingface.co/bert-base-uncased, https://huggingface.co/t5-base, https://huggingface.co/uw-madison/yoso-4096, https://huggingface.co/roberta-base).

## System Overview

Our system aimed to separate the problem into the smallest possible unit of work, the individual question and answer choice, and build from there. As such, a high-level overview of our system is as follows: (1) split the data from multiple choice questions into individual question-answer pairs, (2) tokenize the data, (3) train the model (we used several), and (4) evaluate. Note that our evaluation step was further split into (a) splitting the multiple-choice question into four question-answer pairs, (b) predicting the class using the trained model, and (c) finding the question-answer pair with output closest to 1, submitting that as the answer. 
Splitting the multiple-choice questions into individual question-answer pairs was trivial. For explicitness, consider the following example. The element (“Question: How can a man goes to football team every day but doesn't play football at all?”, “choice_list: [‘He is a coach.’, ‘It is raining every day’]”) into “Question: How can a man goes to football team every day but doesn't play football at all?”, “answer: It is raining every day,” “label: 1”). The label field being 1 signifies the answer is correct, and the label field being 0 signifies the answer is incorrect.  As such, the closer to 1 that our model predicts, the more likely it is to be an actual answer.

## Experimental Setup
We tailored our approach to effectively handle single-question answer pairs during training and development phases, while the testing phase was conducted on entire sets of multiple-choice questions. We performed our training and development on single-question answer pairs and our test on the whole multiple-choice questions. As such, we preprocessed our train/dev data to split it from the format of “question,” “answer,” “districtor1”, “distractor2”, and “distractor(unsure)” to the format of “question,” “answer,” and “label,” where the answer could be either the correct answer or a distractor with the label being 1 or 0 if the answer is correct or a distractor. This restructuring was crucial for training our models to distinguish between correct and incorrect answers effectively.
We split our data into 90% train and 10% dev/test. We chose this split due to the small amount of training data to work with (512 distinct multiple-choice questions). We set 10 epochs as our upper limit; however, we never reached this as we also implemented early stopping, where the training loop would cease as soon as the validation success decreased. Furthermore, we employed the ‘sentencepiece’ tokenizer in our preprocessing pipeline. This choice was particularly crucial due to its ability to handle languages without clear word boundaries. The ‘sentencepiece’ tokenizer's versatility made it an ideal tool for dealing with the diverse and complex linguistic structures present in our dataset, ensuring that our models could process and understand the data effectively.

# Result
The results of our models on the 90/10 training/test split are as follows:
Model used for classification layer
Accuracy (51 Multiple Choice Questions)
XLNet
98.0%
RoBerta
96.0%
BERT
96.1%
BART
94.1%
T5
82.3%
YOSO
35.2%


