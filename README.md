BinTuner
---------------------
BinTuner is a cost-efficient auto-tuning framework, which can deliver a near-optimal binary code that reveals much more differences than -Ox settings. it also can assist the binary code analysis research in generating more diversified datasets for training and testing.
The BinTuner framework is based on OpenTuner, thanks to all contributors for their contributions.

April 4, 2022

We have tested that BinTuner supports the latest GCC 11.2.0.

```
gcc-11 -m64 -std=gnu89 -fgnu89-inline  -DSPEC_CPU -DNDEBUG    401/spec.c 401/blocksort.c 401/bzip2.c 401/bzlib.c 401/compress.c 401/crctable.c 401/decompress.c 401/huffman.c 401/randtable.c -lm -o ./tmp0.bin -O3 -fno-auto-inc-dec -fbranch-count-reg -fcombine-stack-adjustments -fno-compare-elim -fno-cprop-registers -fno-dce -fdefer-pop -fno-delayed-branch -fno-dse -fno-forward-propagate -fno-guess-branch-probability -fif-conversion2 -fno-if-conversion -fno-inline-functions-called-once -fno-ipa-pure-const -fipa-profile -fno-ipa-reference -fmerge-constants -fno-move-loop-invariants -fno-reorder-blocks -fshrink-wrap -fsplit-wide-types -ftree-bit-ccp -ftree-ccp -ftree-ch -fno-tree-coalesce-vars -ftree-copy-prop -fno-tree-dce -fno-tree-dse -fno-tree-forwprop -fno-tree-fre -fno-tree-sink -ftree-slsr -fno-tree-sra -fno-tree-pta -ftree-ter -funit-at-a-time -fomit-frame-pointer -fno-tree-phiprop -ftree-dominator-opts -fno-ssa-backprop -fssa-phiopt -fshrink-wrap-separate -fno-thread-jumps -falign-functions -fno-align-labels -falign-loops -fno-caller-saves -fno-crossjumping -fcse-follow-jumps -fno-cse-skip-blocks -fdelete-null-pointer-checks -fno-devirtualize -fdevirtualize-speculatively -fexpensive-optimizations -fgcse -fgcse-lm -fno-hoist-adjacent-loads -finline-small-functions -findirect-inlining -fno-ipa-cp -fno-ipa-sra -fipa-icf -fno-isolate-erroneous-paths-dereference -flra-remat -foptimize-sibling-calls -fno-optimize-strlen -fno-partial-inlining -fno-peephole2 -freorder-blocks-and-partition -freorder-functions -fno-rerun-cse-after-loop -fsched-interblock -fsched-spec -fschedule-insns -fno-strict-aliasing -fstrict-overflow -ftree-builtin-call-dce -fno-tree-switch-conversion -ftree-tail-merge -fno-tree-pre -ftree-vrp -fipa-ra -fschedule-insns2 -fcode-hoisting -fno-store-merging -fno-ipa-bit-cp -fipa-vrp -fno-inline-functions -funswitch-loops -fno-predictive-commoning -fgcse-after-reload -ftree-loop-vectorize -ftree-loop-distribute-patterns -fno-tree-slp-vectorize -ftree-partial-pre -fno-peel-loops -fipa-cp-clone -fsplit-paths -ftree-vectorize -faggressive-loop-optimizations -falign-jumps -fallow-store-data-races -fno-associative-math -fassume-phsa -fno-asynchronous-unwind-tables -fno-bit-tests -fno-branch-probabilities -fconserve-stack -fno-cx-limited-range -fno-delete-dead-exceptions -fexceptions -ffast-math -fno-finite-loops -fno-finite-math-only -fno-float-store -ffp-int-builtin-inexact -ffunction-cse -fgcse-las -fno-gcse-sm -fgraphite -fgraphite-identity -finline -fno-inline-atomics -fipa-icf-functions -fno-ipa-icf-variables -fipa-modref -fipa-reference-addressable -fno-ipa-stack-alignment --param gcse-cost-distance-ratio=66 --param iv-max-considered-uses=211

```


