# Arabic Sentences Similarity Detection using AraVec Word Embedding


## Introduction

This repository contains a Python-based tool for Arabic sentence similarity detection using AraVec word embeddings. The tool processes a TXT file containing Arabic text and performs the following steps:

1. **Sentence Splitting**: The tool splits each sentence in the input text file based on newline or comma separators.

2. **Word Embedding Averaging**: It utilizes AraVec's pre-trained word embeddings to obtain the embeddings for each word in a sentence and then averages them to get a representation for the entire sentence.

3. **Cosine Similarity Calculation**: The tool calculates the cosine similarity between each sentence using the sentence embeddings obtained in the previous step.

4. **Graph Representation**: The tool uses NetworkX library to create a graph representation of the sentences. Each sentence is represented as a node, and the similarity scores are used to create edges between nodes.

5. **User-defined Threshold**: A user-defined threshold (chosen as 0.4) is used to filter the connections between sentences in the graph. Only sentences with a similarity score above the threshold will have edges connecting them.

## Results

The Arabic sentence similarity detection using AraVec word embedding has shown promising results. By using the user-defined threshold of 0.4, the tool effectively captures semantically similar sentences and forms connections in the graph accordingly.
![Graph (2)](https://github.com/hossamemamo/Sentence-Similarity-Detection/assets/78453559/f388be60-c5d9-4bab-9815-d7ee2d844267)

as you can see similar nodes (sentences) have an edge.

## Usage

To use this tool, follow these steps:

1. Clone the repository
2. Install the required dependencies: `pip install networkx`
3. Download AraVec pre-trained word embeddings from [here](https://github.com/bakrianoo/aravec/tree/master) and place the model files in the appropriate location.

4. upload your text files and add it to the paths in the notebook:

```python
## SPECIFY HERE THE FILE PATH YOU WANT TO CHECK FOR PALAGIRISM
paths=['/content/Almersal_freedom.txt','/content/chatgpt_freedom.txt','/content/mawdoo3.com_freedom.txt','/content/Sotor_mother.txt']
```

5. Adjust the threshold value in the script if needed. (chosen for this test was 0.4)

```python
def graph_sentences_by_similarity(sentences,cosine_similarity_scores):
    # Create a graph. Each node represents a sentence.
    G = nx.Graph()
    reshaped_sentences=[get_display(arabic_reshaper.reshape(x)) for x in sentences]
    G.add_nodes_from(reshaped_sentences)


    # Add an edge between two sentences whose similarity is > 0.4
    for row in range(0, len(cosine_similarity_scores)):
        for col in range(0, len(cosine_similarity_scores[0])):
            if row != col and cosine_similarity_scores[row][col] > 0.4:
                G.add_edge(reshaped_sentences[row], reshaped_sentences[col])

    # Remove nodes that don't have any edges (i.e. dissimilar sentences).
    # G.remove_nodes_from(list(nx.isolates(G)))
    pos = nx.shell_layout(G, nlist=None, rotate=None, scale=1, center=None, dim=2)

    fig, ax = plt.subplots(1,1)
    nx.draw(G, pos, with_labels=True, node_size=100, node_color="skyblue", font_size=10, font_color='black', ax=ax)
    ax.set_title("Sentence Similarity Graph")
    xlim = ax.get_xlim()
    dx = xlim[1]-xlim[0]
    ax.set_xlim(xlim[0]-dx, xlim[1]+dx)

    plt.savefig("Graph.png", format="PNG")
    plt.show()


```

## Acknowledgements

- AraVec: The pre-trained word embeddings used in this project were developed by the AraVec team. Find the original repository [here](https://github.com/bakrianoo/aravec).

- NetworkX: The graph representation and manipulation were implemented using the NetworkX library.
