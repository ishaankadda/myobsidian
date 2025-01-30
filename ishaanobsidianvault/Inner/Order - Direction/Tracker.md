#### Hungarian algorithm
1. figure out a pairwise cost matrix between the detections of each frame. this is based on distance, confidence and other similarity measures.
2. Once the cost matrix is obtained, figure out which boxes should be matched with each other.




#### Current trackers used:
1. **kalman filter**(on coordinates) + **IoU based matching** 
2. **ByteTrack**

# New algorithms
1. **ByteTrackV2** - [ByteTrackV2: 2D and 3D multi-object tracking by associating every detection box](https://scholar.google.com/scholar?cluster=60617770397077178&hl=en&oi=scholarr)


# Benchmarks
#### 1. MOT Challenge - MOT20
https://motchallenge.net/results/MOT20/
- ByteTrack (`kalman_pub`) is quite high up on this leaderboard. 
- SUSHI is not good. In general, large differences between submissions on leaderboard despite numerous submissions.
- Detailed performances available for each algo on various MOT20 samples.




# Progress with ByteTrack implementation
###### ~~**PRIORITY: decode lap.lapjv solver**~~! -> <span class='blue'>solved</span>
**figure out exact alternative to lap.lapjv after understanding what exactly is meant when an unassigned track is reported. is it possible to report a track or a detection as unassigned if everything in it's row/column at the infinite cost? figure this stuff out.**
<span class="red">
SOLUTION:
I have figured this out. The code returns all the values that dont have an assignment. Basically, everything that is not assigned is returned as unassigned. For example:
</span>
```
import lap
import numpy as np

cost_matrix = np.array([
    [np.inf, np.inf, np.inf, 8,    np.inf, 2,    np.inf, np.inf, np.inf],
    [np.inf, np.inf, np.inf, 4,    np.inf, 6,    np.inf, np.inf, np.inf],
    [np.inf, np.inf, np.inf, np.inf, np.inf, np.inf, np.inf, np.inf, np.inf],
    [np.inf, np.inf, np.inf, np.inf, np.inf, np.inf, np.inf, np.inf, np.inf],
    [np.inf, np.inf, np.inf, np.inf, np.inf, np.inf, np.inf, np.inf, np.inf],
    [np.inf, np.inf, np.inf, np.inf, np.inf, np.inf, np.inf, np.inf, np.inf],
    [np.inf, np.inf, np.inf, np.inf, np.inf, np.inf, np.inf, np.inf, np.inf]
])

def linear_assignment(cost_matrix, thresh):
    if cost_matrix.size == 0:
        return np.empty((0, 2), dtype=int), tuple(range(cost_matrix.shape[0])), tuple(range(cost_matrix.shape[1]))
    matches, unmatched_a, unmatched_b = [], [], []
    cost, x, y = lap.lapjv(cost_matrix, extend_cost=True, cost_limit=thresh)
    for ix, mx in enumerate(x):
        if mx >= 0:
            matches.append([ix, mx])
    unmatched_a = np.where(x < 0)[0]
    unmatched_b = np.where(y < 0)[0]
    matches = np.asarray(matches)
    return matches, unmatched_a, unmatched_b


####################################
#### So the ByteTrack code will work something like this:
####################################

matches, u_tracks, u_detections = linear_assignment(cost_matrix, 4)
>>> matches
[[0, 5], [1, 3]]
>>> u_tracks
[2, 3, 4, 5, 6]
>>> u_detections
[0, 1, 2, 4, 6, 7, 8]

```

###### ~~**Next Step: implement lap.lapjv in Yagnesh style**~~ -> <span class='blue'>solved</span>
You have to figure out a similar implementation of the linear assignment problem solver in C++ and then use it instead.
<span class='red'>SOLVED:
The solution involves two augmentations to the optimal problem solver.
1. compute the sum of all the cost matrix elements. add 1.0 to this and call it INF_COST.
3. assign each elements of the input Cost matrix equal to INF_COST if they meet the criteria Cost[i, j] >= thresh
4. if the optimal assignment has any elements equal to INF_COST then remove them from the assignment and subtract their associated cost.
</span>

###### ~~**Next Step: continue rest of ByteTrack algorithm with new solver ->~~ <span class='yellow'>ongoing</span>**

# PEHLA STEP: make tracker work (purana wala)

## DOOSRA STEP: make tracker with IoU assignment work (minimum edit)

###### **Next Step: move the new track id function back inside `mtx::kalman_tracker::track()` and remove the edits (save them inside here)**

###### Next step: figure out `if-else` new flow to `mtx::kalman_tracker::track`
###### Next step: addd back the new id track edits after confirming with shantanu


###### **FINAL STEPS: profile with and without mutex lock ->** <span class='red'>not started</span>
I have modified the code for `mtx::kalman_tracker::get_new_track_id`. However, ChatGPT advises me to avoid this and use an atomic approach in case there is massive concurrency (1000s of requests per second). Ablations need to be run on this to ensure it is not too much of a performance bottleneck.

Chat with GPT-4o on this issue: [chat link](https://chatgpt.com/share/67962386-8d88-8009-b32d-3c4111a26196)
Code for `mtx::kalman_tracker::get_new_track_id`:
```
long kalman_tracker::get_new_track_id() {
    long new_id;

    // Create a unique_lock on m_id_mutex. This guarantees
    // that only one thread at a time is in this critical section.
    std::unique_lock<std::mutex> lock(m_assigned_id_mutex);

    // now safely read/modify the m_assigned_id
    if (this->m_assigned_id < MAX_OBJ_ID){ // find a new id for the track
        new_id = ++this->m_assigned_id;
    } else {
        this->m_assigned_id = 0;
        new_id = ++this->m_assigned_id;
    }

    // release the lock 
    // (apparently this is not necessary, c++ already does it implicitly somehow)
    lock.unlock();

    return new_id;
```




###### **IMPORTANT NOTES (REVISE)**:
1. I have split the assignment step, make sure everything is dealt with.
2. I have split the kalman update inside the first assignment step, make sure that there is an equivalent for creating the new blobs.
4. **DOUBT:** is the second kalman update in the first association step equivalent to the pytorch code from ByteTrack>? basically are the reactivations being done correctly?
5. **BACKLOG**: APS.Solve must be replaced with it's equivalent that incorporates the threshold. This equivalent must make sure that no assignments are used that are assigned infinite score. refer to:
	1. `https://chatgpt.com/share/67936040-cda4-8009-99e8-1e45164ff7ad`
	2. `https://github.com/gatagat/lap/issues/32`
	<span class="red">Solution 1 (implemented): I have replaced </span>
	