Data versioning + data pipelines = Data Lineage

Data versioning: Git like strutcure but not exactly git.
    This allows us to store petabytes of unstructured and strutcured data while minimizing the cost.
    File based versioning

Containerized Data Centric Pipelines: speeds up data processing while lowering compute costs
    kubernetes native approach supports any library/language

pachyderm handles the multi-threading in the code. Just upload the single threaded code and
pachyderm will make it multi-threaded.

In pachyderm, everything is tracked. 
Global IDs make it easy for teams to track the process.

Two concepts:
    1. Pachyderm Pipeline System (PPS)
    2. Pachyderm File System (PFS)

Datums: It defines how the input data is seen by the code.

Glob patterns: How datums are made.
how to select slecific files from the repo:
    /*.md : this will match all the top level markdown files
    /**test*.txt : This will match any text files with test in its name at any level.
    /* : this will match everything in its root directory.
    pachctl glob file <repo>@<branch>:<glob> to test glob on a repo

Pipeline:
    Pipeline is a pachyderm primitive that is responsible for reading data from a specified source, such as a pachderm repo, transforming it according to the pipeline config, and writing the result to a output repo

input: needs repo to read from and how to glob files into datums.
transform: the code that you want to run. Must include container image and the command to run.

pachyderm pipeline specification written in YAML for edge detection
pipeline:
    name: edges
    description: A pipeline that performs image edge detection
    input:
        pfs:    # this will be repo name from there we will take files to read
            glob: /*
            repo: images    this will be responsible to select all images as glob pattern is /*
    transform:
        cmd:
         -python3
         -/edges.py
        image: pachyderm/opencv # this is acts like docker image