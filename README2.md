# AUM-HPU-HSI-Research
Modified DeepHyperX toolbox including the threeLayer Model

## Buidling the container 

In order to run the code you need to build a container using one of the two definition files located in the ```definition_files``` folder. 
  1) The definition file ```test_computeNode.def``` is for building a container in a node **without** internet connection. Use:
     
      ```apptainer build test_computeNode.sif test_computeNode.def```

  2) The definition file ```internetAccessRequired.def``` is for building a container in a node **with** internet connection. Use: 

      ```apptainer build internetAccessRequired.sif internetAccessRequired.def```

## Running the container / code 

In order to run the container using a batch file:
  1) To run in a CPU node from the Login node using the command line:
     
     ```srun --partition=cs --nodes=1 nohup apptainer exec --bind ./data:/mnt,./:/images test_computeNode.sif conda run -n myenv python /workspace/main.py --model nn --dataset IndianPines --training_sample 0.95 --runs 1 > nn_1_IndianPines_0.95.txt```
    
  2) To run in a CPU node use the script ```cpu_batchFile.sh``` (located in the folder ```batch_files```):
     
     For each model and dataset the script needs to be properly modified like so:
     
     ```apptainer exec --bind ./data:/mnt,./:/images test_computeNode.sif conda run -n myenv python /workspace/main.py --model nn --dataset IndianPines --training_sample 0.95 --runs 1 > cpu_nn_1_IndianPines_0.95.txt```

     Then, run this in the command line:
     Select the appropriate cpu nodes for each batch file:
    
     ```sbatch --nodelist=cnode2 cpu_batchFile.sh```
    
  3) To run in a GPU node use the script ```gpu_batchFile.sh``` (located in the folder ```batch_files```):
     
     For each model and dataset the script needs to be properly modified like so:
     
     ```apptainer exec --nv --bind ./data:/mnt,./:/images test_computeNode.sif conda run -n myenv python /workspace/main.py --model nn --dataset IndianPines --training_sample 0.95 --runs 1 --cuda 0 > gpu_nn_1_IndianPines_0.95.txt```

     Then, run this in the command line:
     Select the appropriate gpu nodes for each batch file:
    
     ```sbatch --nodelist=thor gpu_batchFile.sh```
  


  
