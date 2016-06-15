# Query-Regression Networks (QRN)
![Teaser figure for QRN](assets/teaser.png)
QRN is a purely sequential model like LSTM or GRU (but simpler than them) for story-based question answering ([bAbI QA tasks][babi]).
QRN is implemented using [TensorFlow][tensorflow].
Here are some notable results (error rates in %) on bAbI QA 1k dataset:

| Task | [LSTM][lstm] | [MemN2N][memn2n] | Ours (2rb) |
| ---- |:------------:|:----------------:|:----------:|
| 2    | 80.0         | 18.8             | 0.7        |
| 3    | 80.0         | 31.7             | 5.7        |
| 4    | 39.0         | 17.5             | 0.0        |
| 5    | 30.0         | 12.9             | 1.1        |
| avg  | 51.3         | 15.2             | 9.9        |

See model details more results in [this paper]().

## 1. Quick Start
We are assuming you are working in a Linux environment. 
Make sure that you have Python 3.5, and you installed these Python packages: `tensorflow` (0.8 or higher, get it [here][tensorflow]), and `progressbar2`.

First, download bAbI QA dataset (note that this downloads the dataset to `$HOME/data/babi`):

```bash
chmod +x download.sh; ./download.sh 
```

Then preprocess the data for a particular task (e.g. Task 3), storing preprocessed data in `data/babi/en/03/`:
```bash
python prepro.py --task 3
```

Finally, you train the model (test is automatically performed at the end):
```bash
python -m babi.main --noload --task 3
```
It took ~3 minutes on my laptop using CPU.

You can run it several times with new weight initialization (e.g. 10) and report the test result with the lowest dev loss:
```bash
python -m bab.main --noload --task 3 --num_trials 10
```
This is critical to stably get the reported results; some weight initialization leads to a bad optima.

## 2. Visualizing Results
After training and testing, the result is stored in `evals/babi/en/03-None-00-01/test_0150.json`.
We can visualize the magnitudes of the update and reset gates using the result file.
Note that you need `jinja2` (Python package).
Run the following command to host a web server for visualization and open it via browser:

```bash
python -m babi.visualize_result --task 2 --open True
```

then click the file(s). It takes a a few seconds to load the heatmap coloring of the gate values.

[babi]: http://arxiv.org/abs/1502.05698
[lstm]: http://arxiv.org/abs/1502.05698 
[memn2n]: http://arxiv.org/abs/1503.08895
[tensorflow]: https://www.tensorflow.org/
