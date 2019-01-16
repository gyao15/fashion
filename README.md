# Fashion
## Dataset
The whole data can be downloaded from https://cloud.tsinghua.edu.cn/d/defcceb1988841058098/ . <br>
This dataset consists of 68,306 outfits and their meta data crawled from the polyvore website. It is provided by the paper we refer to.<br>
`@inproceedings{VasilevaECCV18FashionCompatibility,
  Author = {Mariya I. Vasileva and Byran A. Plummer and Krishna Dusad and Shreya Rajpal and Ranjitha Kumar and David Forsyth},
  Title = {Learning Type-Aware Embeddings for Fashion Compatibility},<br>
  booktitle = {ECCV},
  Year = {2018}
}`
### Dataset Splits
Two versions of the data:

Polyvore Outfits (nondisjoint) - Outfits are split at random, which means some
items (but not complete outfits) may be seen in both training and test splits.

Polyvore Outfits (disjoint) - Outfits in the test/validation set do not share
any items in common with outfits in the training set (although some items in
the test set may be present in outfits in the validation set)

Within each version folder we have:

<test/valid/train>.json - a list of outfits, their item_id's, and their
ordering (index) we imposed when we compared to prior work using an LSTM

typespaces.p - a list of tuples (t1, t2), each of which identifies a type-
specific embedding that compares items of type t1 to items of type t2

train_hglmm_pca6000.txt - each row contains 6001 comma separated values, where
the first element is the label, and the remaining 6000 dimensions are the
PCA-reduced HGLMM fisher vectors (note: the "label" may also contain a comma)

The remaining files in these folders are for the tasks used to evaluate our
models. They contain item identifiers of the form <set_id>_<index>, which can
be mapped back to item_id's using the list of outfits mentioned above.  These
remaining files are:

compatibility_<test/valid/train>.txt - fashion compatibility experiment data,
where each row is an outfit sample. The first element of the outfit sample is
the label (1/0 for positive/negative) and the remaining elements are item
identifiers in that sample.

fill_in_blank_<test/valid/train>.json - fill-in-the-blank experiment data,
contains an array of dictionaries.  These dictionaries contain the question/
answer pairs, and also identifies the "index" of the item in the outfit in
"blank_position".  Since the set_id is used in the item identifiers, the
correct answer can be determined by matching the set_id in the question
elements with the set_id in the answers.<br>
`We use the disjoint one to train our model`

### Maryland Polyvore Test Data

It provides the fashion compatibility and fill-in-the-blank test data.
This data is more difficult than those used in the original paper
because they replace items of the same type when creating negatives rather than
those sampled at random provided with the Maryland Polyvore dataset. 


### Images and Meta-Data

Images are stored by their item_id, which are organized in lists of outfits for
each each version of the dataset.

polyvore_item_metadata.json - contains a dictionary where each key is an
item_id, and the values are its associated meta-data labels.

polyvore_outfit_titles.json - contains a dictionary where each key is a set_id
and the values are its associated meta-data labels.

categories.csv - Each row contains three items: (category_id, fine-grained
category, semantic category)

## Training
You can see a listing and description of the model options with:<br>
`python main.py --help`
If you want to train a new model, you can run:<br>
`python main.py --learned --l2_embed --datadir {your data directory} --name {your model name}`
Your model will be saved in `runs/{your model name}/`.You can also try to use other options. Here we just show what we used.<br>

## Testing
We provide a pre-trained model in `runs/Type_Specific_Fashion_Compatibility/origin/`, you can run:<br>
`python main.py --test --use_fc --resume runs/Type_Specific_Fashion_Compatibility/origin/model_best.pth.tar`
Or you can test your own trained model. The code will produce two txt files `result_choose.txt` and `result_compatibility.txt`.<br>
We also provide two small python files to show our test questions. You can run `python show_choose/compatibility.py --idx {index of questions you want to visualize}` and then compared to the txt file.
