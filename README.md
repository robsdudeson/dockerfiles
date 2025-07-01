# dockerfiles

A Collection of hand rolled Dockerfiles for various projects

## Usage

To build the `ruby/2.3.6` Docker image, use the `build` script from the `.dockerfiles/scripts` directory, providing the path to the `ruby/2.3.6` context:

```bash
./.dockerfiles/scripts/build .dockerfiles/ruby/2.3.6
```

After building, you can run a container from this image:

```bash
docker run -it ruby:2.3.6 /bin/bash
```

## Directory Structure

The relevant directory structure for `ruby/2.3.6` is as follows:

```
.dockerfiles/
├── ruby/
│   └── 2.3.6/
│       ├── Dockerfile
│       ├── .env
│       └── id (generated after successful build)
└── README.md
```
This structure indicates that the `Dockerfile`, any associated environment files (`.env`), and the generated `id` file for Ruby version 2.3.6 are located within the `.dockerfiles/ruby/2.3.6` subdirectory.
