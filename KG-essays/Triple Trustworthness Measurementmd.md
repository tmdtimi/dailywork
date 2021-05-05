# Triple Trustworthness Measurement
========================================================================

**Abstract**
The Knowledge graph (KG) uses the triples to describe the facts in the real world. It has been widely used in intelligent analysis and applications.However, possible noises and conflicts are inevitably introduced in the process of constructing.And the KG based tasks or applications assume that the knowledge in the KG is completely correct and inevitably bring about potential deviations.

In this paper, we establish a knowledge graph triple trustworthiness measurement model that quantify their semantic correctness and
the true degree of the facts expressed. The model is a crisscrossing neural network structure. It synthesizes the internal semantic
information in the triples and the global inference information of
the KG to achieve the trustworthiness measurement and fusion in
the three levels of entity level, relationship level, and KG global
level. We analyzed the validity of the model output confidence values, and conducted experiments in the real-world dataset FB15K
(from Freebase) for the knowledge graph error detection task. The
experimental results showed that compared with other models, our
model achieved significant and consistent improvements.


--------------------------------------------------------------------

**triple trustworthiness measurement model**

KGTtm[22]提出了一个交错的神经网络，最上层是三种不同的置信度评估单元，这些评估器单元的输出形成了低级融合装置的输入。融合器是一个多层感知机，最终为每个三元组输出最终的可信度。	


![Alt pic] (https://github.com/tmdtimi/repo_pic/blob/master/Triple%20Trustworthiness%20Measurement%20for%20Knowledge%20Graph/QQ%E6%88%AA%E5%9B%BE20210505123949.png)




