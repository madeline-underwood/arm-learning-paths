---
title: Run distributed inference with llama.cpp on Arm-based AWS instances

minutes_to_complete: 30

who_is_this_for: This introductory topic is for developers who are familiar with llama.cpp and want to scale their workloads by running distributed inference on Arm-based AWS instances.

learning_objectives: 
  - Configure master and worker nodes for distributed inference with `llama.cpp`
  - Deploy and run a quantized 405B model (for example Llama 3.1) using distributed CPU inference on Arm-based AWS C8g instances

prerequisites:
  - Three AWS Graviton-based `c8g.4xlarge` instances with at least 500 GB of EBS storage each
  - Python 3.x installed on all instances
  - Access to Meta’s gated repository for the Llama 3.1 model family and a Hugging Face token for downloading models
  - Familiarity with the Learning Path [Deploy a Large Language Model (LLM) chatbot with llama.cpp using KleidiAI on Arm servers](/learning-paths/servers-and-cloud-computing/llama-cpu)
  - Basic knowledge of AWS EC2, networking, and security groups

author: 
  - Aryan Bhusari
  - Joe Stech

### Tags
skilllevels: Introductory
subjects: ML
armips:
  - Neoverse
tools_software_languages:
  - LLM
  - GenAI
  - AWS
operatingsystems:
  - Linux

further_reading:
  - resource:
      title: llama.cpp RPC server code
      link: https://github.com/ggml-org/llama.cpp/tree/master/tools/rpc
      type: code
  - resource:
      title: AWS Graviton Service Documentation
      link: https://aws.amazon.com/ec2/graviton/
      type: documentation
  - resource:
      title: Hugging Face Model Hub
      link: https://huggingface.co/models
      type: documentation

### FIXED, DO NOT MODIFY
# ================================================================================
weight: 1
layout: "learningpathall"
learning_path_main_page: "yes"
---
