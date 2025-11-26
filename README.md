# aws-explo-trend

Dataset tracking the evolution of AWS EC2 instance characteristics from 2006 to the last update in November 2025.  
Focus: resource counts associated with each instance type and their chronological introduction.

## Scope

The dataset merges two independent sources:

1. **Amazon EC2 Instance Types documentation**  
   Extracted attributes: core count, memory size, and other resource indicators per `instance_type`.

2. **EC2 Timeline ([github.com/nrollr/ec2-timeline](https://github.com/nrollr/ec2-timeline))**  
   Extracted attribute: first release date for each `instance_type`.

The merge yields a historical trend line of EC2 flavor growth, enabling longitudinal analysis of AWS compute expansion.

## Structure

- `dataset.csv`  
  Consolidated dataset at the root, merging all sources.

- `source_data/`  
  Original raw sources used to build the dataset.

- `notebooks/`  
  Jupyter notebook for exploration and also for reproducing the merge process that constitutes `dataset.csv`.

## Intent

Provide a reproducible reference for analyzing EC2 instance evolution, scaling patterns, and architectural shifts across the AWS timeline.
