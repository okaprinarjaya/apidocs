# List API

* [GET /reset-data](#reset-data)
* [POST /survei](#survei)
* [POST /daftar-rt-rw](#daftar-rt-rw)
* [POST /daftar-kk](#daftar-kk)
* [GET /kk-terpilih/{email}](#kk-terpilih)
* [POST /responden-terpilih](#responden-terpilih)
* [GET /kk-tidak-terpilih/{email}/{rtId}](#kk-tidak-terpilih)
* [POST /responden-terpilih-pengganti](#responden-terpilih-pengganti)


## GET /reset-data <a id="reset-data"></a>

Untuk mereset data agar dapat mengulang melakukan test dari awal sampai akhir. 

### Request

`GET /reset-data`

### Response

```json
{
  "data": "success resetting data"
}
```

## POST /survei <a id="survei"></a>

Untuk melakukan identifikasi diri ke server. Survei apa? Siapa yg bertugas? Bertugas di desa mana? . Url service API ini dipanggil 
pada menu / tombol 'Download Survei' aplikasi mobile.

### Request (Body: raw, application/json)
```json
{
	"data" : {
		"phoneId" : "desadua@yahoo.com"
	}
}
```

### Response (application/json)
```json
{
  "data": {
    "nama_survei": "A vs B - Survei Tingkat Provinsi ",
    "nama_provinsi": "JAWA TIMUR",
    "nama_kabupaten_kota": "MALANG",
    "nama_kecamatan": "BANTUR",
    "nama_desa": "WONOKERTO"
  }
}
```

## POST /daftar-rt-rw <a id="daftar-rt-rw"></a>

Menginput informasi RT/RW yang ada di sebuah desa. Hasil dari proses input RT/RW adalah RT/RW terpilih.

### Request (Body: raw, application/json)
```json
{
	"data" : {
		"phoneId" : "desadua@yahoo.com",
		"jumlah_rw" : 10,
		"nilai_rw" : [8,7,15,12,6,9,18,9,7,11]
	}
}
```

### Response (application/json)
```json
{
  "data": [
    {
      "rt_id": 91,
      "rw": "9",
      "rt": "7",
      "keterangan": "Terpilih 1"
    },
    {
      "rt_id": 5,
      "rw": "1",
      "rt": "5",
      "keterangan": "Terpilih 2"
    },
    {
      "rt_id": 22,
      "rw": "3",
      "rt": "7",
      "keterangan": "Terpilih 3"
    },
    {
      "rt_id": 3,
      "rw": "1",
      "rt": "3",
      "keterangan": "Terpilih 4"
    }
  ]
}
```

Catat! RT/RW yang terpilih adalah dengan nomor: `91, 5, 22, 3` . Urutan nomor RT/RW terpilih itu akan digunakan untuk konsumsi layanan 
API input daftar KK.

## POST /daftar-kk <a id="daftar-kk"></a>

Menginput semua kepala keluarga yang ada di suatu RT/RW terpilih. Proses penginputan KK dilakukan berulang-ulang sebanyak RT/RW yg 
terpilih yaitu sebanyak 4 R/RW yaitu: 91, 5, 22, 3. Jadi Lakukan 4x request ke url service API `/daftar-kk` dengan pola request
sebagai berikut.

### Request pertama (Body: raw, application/json)
```json
{
	"data" : {
		"phoneId" : "desadua@yahoo.com",
		"rt_id" : 91,
		"item_kk" : [
			["Sasa1_91"],
			["Desi2_91"],
			["Ratna3_91"],
			["Sari4_91"],
			["Sari5_91"],
			["Sasa6_91"],
			["Desi7_91"],
			["Ratna8_91"],
			["Sari9_91"],
			["Sari10_91"],
			["Sasa11_91"],
			["Desi12_91"],
			["Ratna13_91"],
			["Sari14_91"],
			["Sari15_91"],
			["Sasa16_91"],
			["Desi17_91"],
			["Ratna18_91"],
			["Sari19_91"],
			["Sari20_91"]
		]
	}
}
```

### Response (application/json)
```json
{
  "data": {
    "status": "success"
  }
}
```

Lakukan lagi request ke url service API `/daftar-kk` untuk nomor selanjutnya yaitu: 5 .

### Request kedua (Body: raw, application/json)
```json
{
	"data" : {
		"phoneId" : "desadua@yahoo.com",
		"rt_id" : 5,
		"item_kk" : [
			["Sasa1_5"],
			["Desi2_5"],
			["Ratna3_5"],
			["Sari4_5"],
			["Sari5_5"],
			["Sasa6_5"],
			["Desi7_5"],
			["Ratna8_5"],
			["Sari9_5"],
			["Sari10_5"],
			["Sasa11_5"],
			["Desi12_5"],
			["Ratna13_5"],
			["Sari14_5"],
			["Sari15_5"],
			["Sasa16_5"],
			["Desi17_5"],
			["Ratna18_5"],
			["Sari19_5"],
			["Sari20_5"]
		]
	}
}
```

### Response (application/json)
```json
{
  "data": {
    "status": "success"
  }
}
```

Lakukan hal yg sama pada nomor yang selanjutnya.

## GET /kk-terpilih/{email} <a id="kk-terpilih"></a>

Mengambil / mendapatkan semua KK yg terpilih. 

### Request

`GET /kk-terpilih/desadua@yahoo.com`

### Response (application/json)

```json
{
  "data": {
    "status": "success",
    "rows": [
      {
        "survei_daftar_kk_id": 2,
        "nama_kk": "Desi17_91",
        "jenis_kelamin": "Untitled",
        "survei_daftar_rt_id": 91,
        "rt": "7",
        "rw": "9",
        "status": "Untitled"
      },
      {
        "survei_daftar_kk_id": 18,
        "nama_kk": "Sasa16_91",
        "jenis_kelamin": "Untitled",
        "survei_daftar_rt_id": 91,
        "rt": "7",
        "rw": "9",
        "status": "Untitled"
      },
      {
        "survei_daftar_kk_id": 106,
        "nama_kk": "Ratna18_5",
        "jenis_kelamin": "Untitled",
        "survei_daftar_rt_id": 5,
        "rt": "5",
        "rw": "1",
        "status": "Untitled"
      },
      {
        "survei_daftar_kk_id": 104,
        "nama_kk": "Desi7_5",
        "jenis_kelamin": "Untitled",
        "survei_daftar_rt_id": 5,
        "rt": "5",
        "rw": "1",
        "status": "Untitled"
      },
      {
        "survei_daftar_kk_id": 203,
        "nama_kk": "Desi2_22",
        "jenis_kelamin": "Untitled",
        "survei_daftar_rt_id": 22,
        "rt": "7",
        "rw": "3",
        "status": "Untitled"
      },
      {
        "survei_daftar_kk_id": 211,
        "nama_kk": "Sari15_22",
        "jenis_kelamin": "Untitled",
        "survei_daftar_rt_id": 22,
        "rt": "7",
        "rw": "3",
        "status": "Untitled"
      },
      {
        "survei_daftar_kk_id": 313,
        "nama_kk": "Sari20_3",
        "jenis_kelamin": "Untitled",
        "survei_daftar_rt_id": 3,
        "rt": "3",
        "rw": "1",
        "status": "Untitled"
      },
      {
        "survei_daftar_kk_id": 312,
        "nama_kk": "Sari19_3",
        "jenis_kelamin": "Untitled",
        "survei_daftar_rt_id": 3,
        "rt": "3",
        "rw": "1",
        "status": "Untitled"
      }
    ]
  }
}
```

## POST /responden-terpilih <a id="responden-terpilih"></a>

Input daftar anggota keluarga di suatu KK yg terpilih, lalu mendapatkan anggota keluarga yg terpilih sebagai responden terpilih. 
Setelah menginput semua KK di semua RT/RW yg terpilih, maka akan didapatkan 2 KK yg terpilih di masing-masing RT/RW 
yg terpilih. 

Total KK yg terpilih ada 8 KK yaitu dengan id KK sebagai berikut: `2, 18, 106, 104, 203, 211, 313, 312`

### Request (Body: raw, application/json)
```json
{
    "phoneId": "desadua@yahoo.com",
    "kk_id": 2,
    "responden_status": "Asli",
    "kk_id_asli": 0,
    "nomor_urut_kuisioner": 0,
    "item_kk": [
        {
            "nama": "Member 1 KK 1",
            "jenis_kelamin": "L",
            "usia": 30,
            "hubungan": "Hubungan dengan member lainnya",
            "catatan": "Keterangan 1"
        },
        {
            "nama": "Member 2 KK 1",
            "jenis_kelamin": "P",
            "usia": 17,
            "hubungan": "Hubungan dengan member lainnya",
            "catatan": "Keterangan 2"
        },
        {
            "nama": "Member 3 KK 1",
            "jenis_kelamin": "P",
            "usia": 25,
            "hubungan": "Hubungan dengan member lainnya",
            "catatan": "Keterangan 3"
        },
        {
            "nama": "Member 4 KK 1",
            "jenis_kelamin": "P",
            "usia": 28,
            "hubungan": "Hubungan dengan member lainnya",
            "catatan": "Keterangan 4"
        },
        {
            "nama": "Member 5 KK 1",
            "jenis_kelamin": "L",
            "usia": 25,
            "hubungan": "Hubungan dengan member lainnya",
            "catatan": "Keterangan 5"
        }
    ]
}
```

### Response (application/json)
```json
{
  "nama": "Member 5 KK 1",
  "nomor_urut_kuisioner": 9,
  "survei_daftar_rt_id": 91,
  "kk_id": 2,
  "nama_kepala_keluarga": "Desi17_91",
  "rw_rt": "9 / 7",
  "status": "Asli"
}
```

Lakukan request ke `/responden-terpilih` sebanyak 8x request sesuai dengan total jumlah KK yg terpilih yaitu 8 KK dengan KK id yg sudah 
ditentukan diatas. Berikut adalah contoh request kedua:

### Request kedua (Body: raw, application/json)
```json
{
    "phoneId": "desadua@yahoo.com",
    "kk_id": 18,
    "responden_status": "Asli",
    "kk_id_asli": 0,
    "nomor_urut_kuisioner": 0,
    "item_kk": [
        {
            "nama": "Member 1 KK 1",
            "jenis_kelamin": "L",
            "usia": 30,
            "hubungan": "Hubungan dengan member lainnya",
            "catatan": "Keterangan 1"
        },
        {
            "nama": "Member 2 KK 1",
            "jenis_kelamin": "P",
            "usia": 17,
            "hubungan": "Hubungan dengan member lainnya",
            "catatan": "Keterangan 2"
        },
        {
            "nama": "Member 3 KK 1",
            "jenis_kelamin": "P",
            "usia": 25,
            "hubungan": "Hubungan dengan member lainnya",
            "catatan": "Keterangan 3"
        },
        {
            "nama": "Member 4 KK 1",
            "jenis_kelamin": "P",
            "usia": 28,
            "hubungan": "Hubungan dengan member lainnya",
            "catatan": "Keterangan 4"
        },
        {
            "nama": "Member 5 KK 1",
            "jenis_kelamin": "L",
            "usia": 25,
            "hubungan": "Hubungan dengan member lainnya",
            "catatan": "Keterangan 5"
        }
    ]
}
```

### Response (application/json)
```json
{
  "data": {
    "status": "success",
    "terpilih": {
      "nama": "Member 4 KK 2",
      "nomor_urut_kuisioner": 10,
      "nama_kepala_keluarga": "Sasa16_91",
      "rw_rt": "9 / 7",
      "status": "Asli"
    }
  }
}
```

Begitu seterusnya sampai request ke-8

## GET /kk-tidak-terpilih/{email}/{rtId} <a id="kk-tidak-terpilih"></a>

Untuk melisting / menampilkan semua KK lainnya yg tidak terpilih. Dimana semua KK yg tidak terpilih itu berasal dari suatu RT/RW yg terpilih. Ini untuk memilih KK pengganti yg akan diinput anggota keluarganya utk mencari responden pengganti.

### Request

`GET /kk-tidak-terpilih/desadua@yahoo.com/91`

### Response

```json
{
  "data": {
    "status": "success",
    "rows": [
      {
        "survei_daftar_kk_id": 1,
        "nama_kk": "Desi12_91",
        "jenis_kelamin": "Untitled",
        "survei_daftar_rt_id": 91,
        "rt": "7",
        "rw": "9",
        "status": "Untitled"
      },
      {
        "survei_daftar_kk_id": 3,
        "nama_kk": "Desi2_91",
        "jenis_kelamin": "Untitled",
        "survei_daftar_rt_id": 91,
        "rt": "7",
        "rw": "9",
        "status": "Untitled"
      },
      {
        "survei_daftar_kk_id": 6,
        "nama_kk": "Ratna18_91",
        "jenis_kelamin": "Untitled",
        "survei_daftar_rt_id": 91,
        "rt": "7",
        "rw": "9",
        "status": "Untitled"
      },
      {
        "survei_daftar_kk_id": 7,
        "nama_kk": "Ratna3_91",
        "jenis_kelamin": "Untitled",
        "survei_daftar_rt_id": 91,
        "rt": "7",
        "rw": "9",
        "status": "Untitled"
      },
      {
        "survei_daftar_kk_id": 8,
        "nama_kk": "Ratna8_91",
        "jenis_kelamin": "Untitled",
        "survei_daftar_rt_id": 91,
        "rt": "7",
        "rw": "9",
        "status": "Untitled"
      },
      {
        "survei_daftar_kk_id": 9,
        "nama_kk": "Sari10_91",
        "jenis_kelamin": "Untitled",
        "survei_daftar_rt_id": 91,
        "rt": "7",
        "rw": "9",
        "status": "Untitled"
      },
      {
        "survei_daftar_kk_id": 10,
        "nama_kk": "Sari14_91",
        "jenis_kelamin": "Untitled",
        "survei_daftar_rt_id": 91,
        "rt": "7",
        "rw": "9",
        "status": "Untitled"
      },
      {
        "survei_daftar_kk_id": 11,
        "nama_kk": "Sari15_91",
        "jenis_kelamin": "Untitled",
        "survei_daftar_rt_id": 91,
        "rt": "7",
        "rw": "9",
        "status": "Untitled"
      },
      {
        "survei_daftar_kk_id": 12,
        "nama_kk": "Sari19_91",
        "jenis_kelamin": "Untitled",
        "survei_daftar_rt_id": 91,
        "rt": "7",
        "rw": "9",
        "status": "Untitled"
      },
      {
        "survei_daftar_kk_id": 13,
        "nama_kk": "Sari20_91",
        "jenis_kelamin": "Untitled",
        "survei_daftar_rt_id": 91,
        "rt": "7",
        "rw": "9",
        "status": "Untitled"
      },
      {
        "survei_daftar_kk_id": 14,
        "nama_kk": "Sari4_91",
        "jenis_kelamin": "Untitled",
        "survei_daftar_rt_id": 91,
        "rt": "7",
        "rw": "9",
        "status": "Untitled"
      },
      {
        "survei_daftar_kk_id": 15,
        "nama_kk": "Sari5_91",
        "jenis_kelamin": "Untitled",
        "survei_daftar_rt_id": 91,
        "rt": "7",
        "rw": "9",
        "status": "Untitled"
      },
      {
        "survei_daftar_kk_id": 16,
        "nama_kk": "Sari9_91",
        "jenis_kelamin": "Untitled",
        "survei_daftar_rt_id": 91,
        "rt": "7",
        "rw": "9",
        "status": "Untitled"
      },
      {
        "survei_daftar_kk_id": 17,
        "nama_kk": "Sasa11_91",
        "jenis_kelamin": "Untitled",
        "survei_daftar_rt_id": 91,
        "rt": "7",
        "rw": "9",
        "status": "Untitled"
      },
      {
        "survei_daftar_kk_id": 19,
        "nama_kk": "Sasa1_91",
        "jenis_kelamin": "Untitled",
        "survei_daftar_rt_id": 91,
        "rt": "7",
        "rw": "9",
        "status": "Untitled"
      },
      {
        "survei_daftar_kk_id": 20,
        "nama_kk": "Sasa6_91",
        "jenis_kelamin": "Untitled",
        "survei_daftar_rt_id": 91,
        "rt": "7",
        "rw": "9",
        "status": "Untitled"
      },
      {
        "survei_daftar_kk_id": 5,
        "nama_kk": "Ratna13_91",
        "jenis_kelamin": "Untitled",
        "survei_daftar_rt_id": 91,
        "rt": "7",
        "rw": "9",
        "status": "Untitled"
      },
      {
        "survei_daftar_kk_id": 4,
        "nama_kk": "Desi7_91",
        "jenis_kelamin": "Untitled",
        "survei_daftar_rt_id": 91,
        "rt": "7",
        "rw": "9",
        "status": "Untitled"
      }
    ]
  }
}
```

## POST /responden-terpilih-pengganti <a id="responden-terpilih-pengganti"></a>

Input daftar anggota keluarga di suatu KK yg tidak terpilih, lalu mendapatkan anggota keluarga yg terpilih sebagai responden terpilih.
PERHATIKAN! di json request data perbedaannya ada di `kk_id` , `kk_id_asli` , `nomor_urut_kuisioner` . 

`kk_id` didapatkan dari interaksi yg memanggil service `GET /kk-tidak-terpilih/{email}/{rtId}` .

`kk_id_asli` di-passing dari UI yang ada tampilan tombol PENGGANTI nya.

`nomor_urut_kuisioner` di-passing juga dari UI yang ada tampilan tombol PENGGANTI nya.


### Request (Body: raw, application/json)
```json
{
    "phoneId": "desadua@yahoo.com",
    "kk_id": 4,
    "responden_status": "Pengganti",
    "kk_id_asli": 2,
    "nomor_urut_kuisioner": 9,
    "item_kk": [
        {
            "nama": "Member 1 KK Ganti",
            "jenis_kelamin": "L",
            "usia": 30,
            "hubungan": "Hubungan dengan member lainnya",
            "catatan": "Keterangan 1"
        },
        {
            "nama": "Member 2 KK Ganti",
            "jenis_kelamin": "P",
            "usia": 17,
            "hubungan": "Hubungan dengan member lainnya",
            "catatan": "Keterangan 2"
        },
        {
            "nama": "Member 3 KK Ganti",
            "jenis_kelamin": "P",
            "usia": 25,
            "hubungan": "Hubungan dengan member lainnya",
            "catatan": "Keterangan 3"
        },
        {
            "nama": "Member 4 KK Ganti",
            "jenis_kelamin": "P",
            "usia": 28,
            "hubungan": "Hubungan dengan member lainnya",
            "catatan": "Keterangan 4"
        },
        {
            "nama": "Member 5 KK Ganti",
            "jenis_kelamin": "L",
            "usia": 25,
            "hubungan": "Hubungan dengan member lainnya",
            "catatan": "Keterangan 5"
        }
    ]
}
```

### Response (application/json)
```json
{
  "data": {
    "status": "success",
    "terpilih": {
      "nama": "Member 4 KK 2",
      "nomor_urut_kuisioner": 10,
      "nama_kepala_keluarga": "Sasa16_91",
      "rw_rt": "9 / 7",
      "status": "Pengganti"
    }
  }
}
```
