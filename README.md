# <font color="#000080"> E-commerce Alesandora Dashboard ✨</font>

<font size="3"> Streamlit dapat digunakan dengan bahasa pemrograman Python. Jadi saya membuat file bernama dashboard dengan format .py jadi namanya menjadi dashboar.py</font>


```python
```python
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
import streamlit as st
from babel.numbers import format_currency
sns.set(style='dark')

def create_rfm_df(df):
    rfm_df = df.groupby(by="customer_id", as_index=False).agg({
        "order_delivered_customer_date": "max", 
        "order_id": "nunique",
        "payment_value": "sum"
    })
    rfm_df.columns = ["customer_id", "max_order_timestamp", "frequency", "monetary"]
    
    rfm_df["max_order_timestamp"] = rfm_df["max_order_timestamp"].dt.date
    recent_date = df["order_delivered_customer_date"].dt.date.max()
    rfm_df["recency"] = rfm_df["max_order_timestamp"].apply(lambda x: (recent_date - x).days)
    rfm_df.drop("max_order_timestamp", axis=1, inplace=True)
    
    return rfm_df

all_df = pd.read_csv("all.csv")

all_df["order_delivered_customer_date"] = pd.to_datetime(all_df["order_delivered_customer_date"])

min_date = all_df["order_delivered_customer_date"].min()
max_date = all_df["order_delivered_customer_date"].max()
 
with st.sidebar:
    # Menambahkan logo perusahaan
    st.image("https://github.com/dicodingacademy/assets/raw/main/logo.png")
    
    # Mengambil start_date & end_date dari date_input
    start_date, end_date = st.date_input(
        label='Rentang Waktu',min_value=min_date,
        max_value=max_date,
        value=[min_date, max_date]
    )

st.header('Alesandora Dashboard :sparkles:')
rfm_df = create_rfm_df(all_df)
st.subheader("Best Customer Based on RFM Parameters")
 
col1, col2, col3 = st.columns(3)
 
with col1:
    avg_recency = round(rfm_df.recency.mean(), 1)
    st.metric("Average Recency (days)", value=avg_recency)
 
with col2:
    avg_frequency = round(rfm_df.frequency.mean(), 2)
    st.metric("Average Frequency", value=avg_frequency)
 
with col3:
    avg_frequency = format_currency(rfm_df.monetary.mean(), "AUD", locale='es_CO') 
    st.metric("Average Monetary", value=avg_frequency)
 
fig, ax = plt.subplots(nrows=1, ncols=3, figsize=(35, 15))
colors = ["#90CAF9", "#90CAF9", "#90CAF9", "#90CAF9", "#90CAF9"]
 
sns.barplot(y="recency", x="customer_id", data=rfm_df.sort_values(by="recency", ascending=True).head(5), palette=colors, ax=ax[0])
ax[0].set_ylabel(None)
ax[0].set_xlabel("customer_id", fontsize=30)
ax[0].set_title("By Recency (days)", loc="center", fontsize=50)
ax[0].tick_params(axis='y', labelsize=30)
ax[0].tick_params(axis='x', labelsize=25, rotation=90)
 
sns.barplot(y="frequency", x="customer_id", data=rfm_df.sort_values(by="frequency", ascending=False).head(5), palette=colors, ax=ax[1])
ax[1].set_ylabel(None)
ax[1].set_xlabel("customer_id", fontsize=30)
ax[1].set_title("By Frequency", loc="center", fontsize=50)
ax[1].tick_params(axis='y', labelsize=30)
ax[1].tick_params(axis='x', labelsize=25, rotation=90)
 
sns.barplot(y="monetary", x="customer_id", data=rfm_df.sort_values(by="monetary", ascending=False).head(5), palette=colors, ax=ax[2])
ax[2].set_ylabel(None)
ax[2].set_xlabel("customer_id", fontsize=30)
ax[2].set_title("By Monetary", loc="center", fontsize=50)
ax[2].tick_params(axis='y', labelsize=30)
ax[2].tick_params(axis='x', labelsize=25, rotation=90)
 
st.pyplot(fig)
```
```


