### Specify which CPU core to use
use `taskset -c 1 <command>` instead of `<command>` to fix the CPU being assigned to run the script. 

### Concurrent scripts
Be careful to use a CPU which is not assigned to any other training process or such. Check if there are any trainings running on that master node, even if your benchmarking is on CPU. 
