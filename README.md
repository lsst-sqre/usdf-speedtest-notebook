usdf-speedtest-notebook
-----------------------

This is just a little notebook to fetch a large (1.3GB) file from USDF S3 and discard it.

It uses `curl` in parallel mode to maximize transfer throughput, and also tests multiprocess concurrency (using an asyncio TaskGroup, each task of which spawns a process with asyncio.subprocess).

It writes its output to a shared directory, so once we have some data from a parallel test, we can write a different notebook to plot it.

This is designed to be run under mobu so we can do a parallel flock and, hopefully, saturate the network bandwidth to our Kubernetes nodes and determine what we can really pull from USDF.  Using gVNIC on the nodes, this has been surprisingly hard to do with a single instance.  The actual throughput rate is still increasing at 30 concurrent processes, and above that, the Pod falls over, because it's only got 4 CPU cores and 16GB of memory.
