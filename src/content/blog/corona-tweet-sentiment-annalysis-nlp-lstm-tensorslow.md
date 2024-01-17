---
author: Rusli Anwar
pubDatetime: 2023-01-05T20:39:00Z
title: Coronavirus Tweets Text Classification NLP LSTM Tensorflow [Accuracy 99%] 
postSlug: coronavirus-tweets-text-classification-nlp-lstm-tensorflow-accuracy-99%
featured: true
draft: false
tags:
  - docs
description:
  Coronavirus Tweets Text Classification NLP LSTM Tensorflow [Accuracy 99%].
---
Dalam era digital yang terus berkembang, teknologi Natural Language Processing (NLP) telah menjadi elemen penting dalam pemahaman dan pengelolaan bahasa manusia oleh komputer. Sebagai seorang penggiat teknologi dan analisis, saya akan menggali lebih dalam tentang penggunaan NLP dan perannya yang sangat penting dalam transformasi digital saat kini.

Pada kesempatan kali ini, saya akan mencoba melakukan Klasifikasi Teks (Analisis sentimen multikelas data tekstual) pada data Tweet Sentimen menggunakan Natural Language Processing (NLP) dan Long Short-Term Memory (LSTM) Architecture dengan tingkat validasi akurasi mencapai **99%**. Tweet tersebut didapat dari Twitter dan penandaannya dilakukan secara manual. Username dan nama pengguna telah di konversi menjadi kode untuk menghindari masalah privasi.

## Pendahuluan #TLDR
Dalam era digital yang terus berkembang, teknologi Natural Language Processing (NLP) telah menjadi elemen penting dalam pemahaman dan pengelolaan bahasa manusia oleh komputer. Sebagai seorang penggiat teknologi dan analisis, saya akan menggali lebih dalam tentang penggunaan NLP dan perannya yang sangat penting dalam transformasi digital saat kini.

## Apa itu Natural Language Processing (NLP)? #TLDR
Natural Language Processing (NLP) adalah cabang kecerdasan buatan yang berfokus pada interaksi antara komputer dan bahasa manusia. Hal ini bukan sekadar pengenalan dan pemahaman kata, tetapi juga melibatkan interpretasi konteks, sintaksis, dan semantik dalam sebuah kalimat. NLP memberikan kemampuan bagi mesin untuk memproses, menganalisis, dan merespons bahasa manusia dengan cara yang semakin mendekati kemampuan manusia itu sendiri.

## Graph TD
  1. A[Pemahaman Bahasa] -->|Tokenisasi| B[Pemrosesan Kata]
  1. B -->|Sintaksis| C[Pemahaman Struktur Kalimat]
  1. C -->|Semantik| D[Pemahaman Makna]
  1. D -->|Respon| E[Penghasilan Output]

## Table of contents

## Persiapan Datasets
Data yang digunakan dalam pengujian ini menggunakan dataset dari situs Kaggle.

**1. Datasets URL:** https://www.kaggle.com/datasets/datatattle/covid-19-nlp-text-classification/data
**2. Total dataset:** 44955 Sampel

## Import Library

Menyiapkan library yang dibutuhkan
```python
import nltk
import keras
import numpy as np
import pandas as pd
import seaborn as sns
import tensorflow as tf
import re,string,unicodedata
import matplotlib.pyplot as plt
nltk.download('stopwords')

from nltk.corpus import stopwords
from nltk.stem.porter import PorterStemmer
from wordcloud import WordCloud,STOPWORDS
from nltk.stem import WordNetLemmatizer
from nltk.tokenize import word_tokenize,sent_tokenize
from bs4 import BeautifulSoup
import re,string,unicodedata
from keras.preprocessing import text, sequence
from sklearn.metrics import classification_report,confusion_matrix,accuracy_score
from sklearn.model_selection import train_test_split
from string import punctuation
from keras.models import Sequential
from tensorflow.keras.preprocessing.text import Tokenizer
from tensorflow.keras.preprocessing.sequence import pad_sequences
from tensorflow.keras.layers import Dense, Embedding, Activation, Dropout, LSTM

[nltk_data] Downloading package stopwords to /root/nltk_data...
[nltk_data]   Unzipping corpora/stopwords.zip.
```
Fungsi untuk download data dari Kaggle API ke Google Colab
```python
from google.colab import files
files.upload()
```

