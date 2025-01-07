# Dataset for "Semantic Orientation for Indoor Navigation System using Large Language Models"

## Description
This repository contains the dataset used in the research for the article titled "Semantic Orientation for Indoor Navigation System using Large Language Models". The dataset is organized to support and validate the research findings presented in the paper.

## Authors
- Marzena Halama
- Sławomir Nowak
- Konrad Połys

## Affiliation
Institute of Theoretical and Applied Informatics, Polish Academy of Sciences

## Paper Abstract
Autonomous robots play an important role in modern indoor navigation, but existing systems often struggle with seamless human interaction and semantic understanding of environments. This paper presents an Artificial Intelligence (AI)-driven object recognition system enhanced by Large Language Models (LLMs), such as GPT-4 Vision and Gemini, to bridge this gap. Our approach combines vision-based mapping techniques with natural language processing and interactions to enable intuitive collaboration in solving navigation tasks. By leveraging multimodal input and vector space analysis, our system achieves enhanced object recognition, semantic embedding, and context-aware responses, setting a new standard for autonomous indoor navigation. This approach provides a novel framework for improving spatial understanding and dynamic interaction, making it suitable for complex indoor environments.

## Dataset Structure
The dataset comprises various image files and corresponding JSON metadata files. The images include both full-resolution and cached versions for efficient processing. Each JSON file contains structured data that describes the associated image, including:

- **Acquisition Date**: Timestamp indicating when the data was captured.
- **Name**: Identifier for the environment or scenario.
- **Sequence Number**: Numerical order of the data point in the sequence.
- **UID**: Unique identifier for the dataset entry.
- **Position Coordinates (X, Y)**: Spatial location data.
- **Orientation Yaw**: Orientation data for the robot or sensor.

This structure ensures that the dataset is comprehensive and straightforward for researchers to utilize and analyze.
