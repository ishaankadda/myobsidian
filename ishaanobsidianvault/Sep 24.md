#CurrentFocus
- [[MATS Application]]

---
**What must a biological self-bootstrapping intelligence do? Assign an ordering of everything to be done. Play it like Splendor. Life must be played in Splendor.**

no more music
no more caffeine or energy drinks (subject to change later)
no more ego (desire can be postponed for later, or taken care of efficiently. But do not commit for the sake of desire.)

what to do?
clearly detail your vision, predictions for the future, markers to look out for, events to take care of, where you want to be (this will determine many of your actions right now), etc.

reflection and thought are not useful unless they change your behaviour, because it would imply that you have not learnt.

the primary focus for bootstrapping is to figure out a system for these convergent instrumental goals:
1. survival: taken care of? extend to good health and better functioning and risk minimization.
2. alignment: actions should be aligned with the system goal.
3. objective stationarity: objectives should not vary. the agent should remain aligned with the goal and the goal should be crystal clear and not vary on a large timescale (~1 year). The objective will take a long process of planning and updating and refinement. It should guide everything and everything should contribute to the objective.
4. more power, more resources: more power and more resources are good. (build personal brand?)
5. other humans make great bootstrapping devices - it is important to maintain, spread and foster social contact with people who are much much smarter than you and poised to help you or mentor you or be a wall for you to push through.

first you [[learn how to learn]] and then learn how to stay oriented towards goals and then how to set goals and then what goals to be setting and then how to take care of yourself and then how to manage your resources and then the adversarial game with people and then blabla.

# GRE, TOEFL

# Plan chart for Postgrad with Jaisidh
Revisit the timelines that you wrote down in college journal, grab the most important points and start that shit again. Shit needs to be done with in order to progress onto the next stage. Or else, figure out similar prereqs for whatever you need.

