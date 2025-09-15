# Artifact description
This is the artifact of submission #2793 (**Large Language Model based Functional Symbolic Execution**) in FSE 2026. 

The artifact is provided as a docker image, which contains the prototype of the proposed method and benchmarks used for evaluation. The aim is to assist the reviewers in reproducing the experimental results in our evaluation.

# Prerequisites
+ Ubuntu: We have validated the artifact on Ubuntu system
+ [Docker](https://www.docker.com/pricing/)

# Running the artifact

Firstly, pull the already prebuilt docker image from [docker hub](https://hub.docker.com/r/fuse25/fuse).Please make sure its name is `fuse25/fuse`.
```sh
$ sudo docker pull fuse25/fuse:latest
```

If everything is ok, a `fuse25/fuse` image should be found in the images listed by `docker images` command. Then, you can create a container of such image and start a Bash session using the following command. An interactive `bash` shell on the container is also executed at the moment.
```sh
$ sudo docker run -it fuse25/fuse:latest bash
```

If all goes well, the container should be running successfully. Otherwise, you can seek help from [Docker Doc](https://docs.docker.com/) if needed. 

Now, navigate into the directory containing our experimental environment and list the contents. 
```sh
$ cd /home/aaa/LLMPromotedSE/ && ls
LLMPromotedSEPaper/  UTBotCpp/  utbotc.sh  Miniconda3-latest-Linux-x86_64.sh  UTBotJava/
```
The following sections describe the application of the method in different languages.

# Reproducing experimental results for C/C++

The C/C++ part of the experiment is run under the UTBotCpp folder. Navigate into the directory containing our experimental environment and list the contents. 
```sh
$ cd UTBotCpp/ && ls
benchmarkCCpp/     realworld/                  utbot_cli.sh        version.txt
clion_plugin.zip   run_utbot.sh                utbot_distr
humaneval_cpp/     unpack_and_run_utbot.sh     utbot_distr.tar.gz
logic_bombs/       utbot-release-2024.3.0.zip  utbot_plugin.vsix
```

The `benchmarkCCpp/` folder is the directory where the experiments are run on benchmark.
The `logic_bombs/` and `humaneval_cpp/` folders are the benchmark for C and C++. 
The `realworld/` folder is the directory where the experiments are run on real-world programs.
The `utbot_distr/` folder is the release version of utbotcpp 

C/C++ experiments must be run in a native environment. If you find yourself in a python virtual environment, you need to run the following command to exit.
```sh
conda deactivate
```
First we need to run utbotcpp to generate test cases for benchmark and real-world program.
```sh
./run_utbot.sh
```
The results of utbotcpp are stored in the `tests/` folder in the directory of the parsed program. For example, the file being analyzed is in `humaneval_cpp/CPP_2/`, the result named `code_dot_cpp_test.cpp` in `tests/` folder. And, for the file being analyzed as real-world program, such as `e_fmodf.c`, the result of utbotcpp is located in the `UTBotCpp/realworld/realworld/tests/` folder.
## benchmark:
Navigate to benchmark's experiment directory
```sh
$ cd benchmarkCCpp/
```
### run
To run the experiment on benchmark, you need to open the file `run.sh` and fill in your own api-key.
```sh
apikey="xxxxxxxxx-llm-api-xxxxxxxxx"
baseurl="xxxxxxxxx-llm-baseurl-xxxxxxxxx"
llmmodel="xxxxxxxxx-llm-model-xxxxxxxxx"
```
After that, run the `run.sh` file
```sh
./run.sh
```
### get result
The result is stored in the `result/` directory of the current directory. The directory structure is as follows.
```sh
count1_c.csv         count_c.txt         time_2_c_utfail.csv
count1.csv           count_result_c.csv  time_2_utfail.csv
count1_match_c.csv   count_result.csv    time_genllm_c.csv
count1_match.csv     count.txt           time_genllm.csv
count2_c.csv         time_1_c.csv        time_genllm_c_utfail.csv
count2.csv           time_1.csv          time_genllm_utfail.csv
count2_c_utfail.csv  time_2_c.csv        time_match_c.csv
count2_utfail.csv    time_2.csv          time_match.csv
```
The `count*` files in the directory represent the statistics for each of the three steps.

The `time*` files in the directory represent the statistics of time for each of the three steps.

For example, the `count1.csv` file means the numbers of new descreption generated in enhance 1 for each benchmark in `humaneval_cpp/`. The `count1_c.csv` file means the numbers of new descreption generated in enhance 1 for each benchmark in `logic_bombs/`.

And so on, `count_result(_c).cpp` is the result of main for each benchmarks. `count2(_c).cpp` is the result of enhance 2 for each benchmarks.`count1_match(_c).cpp` is the result that matched of enhance 1 for each benchmarks.

`count(_c).txt` is the statistical summary for step main.

The same is true for time statistics.

Finally, a new, annotated and enhanced testcase file is generated in the resulting folder where the original utbotcpp was generated. For example：
```sh
ls /home/aaa/LLMPromotedSE/UTBotCpp/humaneval_cpp/CPP_2/tests/
code_dot_cpp_test1.cpp  code_dot_cpp_test.cpp  code_dot_cpp_test.h  makefiles
```
The file `code_dot_cpp_test1.cpp` is the result of our method.
## real-world programs:
Navigate to realworld program's experiment directory
```sh
$ cd realworld/
```
### run
To run the experiment on real-world program, you need to open the file `run.sh` and fill in your own api-key.
```sh
apikey="xxxxxxxxx-llm-api-xxxxxxxxx"
baseurl="xxxxxxxxx-llm-baseurl-xxxxxxxxx"
llmmodel="xxxxxxxxx-llm-model-xxxxxxxxx"
filepath="./realworld/"
filename="e_fmodf.c"
```
After that, run the `run.sh` file
```sh
./run.sh
```
### get result
The result is stored in the `result/` directory of the directory named by the filename, such as `UTBotCpp/realworld/realworld/e_fmodf/result/`. The directory structure is as follows.
```sh
count1_c.csv        count2_c.csv        time_1_c.csv  time_genllm_c.csv
count1_match_c.csv  count_result_c.csv  time_2_c.csv  time_match_c.csv
```
The `count*` files in the directory represent the statistics for each of the three steps.

The `time*` files in the directory represent the statistics of time for each of the three steps.

For example, the `count1_c.csv` file means the numbers of new descreption generated in enhance 1 for the real-world program.

And so on, `count_result_c.cpp` is the result of main for each benchmarks. `count2_c.cpp` is the result of enhance 2 for each benchmarks.`count1_match_c.cpp` is the result that matched of enhance 1 for each benchmarks.

The same is true for time statistics.

Finally, a new, annotated and enhanced testcase file is generated in the resulting folder where the original utbotcpp was generated. For example：
```sh
ls /home/aaa/LLMPromotedSE/UTBotCpp/realworld/realworld/tests/
e_fmodf.c                e_powf_dot_c_test1.cpp  s_erfcf_dot_c_test1.cpp
e_fmodf_dot_c_test1.cpp  e_powf_dot_c_test.cpp   s_erfcf_dot_c_test.cpp
e_fmodf_dot_c_test.cpp   e_powf_dot_c_test.h     s_erfcf_dot_c_test.h
e_fmodf_dot_c_test.h     makefiles
e_powf.c                 s_erfcf.c

```
The files `e_powf_dot_c_test1.cpp`, `s_erfcf_dot_c_test1.cpp` and `e_fmodf_dot_c_test1.cpp` are the result of our method.
# Reproducing experimental results for Python

## Usage
In order to be able to use this tool, you need the "base_url", "api_key", and "model" in OpenAI format to call the large language model. In the experiment, we used gpt-4o and Qwen2.5.

## Run
### Analyzing benchmark

Since the default time to run SBFT Python Tool to generate test cases for a single file is 380s, and benchmark has a total of 164 files, the whole process takes about 20 hours.

You can run the following command to generate annotated test files for the benchmark used in the paper.
```
cd UTBotPython/exe
./run.sh <base_url> <api_key> <model>  "../responses" "../result/time.csv" "../result/statistical.csv"
```

`humaneval_program/pythonX_syb_test_condition.py` is the annotated testcases generated by our method.

`responses/*` is the conditions generated by the invocation of the large language model.

`result/time.csv`is the time to run each benchmark.The output format of CSV file is as follows:
```
name,LLM1,match,enhance1,enhance2
python0,7957.431203045417,3853.07669499889,3450.640444003511,28435.29871699866
python1,10694.883140036836,3672.4011070327833,59790.594808990136,28604.30741001619
python2,13381.850934005342,1240.2297239750624,11270.78557596542,132968.9901409438
...... many lines ......
```

`result/statistical.csv` is the annotated result of running each benchmark. The output format of CSV file is as follows:
```
name,testcases_count,conditions_count,annotation_success_count,annotation_fial_count,match_pre_post_count,match_pre_not_post_count,not_match_pre_count,run_time_out_count,run_error_count,compile_error_count,match_success_rate,annotation_success_rate,parse_cond_success_count_i,parse_cond_success_rate_i,tc2manycond,enhance1_count,enhance1_match_count,cond_coverage_count,cond_coverage_rate,enhance2_cond_count
python0,5,6,4,1,4,0,1,0,0,0,0.8,0.8,6,1.0,0,1,1,3,0.5,3
python1,7,6,4,3,1,3,3,0,0,0,0.14285714285714285,0.5714285714285714,6,1.0,1,5,4,2,0.3333333333333333,2
python2,2,3,2,0,2,0,0,0,0,0,1.0,1.0,3,1.0,0,0,0,1,0.3333333333333333,2
...... many lines ......
```

The CSV files (`*.csv`) give the supported data of Table 1, Fig. 11 and Fig. 12.

`experiment_results/*` is the result of five times we run GPT-4o and Qwen2.5, and the results of the experiments presented in the paper. 

### Analyzing real world programs
```
cd exe
./run_real_program.sh <base_url> <api_key> <model>
```
The format of the command line output is as follows: 
```
---------------analyzing real world program n_queen.py---------------
...... many lines ......
testcases_count: 16
condition_count: 6
match_count: 11
anno_count: 11
...... many lines ......
```
The command line output gives the supported data of Table 2.

# Reproducing experimental results for Java

## Run

Please choose the LLM to use and provide the URL and API of the LLM. 
First, enter the directory `UTBotJava/benchmarkJava/docker_run_env/`

### Benchmark

Since there are too many commands, we provide a simple script `run.sh`.

Firstly, you should run process.py to get the benchmark for Java from humaneval_java.jsonl. There is a example in process.sh, so you can do: 

> bash process.sh

The directory `./data/*` will be generated. And then you should choose which llm you want to use, and correct it in `run.sh`、`main.sh`、`enhance.sh`, they all like this:

```sh
llm="the llm you choose"
```

And then, you should choose the subdirctory or use default in **run.sh**.

```sh
# run.sh:3
for i in {subdirectoryList}; do
```

Now you can begin to run.
> bash run.sh

After all the processes have finished, in the subdirectory, you can find `checkJson/*`、`enahance1/*`、`enahance2/*`, which store the jsons from llm. And the results of these three processes are located in directories `llmCheckOut/*`, `enhance1Result/*`, and `enhance2Result/*`, respectively. Besides, we also provide the final testsuits in `testEnhance/*` for every file in benchmark.

> NOTE:
> The files associated with each benchmark start with a consistent prefix, such as "enhance1Result/Java_0.txt".

There is also a time statistics file in `timeStatic.txt`, which records the running time of each file in every process.

`data.csv` has overall average statistics for all the data. The output format is as follows:

``` csv
Lang. & No.,Time,LLM,benchmark_num,TC_se,TC_se.avg,SUB_llm,SUB_llm.avg,R_parse,R_parse.avg,Succ_m,Succ_m.avg,Anna_m,Anna_m.avg,Cov_fun,Cov_fun.avg,Aug1,Succ1,Aug2,Succ2,dupli,dupli.avg,se_times(s),se_time.avg(s),llm_time(s),llm_time.avg(s),enhance1_time(s),enhance1_time.avg(s),enhance2_time(s),enhance2_time.avg(s)
Java,5,gpt,164,1286,7.8415,1004,6.122,0.9542,0.948,0.5218,0.5559,0.6524,0.6853,0.5438,0.5482,564,0.4386,379,0.3775,0.1653,0.1519,0,0.0,3817.711,23.279,12912.175,78.733,9545.743,58.206
```

`gpt.csv` or `{llm_name}.csv` records every piece of data for each file. The output format is as follows:

``` csv
program_name,testcases_count,conditions_count,match_count,anno_count,match_rate,anno_rate,cond_cov_count,cond_cov_rate,pre_post_match_enhance1,enhance1_count,enhance_2_count,se_time,llm_time,enhacne1_time,enhance2_time
Java_0,11,5,4,6,0.364,0.545,4,0.8,7,7,3,0,17.157,81.059,22.061
```

### Real world programs

In general, all steps are consistent with the benchmark. Alternatively, you can execute each step individually. 
First, enter the directory `UTBotJava/benchmarkJava/trueProgram/graph`

``` sh
1. bash main.sh {testFileDir}
2. bash enhance 1
3. bash enhacne 2
4. python dataCollect.py -llm {the llm you choose} -times {the turn}
```