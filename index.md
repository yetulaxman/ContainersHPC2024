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
### Slides: Basic introduction to Docker containers
### Slides: Conversion of Docker images to Apptainer image

###  Tutorials and exercises
{% for hands-on in items %}
{% if hands-on.topic == 'docker' %}
1. [{{ hands-on.title }}]({{ hands-on.url | relative_url }})
{% endif %}
{% endfor %}

## 4. Containerised Applications
### Slides: Applications with containers
### Slides: Working with R and Jupyter notebooks
### Slides: Introduction to Bio-workflows
###  Tutorials and exercises
{% for hands-on in items %}
{% if hands-on.topic == 'containers' %}
1. [{{ hands-on.title }}]({{ hands-on.url | relative_url }})
{% endif %}
{% endfor %}
