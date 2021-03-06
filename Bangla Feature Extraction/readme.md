# Bangla Feature Extractor(BnFeatureExtraction)

BnFeatureExtraction is a Bangla Natural Language Processing based feature extractor.


### Feature Extraction

  1. [CountVectorizer](#1-countvectorizer)
  2. [HashVectorizer](#2-hashvectorizer)
  3. [TfIdf](#3-tfidf)
  4. [Word Embedding](#4-word-embedding)
      * [Word2Vec](#word2vec)
      * [FastText](#fasttext)
	  * [GloVe](#glove)

## Installation 
```
pip install BnFeatureExtraction
```
## Example
### 1. CountVectorizer
  - Fit n Transform
  - Transform
  - Get Wordset
  
**Fit n Transform**
```py
from BnFeatureExtraction import CountVectorizer
ct = CountVectorizer()
X = ct.fit_transform(X) # X is the word features
```
Output:
```
the countVectorized matrix form of given features
```

**Transform**
```py
from BnFeatureExtraction import CountVectorizer
ct = CountVectorizer()
get_mat = ct.transform("রাহাত")
```
Output:
```
the countVectorized matrix form of given word
```

**Get Wordset**
```py
from BnFeatureExtraction import CountVectorizer
ct = CountVectorizer()
ct.get_wordSet()
```
Output:
```
get the raw wordset used in training model
```

### 2. HashVectorizer
  - Fit n Transform
  - Transform
```py
from BnFeatureExtraction import HashVectorizer
corpus = [
'আমাদের দেশ বাংলাদেশ', 'আমার বাংলা'
]
Vectorizer = HashVectorizer()
n_features = 8
X = Vectorizer.fit_transform(corpus, n_features)
corpus_t = ["আমাদের দেশ অনেক সুন্দর"]
Xf = Vectorizer.transform(corpus_t)

print(X.shape, Xf.shape)
print("=====================================")
print(X)
print("=====================================")
print(Xf)
```
Output:
```
(2, 8) (1, 8)
=====================================
  (0, 7)	-1.0
  (1, 7)	-1.0
=====================================
  (0, 0)	0.5773502691896258
  (0, 2)	0.5773502691896258
  (0, 7)	-0.5773502691896258
```
  
**Get Wordset**


### 3. TfIdf
  - Fit n Transform
  - Transform
  - Coefficients
  
 **Fit n Transform**
```py
from BnFeatureExtraction import TfIdfVectorizer
k = TfIdfVectorizer()
doc = ["কাওছার আহমেদ", "শুভ হাইদার"]
matrix1 = k.fit_transform(doc)
print(matrix1)
```
Output:
```
[[0.150515 0.150515 0.       0.      ]
 [0.       0.       0.150515 0.150515]]

```

**Transform**
```py
from BnFeatureExtraction import TfIdfVectorizer
k = TfIdfVectorizer()
doc = ["আহমেদ সুমন", "কাওছার করিম"]
matrix2 = k.transform(doc)
print(matrix2)
```
Output: 
```
[[0.150515 0.       0.       0.      ]
 [0.       0.150515 0.       0.      ]]
```
**Coefficients**
```py
from BnFeatureExtraction import TfIdfVectorizer
k = TfIdfVectorizer()
doc = ["কাওছার আহমেদ", "শুভ হাইদার"]
k.fit_transform(doc)
wordset, idf = k.coefficients()
print(wordset)
#Output: ['আহমেদ', 'কাওছার', 'হাইদার', 'শুভ']

print(idf)
'''
Output: 
{'আহমেদ': 0.3010299956639812, 'কাওছার': 0.3010299956639812, 'হাইদার': 0.3010299956639812, 'শুভ': 0.3010299956639812}
'''

```
  
### 4. Word Embedding
- ### Word2Vec
    - Training
    - Get Word Vector
    - Get Similarity
    - Get n Similar Words
    - Get Middle Word
    - Get Odd Words
    - Get Similarity Plot

**Training**
```py
from BnFeatureExtraction import BN_Word2Vec
#Training Against Sentences
w2v = BN_Word2Vec(sentences=[['আমার', 'প্রিয়', 'জন্মভূমি'], ['বাংলা', 'আমার', 'মাতৃভাষা'],['আমার', 'প্রিয়', 'জন্মভূমি'], ['বাংলা', 'আমার', 'মাতৃভাষা'],['আমার', 'প্রিয়', 'জন্মভূমি'], ['বাংলা', 'আমার', 'মাতৃভাষা']])
w2v.train()

#Training Against one Text Corpus
w2v = BN_Word2Vec(corpus_file="path_to_corpus.txt")
w2v.train()

#Training Against Multiple corpuses
'''
    path
      ->corpus
        ->1.txt
        ->2.txt
        ->3.txt
'''
w2v = BN_Word2Vec(corpus_path="path/corpus")
w2v.train(epochs=25)


#Training Against a Dataframe Column
w2v = BN_Word2Vec(df= news_data['text_content'])
w2v.train(epochs=25)
```
After training is done the model "w2v_model"  along with it's supportive vector files will be saved to current directory.

**If you use any pretrained model, specify it while initializing BN_Word2Vec() . Otherwise no model_name is needed.**

**Get Word Vector**
```py
from BnFeatureExtraction import BN_Word2Vec 
w2v = BN_Word2Vec(model_name='give the model name here')
w2v.get_wordVector('আমার')
```

**Get Similarity**
```py
from BnFeatureExtraction import BN_Word2Vec 
w2v = BN_Word2Vec(model_name='give the model name here')
w2v.get_similarity('ঢাকা', 'রাজধানী')
```
Output: 
```
67.457879
```

**Get n Similar Words**
```py
from BnFeatureExtraction import BN_Word2Vec 
w2v = BN_Word2Vec(model_name='give the model name here')
w2v.get_n_similarWord(['পদ্মা'], n=10)
```
Output: 
```
[('সেতুর', 0.5857524275779724),
 ('মুলফৎগঞ্জ', 0.5773632526397705),
 ('মহানন্দা', 0.5634652376174927),
 ("'পদ্মা", 0.5617109537124634),
 ('গোমতী', 0.5605217218399048),
 ('পদ্মার', 0.5547558069229126),
 ('তুলসীগঙ্গা', 0.5274507999420166),
 ('নদীর', 0.5232067704200745),
 ('সেতু', 0.5225246548652649),
 ('সেতুতে', 0.5192927718162537)]
```

**Get Middle Word**

Get the probability distribution of the center word given words list.
```py
from BnFeatureExtraction import BN_Word2Vec 
w2v = BN_Word2Vec(model_name='give the model name here')
w2v.get_outputWord(['ঢাকায়', 'মৃত্যু'], n=2)
```
Output:
```
[("হয়েছে।',", 0.05880642), ('শ্রমিকের', 0.05639163)]
```

**Get Odd Words**

Get the most unmatched word out from given words list
```py
from BnFeatureExtraction import BN_Word2Vec 
w2v = BN_Word2Vec(model_name='give the model name here')
w2v.get_oddWords(['চাল', 'ডাল', 'চিনি', 'আকাশ'])
```
Output: 
```
'আকাশ' 
```

**Get Similarity Plot**

Creates a barplot of similar words with their probability 

```py
from BnFeatureExtraction import BN_Word2Vec 
w2v = BN_Word2Vec(model_name='give the model name here')
w2v.get_similarity_plot('চাউল', 5)
```

- ### FastText
    - Training
    - Get Word Vector
    - Get Similarity
    - Get n Similar Words
    - Get Middle Word
    - Get Odd Words
  
 
**Training**
```py
from BnFeatureExtraction import BN_FastText
#Training Against Sentences
ft = ft = BN_FastText(sentences=[['আমার', 'প্রিয়', 'জন্মভূমি'], ['বাংলা', 'আমার', 'মাতৃভাষা'], ['বাংলা', 'আমার', 'মাতৃভাষা'], ['বাংলা', 'আমার', 'মাতৃভাষা'], ['বাংলা', 'আমার', 'মাতৃভাষা'] ])
ft.train()

#Training Against one Text Corpus
ft = BN_FastText(corpus_file="path to data or txt file")
ft.train()

#Training Against Multiple Corpuses
'''
    path
      ->Corpus
        ->1.txt
        ->2.txt
        ->3.txt
'''
ft = BN_FastText(corpus_path="path/Corpus")
ft.train(epochs=25)

#Training Against a Dataframe Column
ft = BN_FastText(df= news_data['text_content'])
ft.train(epochs=25)

```
After training is done the model "ft_model"  along with it's supportive vector files will be saved to current directory.

**If you don't want to train instead use a pretrained model, specify it while initializing BN_FastText() . Otherwise no model_name is needed.**

**Get Word Vector**
```py
from BnFeatureExtraction import BN_FastText 
ft = BN_FastText(model_name='give the model name here')
ft.get_wordVector('আমার')
```

**Get Similarity**
```py
from BnFeatureExtraction import BN_FastText 
ft = BN_FastText(model_name='give the model name here')
ft.get_similarity('ঢাকা', 'রাজধানী')
```
Output:
```
70.56821120
```

**Get n Similar Words**
```py
from BnFeatureExtraction import BN_FastText 
ft = BN_FastText(model_name='give the model name here')
ft.get_n_similarWord(['পদ্মা'], n=10)
```

Output: 
```
[('পদ্মায়', 0.8103810548782349),
 ('পদ্মার', 0.794012725353241),
 ('পদ্মানদীর', 0.7747839689254761),
 ('পদ্মা-মেঘনার', 0.7573559284210205),
 ('পদ্মা.', 0.7470568418502808),
 ('‘পদ্মা', 0.7413997650146484),
 ('পদ্মাসেতুর', 0.716225266456604),
 ('পদ্ম', 0.7154797315597534),
 ('পদ্মহেম', 0.6881639361381531),
 ('পদ্মাবত', 0.6682782173156738)]
```

**Get Odd Words**

Get the most unmatched word out from given words list
```py
from BnFeatureExtraction import BN_FastText 
ft = BN_FastText(model_name='give the model name here')
ft.get_oddWords(['চাল', 'ডাল', 'চিনি', 'আকাশ'])
```
Output:
```
'আকাশ' 
```

**Get Similarity Plot**

Creates a barplot of similar words with their probability 

```py
from BnFeatureExtraction import BN_FastText 
ft = BN_FastText(model_name='give the model name here')
ft.get_similarity_plot('চাউল', 5)
```

- ### GloVe
    - Training
    - Get n Similar Words
	
 
**Training**
```py
from BnFeatureExtraction import BN_GloVe
#Training Against Sentences
glv = BN_GloVe(sentences=[['আমার', 'প্রিয়', 'জন্মভূমি'], ['বাংলা', 'আমার', 'মাতৃভাষা'], ['বাংলা', 'আমার', 'মাতৃভাষা'], ['বাংলা', 'আমার', 'মাতৃভাষা'], ['বাংলা', 'আমার', 'মাতৃভাষা'] ])
glv.train()

#Training Against one Text Corpus
glv = BN_GloVe(corpus_file="path_to_corpus.txt")
glv.train()

#Training Against Multiple Corpuses
'''
    path
      ->Corpus
        ->1.txt
        ->2.txt
        ->3.txt
'''
glv = BN_GloVe(corpus_path="path/corpus")
glv.train(epochs=25)

#Training Against a Dataframe Column
glv = BN_GloVe(df= news_data['text_content'])
glv.train(epochs=25)


```
After training is done the model "glove_model"  along with it's supportive vector files will be saved to current directory.

**If you don't want to train instead use a pretrained model, specify it while initializing BN_FastText() . Otherwise no model_name is needed.**


**Get n Similar Words**
```py
from BnFeatureExtraction import BN_GloVe 
glv = BN_GloVe(model_name='give the model name here')
glv.get_n_similarWord(['পদ্মা'], n=10)
```

Output: 
```
[('পদ্মায়', 0.8103810548782349),
 ('পদ্মার', 0.794012725353241),
 ('পদ্মানদীর', 0.7747839689254761),
 ('পদ্মা-মেঘনার', 0.7573559284210205),
 ('পদ্মা.', 0.7470568418502808),
 ('‘পদ্মা', 0.7413997650146484),
 ('পদ্মাসেতুর', 0.716225266456604),
 ('পদ্ম', 0.7154797315597534),
 ('পদ্মহেম', 0.6881639361381531),
 ('পদ্মাবত', 0.6682782173156738)]
```



