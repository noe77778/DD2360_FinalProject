This is the final project for KTH DD2360HT22 course: Applied GPU programming.

In this repository, we revise the original k-nearest neighbor program in Rodinia Benchmark Suite for different data types.

There are four different data types: float, double, __half, __nv_bfloat16.

The execution environment is Google colab.

Execution steps:
1. Download the files from github
```
%cd /content/
!git clone https://github.com/noe77778/DD2360_FinalProject
```
2. Generate several data for testing. (You can also change the size of dataset.)

Usage example:
```
./hurricane_gen <num records> <num files>
```

```
%cd /content/DD2360_FinalProject/data/nn
!make hurricanegen
!./hurricanegen 327680 32
!./hurricanegen 171040 16
!./hurricanegen 85520 8
```
3. Initial profiling
```
%cd /content/DD2360_FinalProject/cuda/nn
!nvcc -arch=sm_75 nn_cuda.cu -o Original_code
```
Change ```nn_cuda.cu``` to ```float_DT.cu``` / ```double_DT.cu``` / ```half_DT.cu``` / ```bfloat16_DT.cu``` for different data types.
Also, change the output name for each data type. 

4. Execute programs to get the results
```
%%shell 
#!/bin/bash
PARA="r"
BOARDER="+++++++++++++++++++++++++++++++++++"
for FILE_NAME in "list84k_8" "list168k_16" "list320k_32" 
do
  for VARIABLE in 5 #500 50000 500000
  do
      echo $BOARDER $FILE_NAME $PARA $VARIABLE $BOARDER
      nvprof ./half_DT $FILE_NAME -r $VARIABLE -lat 30 -lng 90
      ncu ./half_DT $FILE_NAME -r $VARIABLE -lat 30 -lng 90
  done
done
```

Change your own input data file names in the first for loop, if needed. Also, change the ```half_DT``` name to your output file name in step 3. Change the -r, -lat, -lng to your desire parameters.

Results will show your input dataset name and the returned number of data in the format of:
+++++++++ FILE_NAME r RETURN_NUMBER +++++++

The first three rows show the time for host-to-device, kernel, and device-to-host with gettimeofday() function.

And the following will show the returned and profiling results analysized by nvprof.

You can also execute the Final_project_DD2360HT22.ipynb under this repository.

