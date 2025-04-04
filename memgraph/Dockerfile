# Stage 1: Python Builder
FROM python:3.9-slim AS builder

# Install system dependencies
RUN apt-get update && apt-get install -y \
    build-essential \
    python3-pip \
    python3-dev

# Install spacy and download the model
RUN python3 -m pip install -U pip
RUN python3 -m pip install -U spacy wheel
RUN python3 -m spacy download en_core_web_sm

# Install pip packages
COPY requirements.txt ./
RUN pip3 install -r requirements.txt

# Stage 2: Memgraph with Python Libraries
FROM memgraph/memgraph-mage:1.01

USER root

# Install Python and pip dependencies
RUN apt-get update && apt-get install -y \
    python3-pip \
    python3-setuptools \
    python3-dev \
    && pip3 install -U pip

# Copy installed Python libraries from the builder stage
COPY --from=builder /usr/local/lib/python3.9/site-packages /usr/local/lib/python3.9/site-packages

# Copy the local query modules
COPY procedures/ /procedures
COPY transformations/ /transformations

# Switch back to memgraph user
USER memgraph

# Expose necessary ports (optional)
# EXPOSE 7687 3000

# Start Memgraph
ENTRYPOINT ["memgraph"]

# # Use Python 3.9 or 3.10 base image
# FROM python:3.9-slim as builder

# # Install system dependencies
# RUN apt-get update && apt-get install -y \
#     build-essential \
#     python3-pip \
#     python3-dev

# # Install spacy
# RUN python3 -m pip install -U pip
# RUN python3 -m pip install -U spacy


# FROM memgraph/memgraph-mage:1.01 

# USER root

# RUN apt-get update && apt-get install -y \
#     python3-pip \
#     python3-setuptools \
#     python3-dev \
#     && pip3 install -U pip

# # Install the NLP libraries
# RUN python3 -m pip install -U wheel
# # RUN python3 -m pip install -U spacy

# # Download the NLP model for English language
# RUN python3 -m spacy download en_core_web_sm

# # Install pip packages
# COPY requirements.txt ./
# RUN pip3 install -r requirements.txt

# # Copy the local query modules
# COPY procedures/ /procedures
# COPY transformations/ /transformations

# USER memgraph