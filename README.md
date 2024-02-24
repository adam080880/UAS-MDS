#!/usr/bin/env python
# coding: utf-8

# #### Identitas:

# In[1]:


print('NAMA:')
print('Muhamad Adam')
print('NIM:')
print('41521110006')


# #### Lakukan 11 instruksi di bawah ini dengan menggunakan dataset berikut:
# Catatan: Anda bebas menambah/mengedit kode dalam melakukan instruksi.

# In[2]:


#Importing required modules
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt

#Reading datasets

path = 'bank-train.csv' #sesuaikan dengan path masing-masing
df = pd.read_csv(path)

print(df)


# #### 1. Dataframe 'df' terdiri dari (a) ... columns dan (b) ... rows. Dataframe 'df' mengandung data dari (c) ... 'education' yang berbeda. Dataframe 'df' mengandung data hasil observasi pada bulan (lihat kolom month) (d) .... dan mempunyai 'duration' atau durasi dengan range antara (e) ... sampai (f) ...

# In[3]:


educations = df['education'].unique()
months = df['month'].unique()
duration_min = df['duration'].min()
duration_max = df['duration'].max()

print('header')
print(df.columns)
print('\n')

print('education:')
print(educations)
print('Jumlah edukasi: ' + str(educations.size))
print('\n')

print('bulan')
print(months)
print('\n')

print('min durasi')
print(duration_min)

print('max durasi')
print(duration_max)


# Answer:
# 
# a. 22
# 
# b. 32950
# 
# c. 8
# 
# d. Maret, April, Mei, Juni, Juli, Agustus, September, Oktober, November, dan Desember
# 
# e. 0
# 
# f. 4918
# 

# ###2. Berapa banyak orang yang status 'marital'-nya 'single' pada dataframe 'df'?

# In[4]:


marital_count = df[['marital']].groupby('marital').value_counts()
print(marital_count)


# ###3. Berapa usia paling muda dan paling tua? (lihat dari kolom 'age')

# In[5]:


min_age = df['age'].min()
max_age = df['age'].max()

print('Usia paling muda ' + str(min_age))
print('Usia paling tua ' + str(max_age))


# ###4. a. Buatlah sebuah dataframe baru bernama 'df_new' yang berisi 10 id dengan usia paling muda (sudah diurutkan dari usia termuda), sudah menikah, dan sudah mempunyai rumah. (lihat dari kolom age, marital, dan housing)
# ###b. Berapa banyak orang yang sudah menikah dan mempunyai rumah?

# In[6]:


df_new = df[['marital', 'age', 'job', 'housing']].query('marital == "married" & housing == "yes"').sort_values(by='age', ascending=True)
print('10 Teratas')
print(df_new.head(10))
print('\n')

print('Jumlah yang sudah menikah dan mempunyai rumah: ' + str(df_new.size))


# ###5. Dari seluruh data pada dataframe 'df', berapa orang yang saat ini mempunyai dan tidak mempunyai loan?

# In[7]:


df_loan = df[['loan']].groupby('loan').value_counts()

print(df_loan)


# ###6. Berapa banyak orang yang berumur di atas 35 tahun, sudah menggunakan cellular, namun kolom y-nya bernilai 1?

# In[8]:


df_age_gt_35_have_cellular = df.query('age > 35 & contact == "cellular" & y == 1')
print("Jumlah orang yang berumur di atas 35 tahun, sudah menggunakan cellular, dan kolom y bernilai 1: " + str(df_age_gt_35_have_cellular.size))


# #### 7. Ubah beberapa data pada kolom 'education' dengan aturan:
# 'original data','replace with this data':
# <br>
# 'basic.4y','Lulus-TK'
# <br>
# 'basic.6y','Lulus-SD'
# <br>
# 'basic.9y','Lulus-SMP'
# <br>
# 'high.school','Lulus-SMA'
# <br>
# 'illiterate','Buta-Huruf'
# <br>
# 'professional.course','Pendidikan-Profesional'
# <br>
# 'university.degree','Lulus-Kuliah'
# <br>
# 'Unknown','Others'

# In[9]:


education_mapping = {
  'basic.4y': 'Lulus-TK',
  'basic.6y': 'Lulus-SD',
  'basic.9y': 'Lulus-SMP',
  'high.school': 'Lulus-SMA',
  'illiterate': 'Buta-Huruf',
  'professional.course': 'Pendidikan-Profesional',
  'university.degree': 'Lulus-Kuliah',
  'unknown': 'Others'
}