# IDEA: get paid for AI tasks
		[shipd.datacurve.ai](https://shipd.datacurve.ai)- a platform that pays you for creating and solving tough problems. provide the data to train better coding models.
Idea is to do this but for AI guys in companies.

# IDEA: Agent Onboarding
You have skipped the part of **what you are building? (idea validation)** and skipped straight to the **how to build it.**

The future of sandboxing. Entire companies will be sandboxed and will allow for the onboarding of highly capable ***AI Developers***, that will be capable of thinking and reasoning their way through tasks for long periods of time. These developers will be trained on proprietary data and provided to us by the leading labs such as OpenAI, for example. (specialized on the backend). 

What will be the workflow in such a company?

# FuseNets
Implement efficientformer
Integrate existing code for plugging in random backbones

# Violence
**Deadline: 01 - 10 - 24**
Tejus -> check out thresholds on SHDataset
Ishaan -> ~~train on skeleton data (ipynb training -> benchmark (val, own))~~ -> training done! run inference
Try out VideoClip for violence

# Pedestrian Attribute Recognition
VTFPAR++ and VideoCLIP -> have to try these out. 
PromptPAR -> (concluded) datasets seem to be trained on non-color specific classes.
UPAR_challenge -> ~~train a model on PAR dataset~~ -> ~~training done! run inference~~ -> Inference done! Organise it into results. [[UPAR challenge results]]
Datasets -> refer to slack image and download all the datasets.
![[Pasted image 20240924122234.png]]

# Dokku
Get started and deploy some shit here

# Paper Reading
Read 2 - 3 papers in the jaisidh chat
daily.dev bookmarks - read any 2
Psycho Cybernetics

# Reading
The cold start problem
Thinking Fast and Slow - Daniel Kahneman
[[My experience with AI safety]] - LessWrong, The Precipice, The Alignment Problem, Human Compatible, Life 3.0, Superintelligence - via [[MATS Application#Reading list]]
An extremely Opinionated Annotated List of my favourite mechanistic interpretability papers v2 - [A Blog by Neel Nanda](https://www.alignmentforum.org/posts/NfFST5Mio7BCAQHPA/an-extremely-opinionated-annotated-list-of-my-favourite-1)
Susim wala blog on Deep Learning optimization tech - whatsapp
**Towards Developmental Interpretability -** [LessWrong](https://www.lesswrong.com/posts/TjaeCWvLZtEDAS5Ex/towards-developmental-interpretability) Continue Reading!!!! -> A mechanistic interpretability analysis of phase changes - [LessWrong Neel Nanda](https://www.lesswrong.com/posts/N6WM6hs7RQMKDhYjB/a-mechanistic-interpretability-analysis-of-grokking#Speculation__Phase_Changes_are_Everywhere)



# How to handle distributions and releases?
Read `download_datasets.py` from the [upar_challenge repo](https://github.com/speckean/upar_challenge/tree/main)
Figure out what to do about FuseNets linkedin and how to establish technical superiority on it!

# The Vortex
Online website synced to my markdown displaying all of my obsidian notes in a zettelkasten fashion.

# Yash Akhauri BITS Pilani

# idea - jump to competitor in US


# Absolutely Do's
- validate your ideas as fully as possible without having to invest significant resources into it.

# Absolutely Don'ts
- dont buy a high cc bike not more than 100cc - resale value khatam ho jayegi
- maintain a life and meet new people, try to be in a company of people who you want to be like.


# graph RAG
Try it out and figure out what can be done about it.

# Style Transfer Website
Read this - https://distill.pub/2017/feature-visualization/
Get inspired and resproduce results on some of the approaches
Ideas:
1. Implement unregulated gradient ascent
2. Implement Style Transfer
3. Implement both kinds of gaussian blurring and decorrelation in the fourier basis
4. Improve Style Transfer using 4.
	1. Figure out a novel direction and improve even more

\
# Bottlenecks
Learn how to implement transformers - [[Reproducing Attention is All you Need, from scratch - PyTorch]]-> when done, link to [[SOTA Thread]]
Learn how to implement transformers - implement a ViT -> when done, link to [[SOTA Thread]]
Train an LLM and instruction tune it -> when done, link to [[SOTA Thread]]
Read up on [[word2vec]] -> when done, link to [[SOTA Thread]]
Implement Reformer, Linformer, [[FlashAttention]]
Implement StyleGAN and try out style transfer, explore the embeddings and figure out how to use it.
Try out this idea for CoN-CLIP - [[Sep 24#Multiple representation subspaces for a better CLIP loss]]

# Adversarial Attacks based on positional embeddings
# Demonstrate grokking in relation to `Toy Models of Superposition#Superposition and Learning Dynamics`
It seems that correlated features sets are kept together in the beginning of the learning process in the toy model, and the learning dynamics shift as soon as they are separated completely.
![[Pasted image 20241007235150.png]]

# Multiple representation subspaces for a better CLIP loss
This idea is based on the contrast between Multi-Head Self Attention, where multiple heads are used to compute information in <u>different representation subspaces</u>. It is very much possible to compute the similarity as a summation across different representation subspaces instead of one big subspace like CLIP traditionally does.

# Interpretability - Find the 'no' circuit and replicate in LLM -> find it again in CLIP

# Unsupervised Sentiment Neuron



```5crop-arrangement-in-OfficeViolence-dataset-from-tejus
0   1
  4
2   3
```

###### Things to do today
1. violence
	0. Make a `testbench_pune` folder for the Pune video dataset.
	1. Mark video timestamps into False, True, and Fine-grained. Store these into a `violence_timestamps.pt` file.
	2. Write a script to generate the False(`nonfight_dirname`), True(`fight_dirname`), and Fine-grained(`fight_dirname`) folders and crop the videos into time-segments of a given length, and place them accordingly into the folders.
	3. Study all the main approaches
		- `pel4vad`: 
		- `skeletons`: 
		- `internVL`:
		- `internVL-2`:
		- `Llama3.1-vision-32B`:


###### pune\_testbench
There are a total of 520 splits (10-second clips). The name of each clip contains the label `__vio` or `__nonvio`.


1. `Fight_1_Cam1_1.json`: 
2. `Fight_1_Cam2_1.json`: 
3. `Fight_2_Cam1_1.json`: 
4. `Fight_2_Cam2_1.json`: 
5. `Fight_3_Cam1_1.json`: 
6. `Fight_3_Cam2_0.json`: 
7. `Fight_4_Cam1_0.json`: 
8. `Fight_4_Cam2_0.json`: 
9. `Fight_5_Cam1_0.json`: 
10. `Fight_6_Cam1_0.json`: 
11. `Fighting_person_collapsing_faiz_nri.json`: Moderate violence. Mostly shoving and light playful blows back and forth.
12. `violence114_20241022_172001.json`: 
13. `violence121_20241022_172001.json`: 
14. `violence127_20241022_172001.json`: 
15. 
