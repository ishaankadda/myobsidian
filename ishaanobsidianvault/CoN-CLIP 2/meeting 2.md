number of samples is easy to drop
diffusion is going to be weird
increase the performance gaps - method should clearly disadvantage simple finetuning -> only the negation based hard negatives the same way as negclip's hard negatives.

Jaisidh: have grad cam for CLIP. find regions for where certain text appears. what we can do:
	imagine there is a sample $i, c, c'$ with a concept $o$ removed from $c$ to make $c'$.
	1. compute mask $m$ which is gradcam of $i$ with $o$.
	2. compute mask $m'$ which is gradcam of $i$ with $c'$.

Jai: two ways of proceeding
1. develop a negation understanding stable diffusion. it will solidify our claims and we will not need any other contribution.
2. edge out over $CLIP-FT$.


---