Buat direktori untuk kaggle.json
```python
!mkdir ~/.kaggle/
!cp kaggle.json ~/.kaggle/
!chmod 600 ~/.kaggle/kaggle.json

!kaggle datasets download -d datatattle/covid-19-nlp-text-classification
!unzip covid-19-nlp-text-classification.zip
```
## Data Preparation
> [!NOTE]
> Kemudian, mengimpor library pandas untuk analisis data dan menampilkan dalam bentuk dataframe
```python
df=pd.read_csv('/content/Corona_NLP_test.csv', encoding='latin1')
df
```
![nlp test](https://github.com/ruslianwar/astro-paper/blob/main/src/assets/images/nlp%20test.png)

```python
df=pd.read_csv('/content/Corona_NLP_train.csv', encoding='latin1')
df
```
![nlp train](https://github.com/ruslianwar/astro-paper/blob/main/src/assets/images/nlp%20train.png)

```python
#Load datasets
train_data = pd.read_csv("Corona_NLP_train.csv", usecols=["OriginalTweet", "Sentiment"], encoding='ISO-8859-1')
test_data = pd.read_csv("Corona_NLP_test.csv", usecols=["OriginalTweet", "Sentiment"], encoding='ISO-8859-1')
```

```python
#Konversi label sentimen
label_mapping = {
    'Extremely Negative': 0,
    'Negative': 0,
    'Neutral': 1,
    'Positive': 2,
    'Extremely Positive': 2
}

train_data['Sentiment'] = train_data['Sentiment'].map(label_mapping)
test_data['Sentiment'] = test_data['Sentiment'].map(label_mapping)

y_train = train_data['Sentiment'].values
y_val = test_data['Sentiment'].values

invalid_labels_train = y_train[(y_train < 0) | (y_train >= 3)]
invalid_labels_val = y_val[(y_val < 0) | (y_val >= 3)]

if len(invalid_labels_train) > 0 or len(invalid_labels_val) > 0:
    print("Invalid labels found. Check label encoding.")
    print("Invalid labels in training set:", invalid_labels_train)
    print("Invalid labels in validation set:", invalid_labels_val)
else:
    print("Pengkodean label sudah benar. Lanjutkan pelatihan.")
```

> [!NOTE]
> Pemetaan label sentimen pada data latih dan data uji.
```python
train_data['Sentiment'] = train_data['Sentiment'].map(label_mapping)
test_data['Sentiment'] = test_data['Sentiment'].map(label_mapping)
```

## Natural Language Processing (NLP) dan Word Embedding
Dalam kode di bawah ini, terdapat fungsi data_cleaner yang dirancang untuk membersihkan dan memproses teks pada kolom **'OriginalTweet'** dalam data latih dan uji. Fungsi ini dapat diartikan sebagai tahap pra-pemrosesan dalam pengolahan data teks sebelum digunakan untuk pelatihan atau evaluasi model machine learning, khususnya pada tugas seperti klasifikasi teks.

```python
def data_cleaner(text):
    text = re.sub(r'@[A-Za-z0-9]+', '', text)
    text = re.sub(r'#', '', text)
    text = re.sub(r'https?:\/\/\S+', '', text)
    text = re.sub(r'[^\w\s]', '', text)
    return text

train_data['OriginalTweet'] = train_data['OriginalTweet'].apply(data_cleaner)
test_data['OriginalTweet'] = test_data['OriginalTweet'].apply(data_cleaner)
```

```python
nltk.download('punkt')
max_len = 100
tokenizer = Tokenizer(num_words=30000, oov_token='', filters='!"#$%&()*+,-./:;<=>?@[\\]^_`{|}~\t\n')

tokenizer.fit_on_texts(train_data['OriginalTweet'])
sequences_train = tokenizer.texts_to_sequences(train_data['OriginalTweet'])
padded_train = pad_sequences(sequences_train, padding='post', maxlen=max_len, truncating='post')

sequences_test = tokenizer.texts_to_sequences(test_data['OriginalTweet'])
padded_test = pad_sequences(sequences_test, padding='post', maxlen=max_len, truncating='post')

nltk.download('stopwords')
stop_words = set(stopwords.words('english'))

def remove_stopwords(text):
    tokens = nltk.word_tokenize(text)
    filtered_tokens = [word for word in tokens if word.lower() not in stop_words]
    return ' '.join(filtered_tokens)

train_data['OriginalTweet'] = train_data['OriginalTweet'].apply(remove_stopwords)
test_data['OriginalTweet'] = test_data['OriginalTweet'].apply(remove_stopwords)
```

```python
#Tokenization and Padding Setelah Penghapusan Stopword
from sklearn.preprocessing import LabelEncoder
tokenizer.fit_on_texts(train_data['OriginalTweet'])
sequences_train = tokenizer.texts_to_sequences(train_data['OriginalTweet'])
padded_train = pad_sequences(sequences_train, padding='post', maxlen=max_len, truncating='post')

sequences_test = tokenizer.texts_to_sequences(test_data['OriginalTweet'])
padded_test = pad_sequences(sequences_test, padding='post', maxlen=max_len, truncating='post')
```

## Label Encoding
```python
encoder = LabelEncoder()
train_labels = encoder.fit_transform(train_data['Sentiment'])
test_labels = encoder.transform(test_data['Sentiment'])

print(np.unique(y_train))
print(np.unique(y_test))
['Extremely Negative' 'Extremely Positive' 'Negative' 'Neutral' 'Positive']
['Extremely Negative' 'Extremely Positive' 'Negative' 'Neutral' 'Positive']

def encoder(data, enc=None):
  data[data=='Extremely Negative'] = 'Negative'
  data[data=='Extremely Positive'] = 'Positive'
  if(enc==None):
    from sklearn.preprocessing import OneHotEncoder
    onehot = OneHotEncoder()
    data_enc = onehot.fit_transform(np.array(data).reshape(-1,1)).toarray()
    return data_enc,onehot
  else:
    data_enc = enc.transform(np.array(data).reshape(-1,1)).toarray()
    return data_enc
y_train_enc, enc = encoder(y_train)
y_test_enc = encoder(y_test,enc)
```

> [!NOTE]
> Setelah itu melakukan pembagian dataset latih menjadi dua bagian: satu bagian untuk melatih model (data latih), dan satu bagian lagi untuk melakukan validasi model (data validasi).

```python
# Train-test split
X_train, X_val, y_train, y_val = train_test_split(padded_train, train_labels, test_size=0.2, random_state=42)
```

> [!NOTE]
> Kemudian lakukan pemeriksaan, apakah data pembagian telah berhasil.
```python
# Menampilkan pembagian datasets
total_samples = len(train_data) + len(test_data)
train_percentage = len(train_data) / total_samples * 100
test_percentage = len(test_data) / total_samples * 100

print(f"Total Samples: {total_samples}")
print(f"Train Set Size: {len(train_data)} ({train_percentage:.2f}%)")
print(f"Test Set Size: {len(test_data)} ({test_percentage:.2f}%)")
```

```
**Total Samples:** 44955
**Train Set Size:** 41157 (91.55%)
**Test Set Size:** 3798 (8.45%)
```

> [!NOTE]
> Menampilkan informasi terkait jumlah total kata unik atau kosakata yang terdapat dalam Tokenizer setelah proses tokenisasi dan pembentukan indeks.

```python
a = tokenizer.word_index
total_vocab = len(a)
total_vocab
```

## Building Neural Networks Model Menggunakan Sequential dan LSTM

```python
from tensorflow.keras.callbacks import ReduceLROnPlateau, EarlyStopping, ModelCheckpoint
from tensorflow.keras.optimizers import Adam

model = tf.keras.Sequential([
    tf.keras.layers.Embedding(input_dim=total_vocab + 2, output_dim=16, input_length=max_len),
    tf.keras.layers.Dropout(0.2),
    tf.keras.layers.Bidirectional(tf.keras.layers.LSTM(128, dropout=0.2, recurrent_dropout=0.4, return_sequences=True)),
    tf.keras.layers.LSTM(64, dropout=0.2, recurrent_dropout=0.4),
    tf.keras.layers.BatchNormalization(),
    tf.keras.layers.Dense(64, activation='relu'),
    tf.keras.layers.Dense(2, activation='softmax')
])

class CustomCallback(tf.keras.callbacks.Callback):
    def on_epoch_end(self, epoch, logs=None):
        if logs.get('val_accuracy') is not None and logs.get('val_accuracy') >= 0.95:
            print("\nAkurasi Validasi Telah Mencapai 95%. Berhenti.")
            self.model.stop_training = True

model.compile(optimizer='adam', loss='sparse_categorical_crossentropy', metrics=['accuracy'])
reduce_lr = ReduceLROnPlateau(monitor='val_loss', factor=0.2, patience=3, min_lr=1e-6)

callbacks = [
    EarlyStopping(patience=3, restore_best_weights=True),
    ModelCheckpoint(filepath='model_checkpoint.h5', save_best_only=True),
]
custom_callback = CustomCallback()
```

## Melatih Model
> [!NOTE]
> Selanjutnya kita akan melatih model yang sudah dibuat.
```python
history = model.fit(
    X_train, y_train,
    epochs=20,
    validation_split=0.2,
    callbacks=[reduce_lr, custom_callback] + callbacks
)
```
![melatih model](https://github.com/ruslianwar/astro-paper/blob/main/src/assets/images/2024-01-17_17-37.png)

## Membuat Plot Diagram

```python
plt.figure(figsize=(15, 5))

plt.subplot(1, 2, 1)
plt.plot(history.history['accuracy'])
plt.plot(history.history['val_accuracy'], linestyle='--')
plt.title('Accuracy Model')
plt.ylabel('Accuracy')
plt.xlabel('Epoch')
plt.legend(['Training Set', 'Validation Set'])
plt.grid(linestyle='--', linewidth=1, alpha=0.5)

plt.subplot(1, 2, 2)
plt.plot(history.history['loss'])
plt.plot(history.history['val_loss'], linestyle='--')
plt.title('Loss Model')
plt.ylabel('Loss')
plt.xlabel('Epoch')
plt.legend(['Training Set', 'Validation Set'])
plt.grid(linestyle='--', linewidth=1, alpha=0.5)

plt.show()
```
![plot diagram](https://github.com/ruslianwar/astro-paper/blob/main/src/assets/images/2024-01-17_17-39.png)

## Evaluasi Model

```python
test_loss, test_accuracy = model.evaluate(padded_test, test_labels)
print(f"Test Loss: {test_loss}, Test Accuracy: {test_accuracy}")
```
```
119/119 [==============================] - 12s 97ms/step - loss: 2.2684e-06 - accuracy: 1.0000
Test Loss: 2.2684216673951596e-06, Test Accuracy: 1.0
```
## Validasi Akurasi
```python
val_accuracy = history.history['val_accuracy'][-1]
print(f'Validation Accuracy: {val_accuracy * 100:.2f}%')
```
```
Validation Accuracy: 100.00% :+1:
```
