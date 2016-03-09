## GET /reset-data

Untuk mereset data agar dapat mengulang melakukan test dari awal sampai akhir. 

### Response

```json
{
  "data": "success resetting data"
}
```

## POST /survei

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

## POST /daftar-rt-rw (Body: raw, application/json)

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

## POST /daftar-kk

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

## POST /responden-terpilih

Input daftar anggota keluarga di suatu KK yg terpilih, lalu mendapatkan anggota keluarga yg terpilih sebagai responden terpilih. 
Setelah menginput KK - KK yang terpilih di semua RT/RW yg terpilih, maka akan didapatkan 2 KK yg terpilih di masing-masing RT/RW 
yg terpilih. 

Total KK yg terpilih ada 8 KK yaitu dengan id KK sebagai berikut: `2, 18, 106, 104, 203, 211, 313, 312`

### Request (Body: raw, application/json)
```json
{
	"data" : { 
		"phoneId" : "desadua@yahoo.com",
		"kk_id" : 2,
		"responden_status" : "Asli",
		"kk_id_asli" : null,
		"item_kk" : [
			["Member 1 KK 1","L",30,"keterangan 1"],
			["Member 2 KK 1","P", 17, "keterangan 2"],
			["Member 3 KK 1","P", 25, "keterangan 3"],
			["Member 4 KK 1","P", 28, "keterangan 4"],
			["Member 5 KK 1","L", 25, "keterangan 4"]
		]
	}
}
```

### Response (application/json)
```json
{
  "data": {
    "status": "success",
    "terpilih": {
      "nama": "Member 5 KK 1",
      "nomor_urut_kuisioner": 9,
      "nama_kepala_keluarga": "Desi17_91",
      "rw_rt": "9 / 7",
      "status": "Asli"
    }
  }
}
```

Lakukan request ke `/responden-terpilih` sebanyak 8x request sesuai dengan total jumlah KK yg terpilih yaitu 8 KK dengan KK id yg sudah 
ditentukan diatas. Berikut adalah contoh request kedua:

### Request kedua (Body: raw, application/json)
```json
{
	"data" : { 
		"phoneId" : "desadua@yahoo.com",
		"kk_id" : 18,
		"responden_status" : "Asli",
		"kk_id_asli" : null,
		"item_kk" : [
			["Member 1 KK 2","L",30,"keterangan 1"],
			["Member 2 KK 2","P", 17, "keterangan 2"],
			["Member 3 KK 2","P", 25, "keterangan 3"],
			["Member 4 KK 2","P", 28, "keterangan 4"],
			["Member 5 KK 2","L", 25, "keterangan 4"]
		]
	}
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

