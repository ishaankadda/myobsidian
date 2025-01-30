# TODO
**low hanging fruit**
- mention how distractor images are collected!!!!!
- minor writing improvements - R1 (line 94, lines 355-357, Figure 5 L3 info)
- add new reference - R3
- equation 1 define $M_{ij}$ - R3
**important**
- explain the importance of L2 to explain ablation results in table 6 - R3
**even bigger tasks**
- addressing R2 concerns
- CLIP-FT

# Reviews (summary)
**Reviewer#1** - borderline accept
- writing improvements - line 94 double quote formatting, line 355-357 provide info on identification of unfaithful responses 
- provide more info about L3 in Figure 5 (unclear why $(i', c')$ should be pulled together.)
- Provide performance of baselines fine-tuned with CC-Neg alongside Table 3.
- line 607 - 612, Wants to measure classification accuracy based whether a triplet is scored correctly ($sim(i, c) > sim(i, c')$). It seems that he is confused about how it works. should we address this? 
**Reviewer#2**
- claims that simply penalising captions containing negation words could achieve a very high score on CC-Neg.
- too easy to saturate CC-Neg benchmark (99.7%)
- **collecting distractor images is hard and important, and the method is not mentioned in the manuscript!!!**
**Reviewer#3** - borderline accept
- `The ablation in table 6 is very interesting. For CC-Neg, it seems that L1 (loss matching I with C and C') is much more important than L2 (I and I' with C). This seems to indicate that distractor images are not very useful for the negation task. Can the authors expand on that and provide understanding of why the drop in performance is so much. Having a row for baseline would be very helpful too.` Note, this was mentioned as a strong point, perhaps he felt it was an informative ablation and needs more clarity on it.
- suggest a new reference - https://aclanthology.org/2021.blackboxnlp-1.27/
- equation 1 - $M_{ij}$ is not formally derived anywhere in the manuscript. Additionally, $i$ is used to subscript $L_i$ where $i \in \{1, 2, 3\}$ however the same $i$ is used on the right side of the equation in a different context.

# Reviews (full text)
```
# View Reviews

Paper ID - 2841

Paper Title - Learning the Power of “No”: Foundation Models with Negations

Track Name - Round 2 (New submissions)

Reviewer #1
Questions

    1. Paper Summary. In a few sentences, describe the key ideas, results, findings, and significance as claimed by the paper’s authors.
        Current Vision-and-Language Models (VLMs) like CLIP are struggling with handling negations. To address this, the authors proposed a novel dataset named CC-Neg, which contains paired images with both true and negated captions, designed to evaluate and enhance VLMs' ability to process negated text. Additionally, they introduced a framework called CoN-CLIP, which modifies CLIP's original contrastive learning loss functions to improve its comprehension of negated captions. Comprehensive experiments on multiple datasets and tasks demonstrate that CoN-CLIP improves performance in handling negated text information.
    2. Strengths. Consider, among others, the significance of critical ideas, validation, writing quality, and data contribution. Explain clearly why these aspects of the paper are valuable.
        1. The paper has good motivation that addressing the limitation of current VLMs in handling negations. The authors introduce the CC-Neg dataset for this purpose, along with the CoN-CLIP framework to learn better embeddings when processing negated text.

        2. The methodology is clearly presented, making it easy to follow.

        3. Comprehensive experiments across multiple datasets and tasks demonstrate the effectiveness of both the CC-Neg dataset and the CoN-CLIP framework.
    3. Weaknesses. Clearly explain why these are weak aspects of the paper, e.g., why a specific prior work has already demonstrated the key contributions, why the experiments are insufficient to validate the claims, etc.
        1. Writing Improvements Suggestions:
        1.1 In line 94, there is an issue with double quotes formatting.
        1.2 In lines 355 to 357, more details should be provided on how the authors identified these responses.

        2. In Figure 5, the L1 and L2 losses are straightforward, but for L3, it’s unclear why distractor images and negated captions should have the highest similarity. In real-world scenarios, they might be unrelated. The authors should provide more intuitive explanations in Section 4 to clarify the motivation behind designing L3.

        3. For Table 3, to ensure a fair comparison, all baselines should be fine-tuned using the proposed CC-Neg dataset. While the authors aim to show the effectiveness of CC-Neg, it would be better to compare the performance of baselines fine-tuned with and without CC-Neg, showing the differences in accuracy. This would be more informative than just comparing CoN-CLIP fine-tuned with CC-Neg.

        4. Line 607 to 612. The use of low top-1 accuracy needs more clarification. For example, in typical classification tasks, only one class is treated as correct, but in the negation setting, all 99 other classes with “this is NOT a photo of {class_name}” would be considered correct in the low top-1 accuracy case. This way may lack discrimination. Following the idea presented in Figure 2 (left), the authors could compare similarity scores for each triplet pair, counting a match if the correct one is higher. This may help for simplify and clarify the evaluation.
    4. Recommendation. Rate the paper as it stands now.
        Borderline Accept
    5. Justification. Be specific: What are the most critical factors in your rating?
        Please refer to the comments in the strengths and weaknesses sections.
    6. Best Paper Candidate
        Not award candidate

Reviewer #2
Questions

    1. Paper Summary. In a few sentences, describe the key ideas, results, findings, and significance as claimed by the paper’s authors.
        This manuscript addresses the challenge of understanding negations in vision-language models (VLMs), such as CLIP. It introduces the CC-Neg dataset, which includes images paired with both positive and negated textual descriptions to benchmark the ability of VLMs to process negations. The authors propose an enhanced framework, CoN-CLIP, which modifies the contrastive loss function in CLIP, significantly improving its performance in CC-Neg and enhancing zero-shot image classification accuracy across various datasets. This work claims to improve the handling of linguistic nuances in VLMs, enhancing their real-world applicability.
    2. Strengths. Consider, among others, the significance of critical ideas, validation, writing quality, and data contribution. Explain clearly why these aspects of the paper are valuable.
        1. Addressing a Critical Challenge: The paper targets the significant challenge of understanding negations in vision-language models (VLMs) and large language models (LLMs). This is a crucial area for enhancing semantic comprehension in applications where negations are integral to natural language. The focus on this under-explored area is a commendable effort towards addressing a foundational limitation in current AI capabilities.
        2. A key strength of the study is the demonstrated improvement in zero-shot classification across various datasets that do not specifically require negation understanding. This result is pretty surprising. This suggests that the proposed CoN-CLIP model not only handles negations better but also generalizes improvements to other types of classification tasks, enhancing the overall utility and applicability of VLMs.
        3. Writing Quality and Presentation: The paper is well-structured and articulately written, effectively communicating complex concepts and methodologies. The examples showing that even large and expensive LMMs can largely ignore negation are very impressive.
    3. Weaknesses. Clearly explain why these are weak aspects of the paper, e.g., why a specific prior work has already demonstrated the key contributions, why the experiments are insufficient to validate the claims, etc.
        1. The CC-Neg dataset seems to be imbalanced. All the negated captions are altered from the true captions and are supposed to be hard negatives (i.e. distractor captions). So if a model learns to simply penalize captions containing negation words, this model will achieve a very high score in the current CC-Neg.

        2. The CC-Neg dataset seems to be too easy to saturate after fine-tuning. The authors just finetuned a ViT-B/32 CLIP on CC-Neg and already achieved 99.70% accuracy (Table 3). Even which loss(es) to use in finetuning does not seem to be important, because in Table 6, three out of four loss configurations achieved above 99.70% accuracy. In fact, I suspect that he phenomenon that the CC-Neg is so easy to be saturated after finetuning is partially due to the imbalance issue I mentioned in the point above.

        3. How did the authors collected the "distractor images" that are supposed to match the negated cations? Basically the I' in their manuscript. Or the image that showing a standing skier in Figure 5, which matches the negated caption "A person on skies does not make her way through the snow". I did not find the authors explaining how to collect those images in the manuscript and I believe that these images are hard to collect. If the authors have a way to collect these images at a reasonable accuracy, they should add these images for the negated captions in their CC-Neg dataset to make it balanced.

        4. Not certain how curial their loss setup is. The authors purposed their loss as L1 + L2 + L3 in Figure 5. But if we add a L4 where the rows are c' and the columns are I + I', then L1+L2+L3+L4 will be equivalent to the original CLIP loss with (I + I') on the rows and (c + c') on the columns.

    4. Recommendation. Rate the paper as it stands now.
        Borderline Reject
    5. Justification. Be specific: What are the most critical factors in your rating?
        My main concerns are Weaknesses 1, 2, 3 that I listed. If those weaknesses are true, then this work did not necessarily improved the understanding of negations in VLM (neither the dataset nor the loss design), despite the fact that the problem that they are trying to tackle is crucial.
    6. Best Paper Candidate
        Not award candidate

Reviewer #3
Questions

    1. Paper Summary. In a few sentences, describe the key ideas, results, findings, and significance as claimed by the paper’s authors.
        The paper identifies the problem of CLIP style VLMs not being able to understand negations. To solve for that, it proposes a dataset CC-Neg which contains 226k image caption pairs with high quality negative captions as well. The paper describes the automated text annotation pipeline for the dataset. Furthermore, the paper proposes a training loss incorporating the negative captions in the dataset along with distractor images.
        Experiments are done over CC-Neg, multiple image classification datasets like ImageNet and the SugarCREPE benchmark.
    2. Strengths. Consider, among others, the significance of critical ideas, validation, writing quality, and data contribution. Explain clearly why these aspects of the paper are valuable.
        - The proposed method of generating negative captions by identifying predicate object pairs and then negating them is very interesting.
        - The analysis around performance degradation with higher complexity of captions and the favorability of certain negative words is quite interesting.
        - The abltation in table 6 is very interesting. For CC-Neg, it seems that L1 (loss matching I with C and C') is much more important than L2 (I and I' with C). This seems to indicate that distractor images are not very useful for the negation task. Can the authors expand on that and provide understanding of why the drop in performance is so much. Having a row for baseline would be very helpful too.
    3. Weaknesses. Clearly explain why these are weak aspects of the paper, e.g., why a specific prior work has already demonstrated the key contributions, why the experiments are insufficient to validate the claims, etc.
        - Image 1 seems irrelevant to the paper. The paper has no discussion of generative models like ChatGPT-4o and focuses on CLIP style models.
        - The observation that VLMs do not correctly work with negations is not novel for a technical contribution. Previous works [1,2] have shown that VLMs do not really focus on the meaning of the caption and the current papers' observation is a simple inference from that. There is also existing work [3] which identifies the problem of not understanding negatives in VLMs. I think the paper should reword the contribution to highlight their deeper analysis and insights.
        - In Figure 4, it would be very helpful to understand the baseline accuracy, i.e. the the accuracy without any negation word. This would help contextualize the drop in performance.
        - The paper should describe how the distractor images are retrieved. This is talked about in the supplementary material but it is important enough to be in the main paper. Otherwise, it makes it very hard to understand how these distractor images are found.
        - Nit: Please write an equation for computing M_ij referred in equation 1. I know it is described in lines 529-532.
        - Equation 1 refers to loss as L_i which I assume to be referring to i=1,2,3 for loss L1, L2, L3. However, i is also used on the right hand side of the equation in a different context. Please disambiguate the equation.



        References

        1. https://openaccess.thecvf.com/content/CVPR2024W/MMFM/papers/Xu_Benchmarking_Zero-Shot_Recognition_with_Vision-Language_Models_Challenges_on_Granularity_and_CVPRW_2024_paper.pdf, CVPRW 2024
        2. https://arxiv.org/pdf/2210.01936, ICLR 2023
        3. https://aclanthology.org/2021.blackboxnlp-1.27/
    4. Recommendation. Rate the paper as it stands now.
        Borderline Accept
    5. Justification. Be specific: What are the most critical factors in your rating?
        I think the paper tackles an important problem and provides an interesting solution. However, I have concerns as listed in the weaknesses.
    6. Best Paper Candidate
        Not award candidate
```