# nvprof - Python tools for NVIDIA Profiler

Tools to help working with nvprof SQLite files, specifically for profiling
scripts to train deep learning models. The files can be big and thus slow to scp and work with in NVVP. This tool is aimed in extracting the small bits of important information and make profiling in NVVP faster.

- Author: Bohumír Zámečník <bohumir.zamecnik@gmail.com>, [Rossum](https://rossum.ai)
- License: MIT

## Installing

Install package `nvprof`:

```
# for development
$ pip install -e .
```

## Features

```
$ nvprof_tools --help
usage: nvprof_tools [-h] {info,truncate} ...

NVIDIA Profiler tools

positional arguments:
  {info,truncate}

optional arguments:
  -h, --help       show this help message and exit
```

### Summary about the file

It can show:

- total time (can be used to decide which time slice to take in nvvp)
- number of events in the tables sorted from highest

```
$ nvprof_tools info foo.sqlite
Total time: 3.300 sec
Total number of events: 516874
Events by table:
CUPTI_ACTIVITY_KIND_RUNTIME : 348080
CUPTI_ACTIVITY_KIND_CONCURRENT_KERNEL : 63792
CUPTI_ACTIVITY_KIND_DRIVER : 48279
CUPTI_ACTIVITY_KIND_SYNCHRONIZATION : 19741
CUPTI_ACTIVITY_KIND_CUDA_EVENT : 17860
CUPTI_ACTIVITY_KIND_MEMCPY : 15974
CUPTI_ACTIVITY_KIND_MEMSET : 2816
CUPTI_ACTIVITY_KIND_OVERHEAD : 309
CUPTI_ACTIVITY_KIND_STREAM : 12
CUPTI_ACTIVITY_KIND_DEVICE_ATTRIBUTE : 8
CUPTI_ACTIVITY_KIND_NAME : 1
CUPTI_ACTIVITY_KIND_DEVICE : 1
CUPTI_ACTIVITY_KIND_CONTEXT : 1
```

### Remove unnecessary events

Typically 80% of the events are runtime/driver CUDA calls, which are not essential for profiling deep learning scripts. Let's remove them.

NOTE: It will overwrite the input file.

```
$ nvprof_tools truncate foo.sqlite
```

Eg. we shrinked a database from 29 MB to 8 MB.