```python
```python
!pip install streamlit babel -q
!wget -q -O - ipv4.icanhazip.com
! streamlit run dashboard.py & npx localtunnel --port 8501
```
```

<font size="3">Jadi diatas merupakan kode untuk menjalankan dashboard.py yang akan dijalankan dilokal host</font>


```python
`!pip install streamlit babel -q` : Perintah ini digunakan untuk menginstal paket Streamlit dan Babel menggunakan pip. Penggunaan opsi -q dimaksudkan untuk mengatur mode "quiet", sehingga output instalasi tidak akan ditampilkan ke layar.
```


```python
`!wget -q -O - ipv4.icanhazip.com`: Perintah ini menggunakan wget untuk mengunduh data dari ipv4.icanhazip.com, kemungkinan untuk mendapatkan alamat IP publik dari host yang kita punya saat ini. Opsi -q digunakan untuk mode "quiet", mengakibatkan wget tidak menampilkan keluaran apapun ke layar, sedangkan opsi -O - menunjukkan bahwa output akan ditampilkan di stdout.
```


```python
`!streamlit run dashboard.py & npx localtunnel --port 8501`: Perintah tersebut merupakan gabungan dari dua langkah. Langkah pertama adalah menjalankan aplikasi Streamlit dengan perintah `streamlit run dashboard.py`, dimana `dashboard.py` adalah file yang digunakan sebagai argumen. Tanda `&` digunakan untuk menjalankan perintah tersebut di latar belakang. Kemudian, langkah kedua menggunakan perintah `npx localtunnel --port 8501` untuk membuat terowongan lokal menggunakan port 8501. Terowongan ini akan memungkinkan akses ke aplikasi Streamlit melalui internet dengan menggunakan URL yang dibuat oleh localtunnel.
```

<font size="3">Setelah selesai menginstall package tersebut dan berhasil, selanjutnya akan ada output yang berisi:</font>


```python
```python
34.125.178.191
[..................] / rollbackFailedOptional: verb npm-session c840043fcc3b18e
Collecting usage statistics. To deactivate, set browser.gatherUsageStats to False.


  You can now view your Streamlit app in your browser.

  Network URL: http://172.28.0.12:8501
  External URL: http://34.125.178.191:8501

npx: installed 22 in 2.013s
your url is: https://ninety-ads-love.loca.lt
```
```

<font size="3">Dari hasil output tersebut, maka akan kita dapatkan url dari dashbord yang sudah kita buat tadi. Jadi kita bisa menekan link dari "your url is **https://slimy-ideas-slide.loca.lt**" </font>

![Text Alternatif](https://s9.gifyu.com/images/SUbCo.png)

<font size="3"> Diatas merupakan tampilan apabila kita melakukan petunjuk sebelumnya, yaitu menekan link yang terdapat pada "your url is **https://slimy-ideas-slide.loca.lt**" Setelah itu kita akan mengisi Tunnel Password agar kita bisa membuka dashboard yang sudah kita buat</font>

<font size="3">**Tunnel Password** dapat dilihat dari hasil output code install package yang sudah kita jalankan</font>

![Text Alternatif](https://s9.gifyu.com/images/SUbU1.png)

<font size="3"> Angka yang ditunjukkan tersebut **34.125.178.191** merupakan Tunnel Password yang akan diminta atau kita butuhkan untuk membuka dashboard yang sudah dibuat.</font>

<font size="3">Setelah itu klik **"Submit"** dan Dashboard yang kita buat akan muncul</font>

![Text Alternatif](https://s9.gifyu.com/images/SUbZp.png)

<font size="3">Berikut merupakan tampilan dari <font size="4" color="#000080"><b>E-commerce Alesandora Dashboard ✨</b></font>
</font>

<font size="3">note: 
URL aplikasi Streamlit yang dijalankan secara lokal tidak dapat diakses dari luar jaringan lokal secara default karena langkah-langkah keamanan standar yang diterapkan oleh sistem operasi dan firewall. Oleh karena itu, aplikasi tersebut hanya dapat diakses melalui URL localhost seperti http://localhost:8501. Hal ini membatasi akses ke aplikasi hanya dari komputer yang menjalankannya.</font>
