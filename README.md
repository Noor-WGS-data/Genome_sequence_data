# Genome_sequence_data

## Purpose

#### We created this github repository to help out folks who want to learn how to align raw short read GWS data to a reference genome. Our directions are targetted at folks who are new to working with sequence data and new to working on the cluster (as we are/were). Our goal was to put together a relatively straightforward explanation the steps needed to align short read sequence data - we aimed to give enough information on working in the terminal and using the cluster to get folks started. Of course, everyone will come across their own errors that will need investigating - but not matter how frustrating those errors can be, learning HOW to figure out what went wrong was pretty key to our own learning process. We hope this gives a good overview and grounding framework for beginners! We plan to be updating this reporsitory as best practices and softawre changes (as it does). 

The Workflow_Overview outlines the broad purpose of our project (what species we're using, how we wanted to use our final VCF) and also links to instructions for individual steps of the process. The first few steps also show how to run a shell script on the terminal, a few tips on using SLURM job arrays, etc, which should set you up for success in running the remaining steps.

We also have created a Tutorials folder, to which we will keep adding. This folder will include tips and tricks on different Unix commands, different programs that could be useful in working with or analyzing your final VCF, etc. 

Please remember that we are adding to this repository as things change and as we learn more, so this is very much a working repository!

**Note** We hope that these instructions cover a common enough practice that they can be broadly used (or at least similar code can be used!) across universities and research purposes. However, we should note that our short read data came from Genewiz and our code is designed to run on the Duke Computer Cluster (which uses SLURM Workload Manager).
