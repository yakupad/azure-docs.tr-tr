---
title: Derleme ve Azure Machine Learning paket için metin analizi kullanarak metin sınıflandırma modeli dağıtın.
description: Oluşturmak, eğitmek, test ve metin analizi için Azure Machine Learning paketi kullanarak bir metin sınıflandırma model dağıtma hakkında bilgi edinin.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: netahw
author: nhaiby
ms.date: 05/07/2018
ms.openlocfilehash: a970bd889e6994833b2e34adc90af594f9db4d6b
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2018
ms.locfileid: "33939727"
---
# <a name="build-and-deploy-text-classification-models-with-azure-machine-learning"></a>Derleme ve metin sınıflandırma modelleri Azure Machine Learning ile dağıtma

Bu makalede, eğitmek ve bir metin sınıflandırma modeliyle dağıtma hakkında bilgi edinebilirsiniz **metin analizi için Azure Machine Learning paketi** (AMLPTA). Metin sınıflandırma amacı bir metin parçası birine atamak için ya da daha fazla sınıf veya kategorileri önceden tanımlanmış. Bu metin, olabilir örneğin bir belge, haber makaleleri, arama sorgusu, e-posta, tweet, destek biletlerini.

Metin sınıflandırma gibi geniş uygulamalar şunlardır: 
+ Kategorilere gazete makaleler ve haber kablo içeriğine konuları
+ Hiyerarşik kategoriler halinde Web sayfalarını düzenleme 
+ İstenmeyen e-posta filtreleme
+ Duygu analizi
+ Kullanıcı amaçtan tahmin etmeye arama sorguları
+ Yönlendirme destek biletlerini
+ Müşteri geri bildirimi analiz etme 

Metin sınıflandırma modeli oluşturma ve dağıtma iş akışı AMLPTA ile bir model için aşağıdaki gibidir:

1. Verileri yükleme
2. Modeli eğitme
3. Sınıflandırıcı Uygula 
4. Model performansını değerlendirme
5. Ardışık Düzen Kaydet
6. Ardışık Düzen test
8. Bir web hizmeti olarak modeli dağıtın