The architecture of BinTuner:
---------------------
![image](https://github.com/BinTuner/Dev/blob/main/Results/Images/BinTuner.png)



The core on the server-side is a metaheuristic search engine (e.g., the genetic algorithm), which directs iterative compilation towards maximizing the effect of binary code differences. 

The client-side runs different compilers (GCC, LLVM ...) and the calculation of the fitness function. 

Both sides communicate valid optimization options, fitness function scores, and compiled binaries to each other, and these data are stored in a database for future exploration. When BinTuner reaches a termination condition, we select the iterations showing the highest fitness function score and output the corresponding binary code as the final outcomes.

Citation
--------------------
If you find our work helpful to your research, please consider citing our [paper](https://dl.acm.org/doi/abs/10.1145/3453483.3454035).

```
@inproceedings{ren2021unleashing,
  title={Unleashing the hidden power of compiler optimization on binary code difference: An empirical study},
  author={Ren, Xiaolei and Ho, Michael and Ming, Jiang and Lei, Yu and Li, Li},
  booktitle={Proceedings of the 42nd ACM SIGPLAN International Conference on Programming Language Design and Implementation},
  pages={142--157},
  year={2021}
}
```


System dependencies
--------------------

A list of system dependencies can be found in [packages-deps](https://github.com/BinTuner/Dev/blob/main/packages-deps) which are primarily python 2.6+ (not 3.x) and sqlite3.

On Ubuntu/Debian there can be installed with:
```
sudo apt-get update
sudo apt-get upgrade
sudo apt-get install `cat packages-deps | tr '\n' ' '`
```
Installation
--------------------

Note: do all installations in a virtual env and you will have to use python2. 

```
pip install virtualenv
python -m virtualenv bintuner_venv
source bintuner_venv/bin/activate
```

Running it out of a git checkout, a list of python dependencies can be found in requirements.txt these can be installed system-wide with pip.
```
sudo apt-get install python-pip
sudo pip install -r requirements.txt
```

If you encounter an error message like this:

```
Could not find a version that satisfies the requirement fn>=0.2.12 (from -r requirements.txt (line 2)) (from versions:)
No matching distribution found for fn>=0.2.12 (from -r requirements.tet (line 2))
```
Please try again or install each manually

```
pip install fn>=0.2.12
...
pip install numpy>=1.8.0
...
```

If you encounter an error message like this:

```
ImportError: No module named lzma
```
Please install lzma
```
sudo apt-get install python-lzma
```
If you encounter an error message like this:
```
assert compile_result['returncode'] == 0
AssertionError
```
Please confirm how to use the compiler in your terminal, such as GCC or gcc-10.2.0
it needs to be modified in your .Py file


If you encounter an error message like this:
```
sqlalchemy.exc.OperationalError: (pysqlite2.dbapi2.OperationalError) database is locked [SQL: u'INSERT INTO tuning_run (uuid, program_version_id, machine_class_id, input_class_id, name, args, objective, state, start_date, end_date, final_config_id) VALUES (?, ?, ?, ?, ?, ?, ?, ?, ?, ?, ?)'] [parameters: ('b3311f3609ff4ce9aa40c0f9bb291d26', 1, None, None, 'unnamed', <read-only buffer for 0x556266495ab0, size -1, offset 0 at 0x7fcca77b67b0>, <read-only buffer for 0x7fcca781b420, size -1, offset 0 at 0x7fcca77b6570>, 'QUEUED', '2021-xx-xx 03:42:04.145932', None, None)] (Background on this error at: http://sqlalche.me/e/e3q8)
```
Just delete the DB file saved before (PATH:/examples/gccflags/opentuner.db/Your PC's Name.db).

z3-solver

I am using z3-solver 4.8.14.0 (pip install z3-solver), it works well.

Install Compiler
--------------------
GCC

Check to see if the compiler is installed

e.g. 
```
gcc -v  shows that
gcc version 11.2.0 (Ubuntu 18.04)
```

Please note that there have different optimization options in different versions of compilers. 

If you use the optimization options that are not included in this version of the compiler, the program can not run and report an error.

It is strongly recommended to confirm that the optimization options are in the official instructions of GCC or LLVM before using them.

e.g.
[GCC version 11.2.0](https://gcc.gnu.org/onlinedocs/gcc-11.2.0/gcc/Optimize-Options.html#Optimize-Options).

You can also use the command to display all options in terminal
```
gcc --help=optimizers


The following options control optimizations:
  -O<number>                  Set optimization level to <number>.
  -Ofast                      Optimize for speed disregarding exact standards
                              compliance.
  -Og                         Optimize for debugging experience rather than
                              speed or size.
  -Os                         Optimize for space rather than speed.
  -faggressive-loop-optimizations Aggressively optimize loops using language
                              constraints.
  -falign-functions           Align the start of functions.
  -falign-jumps               Align labels which are only reached by jumping.
  -falign-labels              Align all labels.
  -falign-loops               Align the start of loops.
  ...

```
LLVM

```
clang -v
```


Check how to install LLVM here 

https://apt.llvm.org/    

https://clang.llvm.org/get_started.html


Checking Installation
--------------------

Enter the following command in terminal to test:
```
eg@xx:~/BinTuner/examples/gccflags$ python main.py 10
```

You will see some info like this:
```
Program Start
************************ Z3 ************************
5- Result--> Unavailable
3- Result--> Available
[ Z3 return Results = first second True four False]
[ Changed "shrink-wrap" value ]
...
-------------------------------------------------

--- BinTuner ---
--- Command lines and compiler optimization options ---:
gcc benchmarks/bzip2.c -lm -o ./tmp0.bin -O3 -fauto-inc-dec -fbranch-count-reg -fno-combine-stack-adjustments 
-fcompare-elim -fcprop-registers -fno-dce -fdefer-pop -fdelayed-branch -fno-dse -fforward-propagate -fguess-branch-probability 
-fno-if-conversion2 -fno-if-conversion -finline-functions-called-once -fipa-pure-const -fno-ipa-profile -fipa-reference 
-fno-merge-constants -fmove-loop-invariants -freorder-blocks -fshrink-wrap -fsplit-wide-types -fno-tree-bit-ccp -fno-tree-ccp 
-ftree-ch -fno-tree-coalesce-vars -ftree-copy-prop -ftree-dce -fno-tree-dse -ftree-forwprop -fno-tree-fre -ftree-sink -fno-tree-slsr 
-fno-tree-sra -ftree-pta -ftree-ter -fno-unit-at-a-time -fno-omit-frame-pointer -ftree-phiprop -fno-tree-dominator-opts -fno-ssa-backprop 
-fno-ssa-phiopt -fshrink-wrap-separate -fthread-jumps -falign-functions -fno-align-labels -fno-align-labels -falign-loops -fno-caller-saves 
-fno-crossjumping -fcse-follow-jumps -fno-cse-skip-blocks -fno-delete-null-pointer-checks -fno-devirtualize -fdevirtualize-speculatively 
-fexpensive-optimizations -fno-gcse -fno-gcse-lm -fno-hoist-adjacent-loads -finline-small-functions -fno-indirect-inlining -fipa-cp 
-fipa-sra -fipa-icf -fno-isolate-erroneous-paths-dereference -fno-lra-remat -foptimize-sibling-calls -foptimize-strlen 
-fpartial-inlining -fno-peephole2 -fno-reorder-blocks-and-partition -fno-reorder-functions -frerun-cse-after-loop -fno-sched-interblock 
-fno-sched-spec -fno-schedule-insns -fno-strict-aliasing -fstrict-overflow -fno-tree-builtin-call-dce -fno-tree-switch-conversion 
-ftree-tail-merge -ftree-pre -fno-tree-vrp -fno-ipa-ra -freorder-blocks -fno-schedule-insns2 -fcode-hoisting -fstore-merging 
-freorder-blocks-algorithm=simple -fipa-bit-cp -fipa-vrp -fno-inline-functions -fno-unswitch-loops -fpredictive-commoning 
-fno-gcse-after-reload -fno-tree-loop-vectorize -ftree-loop-distribute-patterns -fno-tree-slp-vectorize -fvect-cost-model 
-ftree-partial-pre -fpeel-loops -fipa-cp-clone -fno-split-paths -ftree-vectorize --param early-inlining-insns=526 
--param gcse-cost-distance-ratio=12 --param iv-max-considered-uses=762
 -O3
--NCD:0.807842390787
---Test----
--Max:0
--Current:0
--Count:0
...
```






[//]: <> (Checking Installation)


[//]: <> (Paper)


Results
----------

The DB file saved in the PATH:/examples/gccflags/opentuner.db/Your PC's Name.db

Each sequence of compilation flags and the corresponding ncd value are saved in the db file. 

Set up how many times to run
----------
Please refer to the settings in main.py
There are two strategies
The default setting runs 100 times, if you want to modify it according to your own wishes this is ok.
For example, by monitoring the change of NCD value in 10 times, if the cumulative change of 100 times increase is less than 5%, let's terminte it.

First-order formulas
--------------------

We manually generate first-order formulas after understanding the compiler manual. The knowledge we learned is easy to move between the same compiler series---we only need to consider the different optimization options introduced by the new version. 

We use Z3 Prover to analyze all generated optimization option sequences for conflicts and make changes to conflicting options for greater compiling success.

For more details, please refer  [Z3Prover](https://github.com/BinTuner/Dev/blob/main/BinTuner/opentuner/search/Z3Prover.py).

Setting for Genetic Algorithm
--------------------
The genetic algorithm is a metaheuristic inspired by the process of natural selection that belongs to the larger class of evolutionary algorithms. Genetic algorithms are commonly used to generate high-quality solutions to optimization and search problems by relying on biologically inspired operators such as mutation, crossover, and selection.

We tune four parameters for the genetic algorithm, including `mutation_rate`,  `crossover_rate`, `must_mutate_count`, `crossover_strength`.  

For more details, please refer [globalGA](https://github.com/BinTuner/Dev/blob/main/BinTuner/opentuner/search/globalGA.py).

Future Work
--------------------
We are studying constructing custom optimization sequences that present the best tradeoffs between multiple objective functions (e.g., execution speed, energy consumption, and obfuscation). To further reduce the total iterations of BinTuner, an exciting direction is to develop machine learning methods that correlate C language features with particular optimization options. In this way, we can predict program-specific optimization strategies that achieve the expected binary code differences.

Contact
--------------------
If you experience any issues during installing or using the software, or if you want to contribute to BinTuner, please feel free to reach out to us either by creating issues on GitHub or sending emails to xiaolei.ren@mavs.uta.edu.


