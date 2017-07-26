# Political Vector Projector

Given word vectors trained by word2vec (Mikolov et al. 2013) or fastText (Bojanowski et al. 2016), project the vectors of U.S. senators onto a 'conservative' to 'liberal' axis. The scalar components of such projections may be interpreted as a valid metric of political ideology.

Learn more about this project at [here](https://empirical.coffee/blog/2017/political-embedding-vector-projector).

## Highlight

Plotting the vector projected ideology against [DW-NOMINATE](https://en.wikipedia.org/wiki/NOMINATE_(scaling_method), an ideology metric widely used in political science, reveals a strong correlation:
![alt text](https://static1.squarespace.com/static/53d176f8e4b02aa639c65599/t/59738be4e6f2e17330d7d551/1500744686272/?format=1500w)

| Training Corpus       | Avg. Pearson’s r | Avg. Spearman’s 𝝆 |
|-----------------------|------------------|--------------------|
| NYT 1981 - 2016       | 0.7559           | 0.7602             |
| Wash Post 1977 - 2007 | __0.7902__       | __0.8003__         |
| WSJ 1997 - 2017       | 0.7205           | 0.7184             |

In addition to members of Congress, you can also project vectors of public policies. These results are quite amusing but still highly experimental. Again, for a detailed account, please refer to [here](https://empirical.coffee/blog/2017/political-embedding-vector-projector).
![alt text](https://static1.squarespace.com/static/53d176f8e4b02aa639c65599/t/59777ee1cf81e03bebde6c2c/1501003497098/NYT+policies.png?format=1500w)


## Requirements 

[fastText](https://github.com/facebookresearch/fastText) or [word2vec](https://code.google.com/archive/p/word2vec/) (in their original C implementations). 

[Gensim](https://radimrehurek.com/gensim/install.html) (optional, only needed for the experimental feature of projecting public policies.)

The DW-NOMINATE ideology data is available at [voteview.com](voteview.com).

I apologize that I have tested the code only with Python 3.6

## Loading Data

There are two methods for loading vectors into PoliVec Projector:

First Method: Use the `read_vec_file_as_dict` function to read vector files generated by word2vec. You may then get any in-vocabulary vector by calling `vec_dict['word']`. Similarly, you can use this dictionary method with fastText, but this requires the [Gensim](https://radimrehurek.com/gensim/install.html) Python wrapper for fastText, which enables querying out-of-vocabulary vectors.

Second Method: (seemingly more complicated, but requires no wrapper; more efficient for comparing multiple years of data) Provide a plain text list of words you want to query, along with the axes onto which you want to project. The axes follow the order of: [positive x axis, negative x axis, positive y axis, negative y axis]. An example queries.txt look like this:
```
conservative
liberal
good
bad
johnson
nixon
carter
reagan
etc.
```
If you are interested in members of Congress, the `gen_queries.py` script in this repo can take care of this step for you. The namelist from the 95th to the 114th Senate (1977 - 2017) are also already included in this repo at `queries/`

Then, run `gen_vectors.sh`, which takes multiple lists of queries and feed them to fastText's `print-word-vectors` function. Be sure to revise the directories specified in the shell script so that it loads your own pre-trained fastText models. The vectors of members of the 95th to 114th Senate are also already included in this repo at `queried_vectors/`

Lastly, you can simply call `read_vec_file_with_axes` in the main script to load your vectors of choice.

(In principal, you can use PoliVec Projector with any word embedding models, so long as you tweak the file IO functions to load your vectors properly.)

## License
MIT