Başvurun [paketini başvuru belgeleri](https://aka.ms/aml-packages/text) her modül ve sınıfı için ayrıntılı başvuru için.

Bu makaledeki örnek kod bir scikit kullanır-ardışık düzen öğrenin.

## <a name="prerequisites"></a>Önkoşullar 

1. Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

1. Aşağıdaki hesapları ve uygulama kurmalı ve yüklü:
   - Azure Machine Learning deneme hesabı 
   - Bir Azure Machine Learning modeli yönetim hesabı
   - Azure Machine Learning Workbench'in yüklü olması

   Bu üç henüz oluşturduğunuz veya yüklü uygulayın [Azure Machine Learning Quickstart ve çalışma ekranı yükleme](../service/quickstart-installation.md) makalesi. 

1. Metin analizi için Azure Machine Learning paketi yüklü olmalıdır. Bilgi edinmek için nasıl [burada bu paketi yükledikten](https://aka.ms/aml-packages/text).


## <a name="sample-data-and-jupyter-notebook"></a>Örnek veriler ve Jupyter not defteri

### <a name="get-the-jupyter-notebook"></a>Jupyter Not Defteri Al

Kendiniz deneyin. Not Defteri indirin ve kendiniz çalıştırın.

> [!div class="nextstepaction"]
> [Jupyter Not Defteri Al](https://aka.ms/aml-packages/text/notebooks/text_classification_sentiment_data)

### <a name="download-and-explore-the-sample-data"></a>Karşıdan yükle ve örnek verileri keşfedin
Aşağıdaki örnek kullanır [20 haber dataset](http://qwone.com/~jason/20Newsgroups/) scikit kullanılabilir-metin sınıflandırıcı metin analizi için Azure Machine Learning paketiyle oluşturma göstermek için kitaplık öğrenin. 

İki alt kümeler bölünmüş 20 farklı konularda 18, 000'yaklaşık haber gönderileri 20 haber veri kümesi vardır: biri eğitim ve Performans değerlendirmesinin için başka bir. Eğitin ve test Böl her ileti post tarihine dayanır tabanlı mı yoksa önce belirli bir tarihten sonra.

```python
# Import Packages 
# Use Azure Machine Learning history magic to control history collection
# History is off by default, options are "on", "off", or "show"
#%azureml history on
%matplotlib inline
# Use the Azure Machine Learning data collector to log various metrics
from azureml.logging import get_azureml_logger
import os

logger = get_azureml_logger()

# Log cell runs into run history
logger.log('Cell','Set up run')
# from tatk.utils import load_newsgroups_data, data_dir, dictionaries_dir, models_dir
import pip
pip.main(["show", "azureml-tatk"])
```

### <a name="set-the-location-of-the-data"></a>Verilerin konumu ayarlayın
Verileri veri dir parametresinde indirdiğiniz konumunu ayarlayın. Kendi verilerinizi de kullanabilirsiniz, girdi veri kümesi bir *.tsv dosyası biçiminde olmalıdır.

```python
import os
import pandas as pd

#set the working directory where to save the training data files
resources_dir = os.path.join(os.path.expanduser("~"), "tatk", "resources")
data_dir = os.path.join(os.path.expanduser("~"), "tatk", "data")

from sklearn.datasets import fetch_20newsgroups
twenty_train = fetch_20newsgroups(data_home=data_dir, subset='train')
X_train, y_train = twenty_train.data, twenty_train.target
df_train = pd.DataFrame({"text":X_train, "label":y_train})

twenty_test = fetch_20newsgroups(data_home=data_dir, subset='test')
X_test, y_test = twenty_test.data, twenty_test.target   
df_test = pd.DataFrame({"text":X_test, "label":y_test})
    
# Training Dataset Location
#training_file_path = <specify-your-own-training-data-file-path-here>
# df_train = pd.read_csv(training_file_path,
#                        sep = '\t',                        
#                        header = 0, names= <specify-your-column-name-list-here>)
df_train.head()
print("df_train.shape= {}".format(df_train.shape))

# Test Dataset Location
#test_file_path = <specify-your-own-test-data-file-path-here>
# df_test = pd.read_csv(test_file_path,
#                       sep = '\t',                        
#                       header = 0, names= <specify-your-column-name-list-here>)

print("df_test.shape= {}".format(df_test.shape))
df_test.head()
```
    df_train.shape= (11314, 2)
    df_test.shape= (7532, 2)
 
 Etiket ve metin verileri oluşur
    
<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Etiket</th>
      <th>metin</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>7</td>
      <td>Kimden: v064mb9k@ubvmsd.cc.buffalo.edu (NEIL B....</td>
    </tr>
    <tr>
      <th>1</th>
      <td>5</td>
      <td>Kimden: Rick Mert &lt; rick@ee.uwm.edu &gt;\nSubject:...</td>
    </tr>
    <tr>
      <th>2</th>
      <td>0</td>
      <td>Kimden: mathew &lt; mathew@mantis.co.uk &gt;\nSubject: r...</td>
    </tr>
    <tr>
      <th>3</th>
      <td>17</td>
      <td>Kimden: bakken@cs.arizona.edu (Dave Bakken) \nSub...</td>
    </tr>
    <tr>
      <th>4</th>
      <td>19</td>
      <td>Kimden: livesey@solntze.wpd.sgi.com (Jon Livesey...</td>
    </tr>
  </tbody>
</table>
</div>

Kategoriler ve kullanıcıların adı arasında correspondance alın.

```python
int_to_categories = pd.DataFrame({'category':range(20), 'category_name': list(twenty_train.target_names)})
int_to_categories 
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>category</th>
      <th>category_name</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>alt.atheism</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>comp.Graphics</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2</td>
      <td>comp.OS.MS windows.misc</td>
    </tr>
    <tr>
      <th>3</th>
      <td>3</td>
      <td>comp.sys.ibm.PC.Hardware</td>
    </tr>
    <tr>
      <th>4</th>
      <td>4</td>
      <td>comp.sys.Mac.Hardware</td>
    </tr>
    <tr>
      <th>5</th>
      <td>5</td>
      <td>comp.Windows.x</td>
    </tr>
    <tr>
      <th>6</th>
      <td>6</td>
      <td>Misc.forsale</td>
    </tr>
    <tr>
      <th>7</th>
      <td>7</td>
      <td>rec.Autos</td>
    </tr>
    <tr>
      <th>8</th>
      <td>8</td>
      <td>rec.motorcycles</td>
    </tr>
    <tr>
      <th>9</th>
      <td>9</td>
      <td>rec.sport.Baseball</td>
    </tr>
    <tr>
      <th>10</th>
      <td>10</td>
      <td>rec.sport.hockey</td>
    </tr>
    <tr>
      <th>11</th>
      <td>11</td>
      <td>sci.crypt</td>
    </tr>
    <tr>
      <th>12</th>
      <td>12</td>
      <td>sci.Electronics</td>
    </tr>
    <tr>
      <th>13</th>
      <td>13</td>
      <td>sci.MED</td>
    </tr>
    <tr>
      <th>14</th>
      <td>14</td>
      <td>sci.Space</td>
    </tr>
    <tr>
      <th>15</th>
      <td>15</td>
      <td>SOC.religion.Christian</td>
    </tr>
    <tr>
      <th>16</th>
      <td>16</td>
      <td>Talk.politics.guns</td>
    </tr>
    <tr>
      <th>17</th>
      <td>17</td>
      <td>Talk.politics.mideast</td>
    </tr>
    <tr>
      <th>18</th>
      <td>18</td>
      <td>Talk.politics.misc</td>
    </tr>
    <tr>
      <th>19</th>
      <td>19</td>
      <td>Talk.religion.misc</td>
    </tr>
  </tbody>
</table>
</div>

Şimdi, eğitim sınıfı frekans ön araştırması çizim çubuk grafik oluşturma ve veri kümelerini test. 

```python
import numpy as np
import math
from matplotlib import pyplot as plt

data = df_train["label"].values
labels = set(data)
print(labels)
bins = range(len(labels)+1) 

#plt.xlim([min(data)-5, max(data)+5])

plt.hist(data, bins=bins, alpha=0.8)
plt.title('training data distribution over the class labels)')
plt.xlabel('class label')
plt.ylabel('frequency')
plt.grid(True)
plt.show()

data = df_test["label"].values
labels = set(data)
print(labels)
bins = range(len(labels)+1) 

#plt.xlim([min(data)-5, max(data)+5])

plt.hist(data, bins=bins, alpha=0.8)
plt.title('test data distribution over the class labels)')
plt.xlabel('class label')
plt.ylabel('frequency')
plt.grid(True)
plt.show()
```

Önceki kod bloğu çalıştırıldıktan sonra Jupypter not defteri çalıştırırken çizimleri görüntülenir.


## <a name="train-the-model"></a>Modeli eğitme

### <a name="specify-scikit-learn-algorithm-and-define-the-text-classifier"></a>Scikit belirtin-algoritması öğrenin ve metin sınıflandırıcı tanımlayın

Bu adım bir scikit eğitim içerir-Rest karşı birini kullanarak metin sınıflandırma modeli öğrenin Lojistik regresyon öğrenme algoritması.

Learnings tam listesi için bkz [Scikit Öğrencileriyle](http://scikit-learn.org/stable/supervised_learning) belgeleri.

```python
from sklearn.linear_model import LogisticRegression
import tatk
from tatk.pipelines.text_classification.text_classifier import TextClassifier

log_reg_learner =  LogisticRegression(penalty='l2', dual=False, tol=0.0001, 
                            C=1.0, fit_intercept=True, intercept_scaling=1, 
                            class_weight=None, random_state=None, 
                            solver='lbfgs', max_iter=100, multi_class='ovr',
                            verbose=1, warm_start=False, n_jobs=3) 

#train the model a text column "tweets"
text_classifier = TextClassifier(estimator=log_reg_learner, 
                                text_cols = ["text"], 
                                label_cols = ["label"], 
#                                 numeric_cols = None,
#                                 cat_cols = None, 
                                extract_word_ngrams=True, extract_char_ngrams=True)

```

    TextClassifier::create_pipeline ==> start
    :: number of jobs for the pipeline : 12
    0   text_nltk_preprocessor
    1   text_word_ngrams
    2   text_char_ngrams
    3   assembler
    4   learner
    TextClassifier::create_pipeline ==> end
    
### <a name="fit-the-model"></a>Model Sığdır

Paketin varsayılan parametrelerini kullanın. Varsayılan olarak, metin sınıflandırıcı ayıklar:
+ Word unigrams ve bigrams
+ 4 gram karakter

```python
text_classifier.fit(df_train)        
```
   
    TextClassifier::fit ==> start
    schema: col=label:I4:0 col=text:TX:1 header+
    NltkPreprocessor::tatk_fit_transform ==> start
    NltkPreprocessor::tatk_fit_transform ==> end     Time taken: 0.08 mins
    NGramsVectorizer::tatk_fit_transform ==> startNGramsVectorizer::tatk_fit_transform ==> start
    
                vocabulary size=216393
    NGramsVectorizer::tatk_fit_transform ==> end     Time taken: 0.41 mins
                vocabulary size=67230
    NGramsVectorizer::tatk_fit_transform ==> end     Time taken: 0.49 mins
    VectorAssembler::transform ==> start, num of input records=11314
    (11314, 216393)
    (11314, 67230)
    all_features::
    (11314, 283623)
    Time taken: 0.06 mins
    VectorAssembler::transform ==> end
    LogisticRegression::tatk_fit ==> start
    
    [Parallel(n_jobs=3)]: Done  20 out of  20 | elapsed:  2.4min finished
    
    LogisticRegression::tatk_fit ==> end     Time taken: 2.4 mins
    Time taken: 3.04 mins
    TextClassifier::fit ==> end

    TextClassifier(add_index_col=False, callable_proprocessors_list=None,
            cat_cols=None, char_hashing_original=False, col_prefix='tmp_00_',
            decompose_n_grams=False, detect_phrases=False,
            dictionary_categories=None, dictionary_file_path=None,
            embedding_file_path=None, embedding_file_path_fastText=None,
            estimator=None, estimator_vectorizers_list=None,
            extract_char_ngrams=True, extract_word_ngrams=True,
            label_cols=['label'], numeric_cols=None,
            pos_tagger_vectorizer=False,
            preprocessor_dictionary_file_path=None, regex_replcaement='',
            replace_regex_pattern=None, scale_numeric_cols=False,
            text_callable_list=None, text_cols=['text'], text_regex_list=None,
            weight_col=None)


Eğitim sırasında metin ve etiket sütuna sahip olmalıdır. Yalnızca metin sütunu tahminleri için gereklidir, ancak. 

### <a name="examine-and-set-the-parameters-of-the-different-pipeline-steps"></a>İnceleyin ve farklı ardışık düzen adımları parametrelerini ayarlama
    
Genellikle, bir model sığacak önce parametrelerini ayarlayın. 

***Text_word_ngrams ile gösterilen örnek*** 

Aşağıdaki kod örnekleri varsayılan ardışık düzen ve model parametreleri kullanarak modeli eğitmek nasıl gösterir. 

Hangi parametreler "text_word_ngrams" için dahil edilen görmek için [get_step_param_names_by_name](https://docs.microsoft.com/python/api/tatk.core.base_text_model.basetextmodel). Bu işlev, küçük harf, input_col, output_col gibi parametreleri vb. döndürür. 

```python
text_classifier.get_step_param_names_by_name("text_word_ngrams")
```

    ['min_df',
     'strip_accents',
     'max_df',
     'decode_error',
     'max_features',
     'binary',
     'input',
     'vocabulary',
     'analyzer',
     'token_pattern',
     'encoding',
     'use_idf',
     'save_overwrite',
     'output_col',
     'stop_words',
     'sublinear_tf',
     'input_col',
     'lowercase',
     'ngram_range',
     'preprocessor',
     'tokenizer',
     'hashing',
     'dtype',
     'norm',
     'smooth_idf',
     'n_hashing_features']

Ardından, "text_char_ngrams" parametre değerlerini kontrol edin:

```python
text_classifier.get_step_params_by_name("text_char_ngrams")        
```
    {'analyzer': 'char_wb',
     'binary': False,
     'decode_error': 'strict',
     'dtype': numpy.float32,
     'encoding': 'utf-8',
     'hashing': False,
     'input': 'content',
     'input_col': 'NltkPreprocessor5283a730506549cc880f074e750607b0',
     'lowercase': True,
     'max_df': 1.0,
     'max_features': None,
     'min_df': 3,
     'n_hashing_features': None,
     'ngram_range': (4, 4),
     'norm': 'l2',
     'output_col': 'NGramsVectorizer8eb11031f6b64eaaad9ff0fd3b0f5b80',
     'preprocessor': None,
     'save_overwrite': True,
     'smooth_idf': True,
     'stop_words': None,
     'strip_accents': None,
     'sublinear_tf': False,
     'token_pattern': '(?u)\\b\\w\\w+\\b',
     'tokenizer': None,
     'use_idf': True,
     'vocabulary': None}

Gerekirse, varsayılan parametreleri değiştirebilirsiniz.  Aşağıdaki kod ile ayıklanan karakter n-karakter Üçe-gram ve 4 gram ayıklamak için gram (4,4) gelen (3,4) için aralığını değiştirebilirsiniz:

```python
text_classifier.set_step_params_by_name("text_char_ngrams", ngram_range =(3,4)) 
text_classifier.get_step_params_by_name("text_char_ngrams")
```
    {'analyzer': 'char_wb',
     'binary': False,
     'decode_error': 'strict',
     'dtype': numpy.float32,
     'encoding': 'utf-8',
     'hashing': False,
     'input': 'content',
     'input_col': 'NltkPreprocessor5283a730506549cc880f074e750607b0',
     'lowercase': True,
     'max_df': 1.0,
     'max_features': None,
     'min_df': 3,
     'n_hashing_features': None,
     'ngram_range': (3, 4),
     'norm': 'l2',
     'output_col': 'NGramsVectorizer8eb11031f6b64eaaad9ff0fd3b0f5b80',
     'preprocessor': None,
     'save_overwrite': True,
     'smooth_idf': True,
     'stop_words': None,
     'strip_accents': None,
     'sublinear_tf': False,
     'token_pattern': '(?u)\\b\\w\\w+\\b',
     'tokenizer': None,
     'use_idf': True,
     'vocabulary': None}

### <a name="export-the-parameters-to-a-file"></a>Parametreleri bir dosyaya dışarı aktarma
Gerekirse, yeniden düzenlenen parametreler adımıyla sığdırma modeli çalıştırarak modeli performansı en iyi duruma getirebilirsiniz:

```python
import os
params_file_path = os.path.join(data_dir, "params.tsv")
text_classifier.export_params(params_file_path)
```

## <a name="apply-the-classifier"></a>Sınıflandırıcı Uygula

Eğitilmiş metin sınıflandırıcı sınıfı tahminleri oluşturmak için test veri kümesinde Uygula:

```python
 df_test = text_classifier.predict(df_test)
```

    TextClassifier ::predict ==> start
    NltkPreprocessor::tatk_transform ==> start
    NltkPreprocessor::tatk_transform ==> end     Time taken: 0.05 mins
    NGramsVectorizer::tatk_transform ==> startNGramsVectorizer::tatk_transform ==> start
    
    NGramsVectorizer::tatk_transform ==> end     Time taken: 0.15 mins
    NGramsVectorizer::tatk_transform ==> end     Time taken: 0.37 mins
    VectorAssembler::transform ==> start, num of input records=7532
    (7532, 216393)
    (7532, 67230)
    all_features::
    (7532, 283623)
    Time taken: 0.03 mins
    VectorAssembler::transform ==> end
    LogisticRegression::tatk_predict_proba ==> start
    LogisticRegression::tatk_predict_proba ==> end   Time taken: 0.01 mins
    LogisticRegression::tatk_predict ==> start
    LogisticRegression::tatk_predict ==> end     Time taken: 0.01 mins
    Time taken: 0.46 mins
    TextClassifier ::predict ==> end
    Order of Labels in predicted probabilities saved to attribute label_order of the class object
    
<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Etiket</th>
      <th>metin</th>
      <th>olasılıklar</th>
      <th>Tahmin</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>7</td>
      <td>Kimden: v064mb9k@ubvmsd.cc.buffalo.edu (NEIL B....</td>
      <td>[0.0165036341329, 0.0548664746458, 0.020549685...</td>
      <td>12</td>
    </tr>
    <tr>
      <th>1</th>
      <td>5</td>
      <td>Kimden: Rick Mert &lt; rick@ee.uwm.edu &gt;\nSubject:...</td>
      <td>[0.025145498995, 0.125877400021, 0.03947047877...</td>
      <td>1</td>
    </tr>
    <tr>
      <th>2</th>
      <td>0</td>
      <td>Kimden: mathew &lt; mathew@mantis.co.uk &gt;\nSubject: r...</td>
      <td>[0.67566338235, 0.0150749738583, 0.00992439163...</td>
      <td>0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>17</td>
      <td>Kimden: bakken@cs.arizona.edu (Dave Bakken) \nSub...</td>
      <td>[0.146063943868, 0.00232465192179, 0.002442807...</td>
      <td>18</td>
    </tr>
    <tr>
      <th>4</th>
      <td>19</td>
      <td>Kimden: livesey@solntze.wpd.sgi.com (Jon Livesey...</td>
      <td>[0.670712265297, 0.017332269703, 0.01062429663...</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
</div>

## <a name="evaluate-model-performance"></a>Model performansını değerlendirme
[Değerlendirme Modülü](https://docs.microsoft.com/python/api/tatk.evaluation) sınama veri kümesi üzerinde eğitilen metin sınıflandırıcı doğruluğunu değerlendirir. Evaluate işlevi karışıklığı matris oluşturur ve bir makrosu F1 puan sağlar.

```python
 text_classifier.evaluate(df_test)          
```

    TextClassifier ::evaluate ==> start
    Time taken: 0.0 mins
    TextClassifier ::evaluate ==> end
    

Karışıklığı önlemek için normalleştirme matris görselleştirmenin olmadan çizim.


```python
evaluator.plot_confusion_matrix(normalize=False,
                                title='Confusion matrix, without normalization', 
                                print_confusion_matrix=False,
                                figsize=(8,8),
                                colors=None)
```

Normalleştirme olmadan karışıklığı Matrisi
    
Not Defteri çalıştırırken karışıklığı matris görüntülenir



Normalleştirilmiş karışıklığı matris görselleştirme için çizim.


```python
evaluator.plot_confusion_matrix(normalize=True,
                                title='Normalized Confusion matrix', 
                                print_confusion_matrix=False,
                                figsize=(8,8),
                                colors=None)
```

    Normalized confusion matrix
   
Not Defteri çalıştırırken karışıklığı matris görüntülenir

## <a name="save-the-pipeline"></a>Ardışık Düzen Kaydet
Sınıflandırma ardışık düzen bir zip dosyasına kaydedin. Ayrıca, word ngrams ve karakter n-gram metin dosyası olarak kaydedin.

```python
import os
working_dir = os.path.join(data_dir, 'outputs')  
if not os.path.exists(working_dir):
    os.makedirs(working_dir)

# you can save the trained model as a folder or a zip file
model_file = os.path.join(working_dir, 'sk_model.zip')    
text_classifier.save(model_file)
# %azureml upload outputs/models/sk_model.zip

```
    BaseTextModel::save ==> start
    TatkPipeline::save ==> start
    Time taken: 0.28 mins
    TatkPipeline::save ==> end
    Time taken: 0.38 mins
    BaseTextModel::save ==> end
    
```python
# for debugging, you can save the word n-grams vocabulary to a text file
word_vocab_file_path = os.path.join(working_dir, 'word_ngrams_vocabulary.tsv')
text_classifier.get_step_by_name("text_word_ngrams").save_vocabulary(word_vocab_file_path) 
# %azureml upload outputs/dictionaries/word_ngrams_vocabulary.pkl

# for debugging, you can save the character n-grams vocabulary to a text file
char_vocab_file_path = os.path.join(working_dir, 'char_ngrams_vocabulary.tsv')
text_classifier.get_step_by_name("text_char_ngrams").save_vocabulary(char_vocab_file_path) 
# %azureml upload outputs/dictionaries/char_ngrams_vocabulary.pkl
```

    save_vocabulary ==> start
    saving 216393 n-grams ...
    Time taken: 0.01 mins
    save_vocabulary ==> end
    save_vocabulary ==> start
    saving 67230 n-grams ...
    Time taken: 0.0 mins
    save_vocabulary ==> end
 
## <a name="load-the-pipeline"></a>Ardışık Düzen yükleme
Sınıflandırma ardışık düzen ve word ngrams ve karakter n-gram inferencing için yük:

```python
# in order to deploy the trained model, you have to load the zip file of the classifier pipeline
loaded_text_classifier = TextClassifier.load(model_file)

from tatk.feature_extraction import NGramsVectorizer
word_ngram_vocab = NGramsVectorizer.load_vocabulary(word_vocab_file_path)
char_ngram_vocab = NGramsVectorizer.load_vocabulary(char_vocab_file_path)
```
    BaseTextModel::load ==> start
    TatkPipeline::load ==> start
    Time taken: 0.14 mins
    TatkPipeline::load ==> end
    Time taken: 0.15 mins
    BaseTextModel::load ==> end
    loading 216393 n-grams ...
    loading 67230 n-grams ...
    

## <a name="test-the-pipeline"></a>Ardışık Düzen test

Bir test veri değerlendirmek için yüklü metin sınıflandırma ardışık Uygula:

```python
predictions = loaded_text_classifier.predict(df_test)
loaded_evaluator = loaded_text_classifier.evaluate(predictions)
loaded_evaluator.get_metrics('macro_f1')
```
    TextClassifier ::predict ==> start
    NltkPreprocessor::tatk_transform ==> start
    NltkPreprocessor::tatk_transform ==> end     Time taken: 0.05 mins
    NGramsVectorizer::tatk_transform ==> startNGramsVectorizer::tatk_transform ==> start
    
    NGramsVectorizer::tatk_transform ==> end     Time taken: 0.14 mins
    NGramsVectorizer::tatk_transform ==> end     Time taken: 0.36 mins
    VectorAssembler::transform ==> start, num of input records=7532
    (7532, 216393)
    (7532, 67230)
    all_features::
    (7532, 283623)
    Time taken: 0.03 mins
    VectorAssembler::transform ==> end
    LogisticRegression::tatk_predict_proba ==> start
    LogisticRegression::tatk_predict_proba ==> end   Time taken: 0.01 mins
    LogisticRegression::tatk_predict ==> start
    LogisticRegression::tatk_predict ==> end     Time taken: 0.01 mins
    Time taken: 0.45 mins
    TextClassifier ::predict ==> end
    Order of Labels in predicted probabilities saved to attribute label_order of the class object
    TextClassifier ::evaluate ==> start
    Time taken: 0.0 mins
    TextClassifier ::evaluate ==> en
    
    0.82727029243808903

## <a name="operationalization-deploy-and-consume"></a>Operationalization: dağıtma ve kullanma

Bu bölümde, dağıttığınız bir Azure Machine Learning web hizmetini kullanarak olarak metin sınıflandırma ardışık düzen [Azure Machine Learning CLI](https://docs.microsoft.com/azure/machine-learning/desktop-workbench/cli-for-azure-machine-learning). Ardından, eğitim ve puanlama web hizmetini kullanabilir.

**Azure CLI, Azure aboneliğinizle oturum açın**

Kullanarak bir [Azure](https://azure.microsoft.com/) hesap geçerli bir abonelik ile aşağıdaki CLI komutu kullanarak oturum açın:
<br>`az login`

+ Başka bir Azure aboneliğine geçmeniz komutu kullanın:
<br>`az account set --subscription [your subscription name]`

+ Geçerli model yönetim hesabı görmek için komutu kullanın:
  <br>`az ml account modelmanagement show`

**Oluşturun ve dağıtım ortamınızı ayarlayın**

Yalnızca dağıtım ortamınızı bir kez ayarlamanız gerekir. Henüz yoksa, şimdi kullanarak dağıtım ortamınızı ayarlayın [bu yönergeleri](https://docs.microsoft.com/azure/machine-learning/desktop-workbench/deployment-setup-configuration#environment-setup). 

1. Azure Machine Learning ortamı, model yönetim hesabı ve kaynak grubu aynı bölgede bulunan emin olun.

1. Blob depolama alanından dağıtım yapılandırması dosyasını indirin ve yerel olarak kaydedin:

   ```python
   # Download the deployment config file from Blob storage `url` and save it locally under `file_name`:
   deployment_config_file_url = 'https://aztatksa.blob.core.windows.net/dailyrelease/tatk_deploy_config.yaml'
   deployment_config_file_path=os.path.join(resources_dir, 'tatk_deploy_config.yaml')
   import urllib.request
   urllib.request.urlretrieve(deployment_config_file_url, deployment_config_file_path)
   ```

1. İndirilen kaynaklarınızı yansıtmak için dağıtım yapılandırma dosyasını güncelleştir:

   ```python
   web_service_name = 'please type your web service name'
   working_directory= os.path.join(resources_dir, 'deployment') 

   web_service = text_classifier.deploy(web_service_name= web_service_name, 
                          config_file_path=deployment_config_file_path,
                          working_directory= working_directory)  
   ```

1. O eğitilen model başarıyla dağıtıldığından, yeni veri kümesi üzerinde Puanlama web hizmetini çağırmak:

   ```python
   print("Service URL: {}".format(web_service._service_url))
   print("Service Key: {}".format(web_service._api_key))
   ```

1. Web hizmeti adı kullanarak herhangi bir zamanda yükleyin:

   ```python
   from tatk.operationalization.csi.csi_web_service import CsiWebService
   url = "<please type the service URL here>"
   key = "<please type the service Key here>"
   web_service = CsiWebService(url, key)
   ```

1. Web hizmeti 20 newsgrpoups veri kümesinden alınan geçen iki e-posta gövdesi sınayın:

   ```python
   # Example input data for scoring
   import json
   dict1 ={}
   dict1["recordId"] = "a1" 
   dict1["data"]= {}
   dict1["data"]["text"] = """
   I'd be interested in a copy of this code if you run across it.
   (Mail to the author bounced)
    > / hpldsla:comp.graphics / email-address-removed / 12:53 am  May 13,
    1993 /
    > I fooled around with this problem a few years ago, and implemented a
    > simple method that ran on a PC.
    > was very simple - about 40 or 50 lines of code.
    . . .
    > Somewhere I still have it
    > and could dig it out if there was interest.
   """
   
   dict2 ={}
   dict2["recordId"] = "b2"
   dict2["data"] ={}
   dict2["data"]["text"] = """
   >>Could the people discussing recreational drugs such as mj, lsd, mdma, etc.,
   >>take their discussions to alt.drugs? Their discussions will receive greatest
   >>contribution and readership there. The people interested in strictly
   >>"smart drugs" (i.e. Nootropics) should post to this group. The two groups
   >>(alt.drugs & alt.psychoactives) have been used interchangably lately.
   >>I do think that alt.psychoactives is a deceiving name. alt.psychoactives
   >>is supposedly the "smart drug" newsgroup according to newsgroup lists on
   >>the Usenet. Should we establish an alt.nootropics or alt.sdn (smart drugs &
   >>nutrients)? I have noticed some posts in sci.med.nutrition regarding
   >>"smart nutrients." We may lower that groups burden as well.
   >   

   I was wondering if a group called 'sci.pharmacology' would be relevent.
   This would be used for a more formal discussion about pharmacological
   issues (pharmacodynamics, neuropharmacology, etc.)      

   Just an informal proposal (I don't know anything about the net.politics
   for adding a newsgroup, etc.)

   """

   dict_list =[dict1, dict2]
   data ={}
   data["values"] = dict_list
   input_data_json_str = json.dumps(data)
   print (input_data_json_str)
   prediction = web_service.score(input_data_json_str)
   prediction
   ```

   ```
   {"values": [{"recordId": "a1", "data": {"text": "\nI'd be interested in a copy of this code if you run across it.\n(Mail to the author bounced)\n > / hpldsla:comp.graphics / email-address-removed / 12:53 am  May 13,\n 1993 /\n > I fooled around with this problem a few years ago, and implemented a\n > simple method that ran on a PC.\n > was very simple - about 40 or 50 lines of code.\n . . .\n > Somewhere I still have it\n > and could dig it out if there was interest.\n"}}, {"recordId": "b2", "data": {"text": "\n>>Could the people discussing recreational drugs such as mj, lsd, mdma, etc.,\n>>take their discussions to alt.drugs? Their discussions will receive greatest\n>>contribution and readership there. The people interested in strictly\n>>\"smart drugs\" (i.e. Nootropics) should post to this group. The two groups\n>>(alt.drugs & alt.psychoactives) have been used interchangably lately.\n>>I do think that alt.psychoactives is a deceiving name. alt.psychoactives\n>>is supposedly the \"smart drug\" newsgroup according to newsgroup lists on\n>>the Usenet. Should we establish an alt.nootropics or alt.sdn (smart drugs &\n>>nutrients)? I have noticed some posts in sci.med.nutrition regarding\n>>\"smart nutrients.\" We may lower that groups burden as well.\n>\n\nI was wondering if a group called 'sci.pharmacology' would be relevent.\nThis would be used for a more formal discussion about pharmacological\nissues (pharmacodynamics, neuropharmacology, etc.)\n\nJust an informal proposal (I don't know anything about the net.politics\nfor adding a newsgroup, etc.)\n\n"}}]}
   F1 2018-05-02 00:10:58,272 INFO Web service scored. 
    
   '{"values": [{"recordId": "b2", "data": {"text": "\\n>>Could the people discussing recreational drugs such as mj, lsd, mdma, etc.,\\n>>take their discussions to alt.drugs? Their discussions will receive greatest\\n>>contribution and readership there. The people interested in strictly\\n>>\\"smart drugs\\" (i.e. Nootropics) should post to this group. The two groups\\n>>(alt.drugs & alt.psychoactives) have been used interchangably lately.\\n>>I do think that alt.psychoactives is a deceiving name. alt.psychoactives\\n>>is supposedly the \\"smart drug\\" newsgroup according to newsgroup lists on\\n>>the Usenet. Should we establish an alt.nootropics or alt.sdn (smart drugs &\\n>>nutrients)? I have noticed some posts in sci.med.nutrition regarding\\n>>\\"smart nutrients.\\" We may lower that groups burden as well.\\n>\\n\\nI was wondering if a group called \'sci.pharmacology\' would be relevent.\\nThis would be used for a more formal discussion about pharmacological\\nissues (pharmacodynamics, neuropharmacology, etc.)\\n\\nJust an informal proposal (I don\'t know anything about the net.politics\\nfor adding a newsgroup, etc.)\\n\\n", "class": 13}}, {"recordId": "a1", "data": {"text": "\\nI\'d be interested in a copy of this code if you run across it.\\n(Mail to the author bounced)\\n > / hpldsla:comp.graphics / email-address-removed / 12:53 am  May 13,\\n 1993 /\\n > I fooled around with this problem a few years ago, and implemented a\\n > simple method that ran on a PC.\\n > was very simple - about 40 or 50 lines of code.\\n . . .\\n > Somewhere I still have it\\n > and could dig it out if there was interest.\\n", "class": 1}}]}'
   ```


## <a name="next-steps"></a>Sonraki adımlar

Azure Machine Learning paketi hakkında daha fazla bilgi için metin analizi bu makalelerde öğrenin:

+ Okuma [genel bakış paketini ve nasıl yükleneceği öğrenin](https://aka.ms/aml-packages/text).

+ Araştır [başvuru belgelerini](https://aka.ms/aml-packages/text) bu paket için.

+ Hakkında bilgi edinin [Azure Machine Learning için diğer Python paketlerini](reference-python-package-overview.md).

