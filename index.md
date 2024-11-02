---
title: Containers in Supercomputing Environment
author: CSC Training
---

{% assign items = site.hands-on |  sort: "title" %}

## 1. Introduction to Containers
### Slides: Brief Introduction to CSC HPC Environments
### Slides: Basics of Containers
### Slides: Tykky Wrapper Containerisation

###  Tutorials and exercises
{% for hands-on in items %}
{% if hands-on.topic == 'introduction' %}
1. [{{ hands-on.title }}]({{ hands-on.url | relative_url }})
{% endif %}
{% endfor %}

## 2. Introduction to Apptainer/Singularity containers
### Slides:  Introduction to Singularity/Apptainer
### Slides:  Building Apptainer/Singularity container images

###  Tutorials and exercises
{% for hands-on in items %}
{% if hands-on.topic == 'apptainer' %}
1. [{{ hands-on.title }}]({{ hands-on.url | relative_url }})
{% endif %}
{% endfor %}



## 3. Working with Docker Containers 
### Slides: [Basic introduction to Docker containers](https://a3s.fi/biocontainers2024/intro_docker_2024.html)
### Slides: [Applications with Docker containers](https://a3s.fi/biocontainers2024/containerised_applications_2024.html)

###  Tutorials and exercises
{% for hands-on in items %}
{% if hands-on.topic == 'docker' %}
1. [{{ hands-on.title }}]({{ hands-on.url | relative_url }})
{% endif %}
{% endfor %}

## 4. Containers in web applications and workflows
### Slides: [Rstudio and Jupyter Notebooks](https://a3s.fi/biocontainers2024/notebooks_2024.html)
### Slides: [Introduction to Bio-workflows](https://a3s.fi/biocontainers2024/workflows_csc_2024.html)
###  Tutorials and exercises
{% for hands-on in items %}
{% if hands-on.topic == 'containers' %}
1. [{{ hands-on.title }}]({{ hands-on.url | relative_url }})
{% endif %}
{% endfor %}
