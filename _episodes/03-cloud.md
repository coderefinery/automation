---
layout: episode
title: "Tools for automation, cloud"
teaching: 10
exercises: 0
questions:
  - "What is cloud?"
  - "How is it related to DevOps?"
objectives:
  - "You will learn basics of cloud and how you could use it"
keypoints:
  - "Cloud platforms"

---

# What is cloud?

Cloud in this course's perspective means IaaS (Infrastructure as a Service) services. Cloud services in general can contain other type of services as well. 

<img src="https://upload.wikimedia.org/wikipedia/commons/b/b5/Cloud_computing.svg" style="width: 700px;" />

## Available cloud (IaaS) services

To name but a few:

   - Pouta (CSC - IT Center for Science)
   - Cloud 9 (Nebula)
   - Amazon AWS
   - Microsoft Azure
   - Rackspace Open Cloud
   - And many many more from Finland, nordics and the rest of the world

Using these service providers you can spawn servers almost anywhere in the world over Internet connection, usually with a web user interface and/or a command line client. Servers that can easily be created and deleted programmatically are a very good option to embed to your automated processes! For example Ansible contains modules for many cloud platforms: with it you can have a very dynamic environment.

Cloud environments are used in myriad ways, some examples:

* Research use, examples:
    * [Euclid](http://sci.esa.int/euclid/), astronomy
    * [ELIXIR](http://www.elixir-finland.org/), bioinformatics
    * [CMS](https://home.cern/about/experiments/cms), particle physics
    * [FIMM](https://www.fimm.fi/en), medicine
* High performance computing on exotic platforms, for example with special operating systems
* Large scale services are often built on Cloud (or to store data on a Cloud)
* A lot of development work is done on Cloud
* And many many more

