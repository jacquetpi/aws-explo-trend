# aws-explo-trend

A dataset tracking the evolution of **AWS EC2 instance characteristics** from 2006 to present (last update November 2025). It combines resource specs (vCPUs, memory, etc.) with release dates so you can analyze how AWS compute has grown over time.

## Scope

The dataset merges two independent sources:

| Source | What it provides |
|--------|------------------|
| **Amazon EC2 instance type specs** | vCPUs, memory (GiB), processor, cores, threads, accelerator info per `instance_type`. Sourced from [AWS docs](https://docs.aws.amazon.com/ec2/latest/instancetypes/ec2-instance-type-specifications.html); CSVs live in `source_data/instances/`. |
| **[EC2 Timeline](https://github.com/nrollr/ec2-timeline)** | First release date per `instance_type`. Synced from [instancetyp.es/timeline.json](https://instancetyp.es/timeline.json); raw data in `source_data/timeline/`. |

The merge yields a single table suitable for longitudinal analysis: instance growth over time, scaling patterns, and shifts in instance families (general, compute, memory, storage, accelerator, HPC).

## Repository structure

```
aws-explo-trend/
├── dataset.csv          # Merged dataset (main artifact)
├── source_data/
│   ├── instances/       # EC2 spec CSVs by category (general, compute, memory, …)
│   └── timeline/        # timeline.json from EC2 Timeline
├── notebooks/
│   ├── investig.ipynb   # Build dataset + exploration & figures
│   └── figures/        # Generated plots (e.g. flavors over time)
├── README.md
└── LICENSE
```

## Dataset (`dataset.csv`)

One row per instance type. Main columns:

- **Identity:** `instance_type`, `uid`, `_index`
- **Timeline:** `release_month`, `release_year`, `date`
- **Specs:** `vCPUs`, `Memory (GiB)`, `Processor`, `CPU cores`, `Threads per core`, accelerator fields
- **Classification:** `instance_category`, `instance_generation`, `category` (flavor family used in this repo)

Load and filter by date or category to study trends (e.g. vCPU/memory growth, new families over time).

## Reproducing the dataset

1. Clone the repo and ensure `source_data/` is populated (instances CSVs + `timeline/timeline.json`).
2. To refresh the timeline:  
   `wget https://instancetyp.es/timeline.json -O source_data/timeline/timeline.json`
3. Run the notebook from `notebooks/`: open `investig.ipynb` and execute the “Generate dataset” section. It reads from `../source_data/`, merges timeline + instance specs, and can write `dataset.csv` at the repo root.

Instance spec CSVs in `source_data/instances/` are maintained manually from the [AWS instance type specifications](https://docs.aws.amazon.com/ec2/latest/instancetypes/ec2-instance-type-specifications.html); update them when AWS adds or changes instance types.

## Intent

To provide a **reproducible reference** for:

- EC2 instance evolution and release cadence
- Scaling patterns (vCPU/memory ratios, family mix over time)
- Architectural shifts in AWS compute (e.g. graviton, accelerators)

## License

MIT. See [LICENSE](LICENSE). Credits: [EC2 Timeline](https://github.com/nrollr/ec2-timeline) (Nicolas Rollier), instance data from AWS documentation, this repo (Pierre Jacquet).