df_replace_education = df.copy()
df_replace_education['education'] = df_replace_education['education'].replace(education_mapping)

print('Sebelum di replace')
print(df['education'].unique())

print('Setelah di replace')
print(df_replace_education['education'].unique())


# #### 8. Drop semua row pada 'df' yang data 'duration'-nya di bawah 100

# In[10]:


df_remove_100_lt_100 = df.copy()
df_remove_100_lt_100 = df.drop(df_remove_100_lt_100[df_remove_100_lt_100['duration'] < 100].index)

print('Sebelum di drop di bawah 100')
print('duration minimal: ' + str(df['duration'].min()))
print('jumlah: ' + str(df.size))

print('Setelah di drop di bawah 100')
print('duration minimal: ' + str(df_remove_100_lt_100['duration'].min()))
print('jumlah: ' + str(df_remove_100_lt_100.size))


# #### 9. Tambahkan kolom 'kel_usia' pada dataframe 'df' dan isi berdasarkan aturan berikut:
# - usia 17-30 diisi dengan 'dewasa_awal'
# - usia 31-45 diisi dengan 'dewasa_akhir'
# - usia 46-59 diisi dengan 'lansia_awal'
# - usia 60-75 diisi dengan 'lansia'
# - usia >75 diisi dengan 'lansia_akhir'

# In[11]:


conditions = [
    (df['age'] >= 17) & (df['age'] <= 30),
    (df['age'] >= 31) & (df['age'] <= 45),
    (df['age'] >= 46) & (df['age'] <= 59),
    (df['age'] >= 60) & (df['age'] <= 75),
    (df['age'] > 75)
]

choices = ['dewasa_awal', 'dewasa_akhir', 'lansia_awal', 'lansia', 'lansia_akhir']

df['kel_usia'] = np.select(conditions, choices, default='Unknown')

result = df.groupby('kel_usia')['kel_usia'].count()

print(result)


# ###10. a. Buatlah sebuah bar plot berdasarkan dataframe 'df' yang menunjukkan kelompok usia (menjadi sumbu x) dengan rataan durasi (menjadi sumbu y) dan ditinjau berdasarkan nilai kolom 'y'.
# Hint: bentuk plot seperti pada slide 155 modul belajar.
# 
# ###b. Buatlah histogram yang menunjukkan sebaran kolom 'duration' dan diberi warna sesuai nilai kolom 'y'
# Hint: bentuk plot seperti pada slide 158 modul belajar.

# In[12]:


grouped = df.groupby(['kel_usia', 'y'])['duration'].mean().reset_index()


plt.figure(figsize=(12, 6))
barplot = plt.bar(grouped[grouped['y'] == 0]['kel_usia'], grouped[grouped['y'] == 0]['duration'], color='g', alpha=0.7, label='y=0')
barplot = plt.bar(grouped[grouped['y'] == 1]['kel_usia'], grouped[grouped['y'] == 1]['duration'], color='y', alpha=0.7, label='y=1')


plt.xlabel('Kelompok Usia')
plt.ylabel('Rata-rata Durasi')
plt.title('Rata-rata Durasi Berdasarkan Kelompok Usia dan Nilai y')
plt.legend()


plt.tight_layout()
plt.show()


# ###11. a. Buatlah sebuah dataframe baru bernama 'df_model' yang berisi kolom age, housing, education, loan, euribor3m, dan y
# ###b. Lakukan encoding untuk kolom housing, education, dan loan
# ###c. Buatlah dua buah model klasifikasi menggunakan dataframe 'df_model' untuk memprediksi nilai kolom y (boleh memilih mau menggunakan algoritma apa: regresi logistik, SVM, decision tree, random forest, atau XGBoost)
# 
# Catatan: jangan lupa untuk membuang kolom kategorikal yang sudah di-encoding pada bagian b. Gunakan kolom y sebagai label atau output klasifikasi.

# In[13]:


df_model = df.copy()
df_model = df_model[['age', 'housing', 'education', 'loan', 'euribor3m', 'y']]

plt.figure(figsize=(12, 6))
plt.hist([df[df['y'] == 0]['duration'], df[df['y'] == 1]['duration']], bins=20, color=['y', 'g'], alpha=0.7, label=['y=0', 'y=1'])

plt.xlabel('Duration')
plt.ylabel('Frequency')
plt.title('Histogram of Duration Colored by y')
plt.legend()

plt.show()
