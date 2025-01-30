
# Nov 6
- Do something for Vans bday
- Video call mom and dad
- Meditate
- Journal
- Start [[camera ready]] tasks, clear up any confusions
- Figure out the FD stuff and make one on whatever app
- Make your groww account
- watch 2 episodes of jojo (40 min) with food


# Nov 7
- Go gym in the morning


---





# violence todo

-> i3d repo action recognition setup kardo apne videos pe.
-> tejus: watch out for reply to google form to the weights wala thing

------

- try human skeleton tf setup.
- read i3d
- setup two-stream i3d
- maybe can merge human skeleton and i3d



# Today
1. Openpose process rwf2000 => Violence training via jupyter notebook
2. Attribute wala thing run it
3. `scp` backup everything
4. Start to make a summary of everything and the papers that are done in Violence on the board.

1. Implement efficientformer in fusenets
2. Jaisidh Advice
	```
		1. torch.compile everything before you run it. 1.5x speedup.
		2. amp + compile + autocast + num_workers
	```


# Today
### 1. work
1. 10:40 - write a script to do 5 - crops for a list of videos
2. 10:50 
	- run 5 - crop for a video from the video shifted to `datasets/compare_output/videos`
	- feature extract to `datasets/compare_outputs/features/viashin`
	- copy download features to `datasets/compare_outputs/features/download`
	- make `MTF_compare_viashin_Test.list` and `MTF_compare_download_Test.list`
	- run `python get_predict.py --root_dir /workspace/violence/datasets/compare_outputs/features/download --dataset MTF_compare_viashin` and then for `MTF_compare_download`. and then compare the resulting features. 
		```
		### experiment
		compare:
		1. reprod (viashin features)
		2. downloaded video features (xd vio)

		### methodology:
		run inference on this video using the two different kinds of features above. video path - ./datasets/XD_vio_test_data/videos/Skyfall.2012__#00-03-22_00-03-40_label_B6-0-0.mp4

		1. infer on downloaded video features:
			- get predictions (python get_predict.py --model_path ckpts/xd_best.pkl --root_dir /workspace/violence/datasets/i3d-features/combined_symlinks_rgb --dataset MTF_compare_download)
		2. infer on own extracted features:
			- split into 5-crops (python run1.py in ./pipeline)
			- features extracted (bash extract_features_compare_old.sh)
			- python get_predict.py --model_path ckpts/xd_best.pkl --root_dir /workspace/violence/datasets/compare_outputs/viashin/features --dataset MTF_compare_viashin

		### Results of comparison:
		reprod feature predictions were around 1.3 - 2.6 points higher than prediction on downloaded features. Both were in the range of 7.00 - 14.00.
		```

```
		### experiment 2
		compare:
		first 13 samples viashin
		vs
		first 13 samples download

		# reason
		tried to get video crops for all the videos but segfault on iter 14.

		# methodology
		modify get_predict.py to give the test scores on the sliced portions of the datasets as well.

		# results
		the ROC curve for first 10 samples matches quite well. given below:
		"download_13.png" vs "viashin_13.png" -> almost the same.
```
![[Pasted image 20240906144746.png]]


```
>>> # first video: last_minute_cropped_violence_vid1.mp4
>>> first.max(), first.min()
(5.631922, 1.8874228)
>>>
>>> # second video: nonviolenceoffice.mp4
>>> second.max(), second.min()
(3.2982297, 0.5212675)
>>>
>>> first.shape, second.shape
((39,), (174,))
```

### 2. other
- buy belt at logix
- fix charging samsung store
- wash towel
- ~~book shatabdi back home after discussing with bhaiyya~
- talk to mamu about personal finance
- "how can i pursue this research direction further?"
- read a book this weekend and then implement it

### Things done today (review)
1. put `scp` for the backup on tmux session

# General
1. go through [[Raunak - MnM meals 2]] and make sheet and implement everything. slowly. figure out how to
2. Go through quant-advice by aniruhdh and generate a document for what to do about it.
3. start to figure out how to make a plan chart for MS and PhDs abroad.



![[Pasted image 20240907182504.png]]

| Name                     | Value     |
| ------------------------ | --------- |
| benchmark_thread_count   | 2         |
| enable_censorship_checks | 0         |
| health_thread_count      | 40        |
| health_timeout           | 3.75      |
| hide_results             | 0         |
| input_source             | alexa     |
| num_servers              | 11        |
| ping_timeout             | 0.5       |
| query_count              | 250       |
| run_count                | 1         |
| select_mode              | automatic |
| template                 | html      |
| timeout                  | 3.5       |
| upload_results           | 0         |
| version                  | 1.3.1     |




### Today
- read `situational-awareness` and annotate on phone
- transfer and process the datasets for violence
- read about TCP, UDP and SCTP
- compute.hyper.space, play.hyper.space





# Journal
Reason, clearly, does not stand in control. The "elephant" works in mysterious ways that escape it, such as our inability to explain how the entire complex motion of moving a leg is achieved without the express formulation of the constituents of that movement. At best, the "rider" provides an objective and the actions or steps naturally follow from there. Reality is usually far more displeasing - most of what happens, happens without completely conscious thought or decision. Thus it can be said that whatever action takes place, serves a goal that is sometimes conscious and sometimes subconscious. How is it possible to trust a goal that 

I want to embody every archetypal image inside my head. Depraved, hungry, a desperate dog. A true agent. Shiva incarnate. Inverted spear of heaven. The enslaver of elephants.

I hate elephants.
I want to stop the elephant stomping on me. I want to rein in control. I want to make sense of these things. I want to do these things that seem to be impossible. That alone makes them worth pursuing.


I want to become the embodiment of action itself. Action comes with a feeling of desire, and I have felt enough. Now I must feel the way forward.

Fear causes the backward collapse into the known. However the key to ecstasy is in unifying ourselves with the unknown, to reject the individual variation in our own myriad of accumulated experience, and to be able to sit contently with it in acceptance of it, to not fear it, to let it pass through us.