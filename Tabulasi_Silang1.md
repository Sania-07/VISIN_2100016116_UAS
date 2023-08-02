# Proses Pembuatan Tabulasi Silang Data Penyebab - Penyebab Kematian di Indonesia

## Alur Proses

1. Mengambil aktifkan Spreadsheet dan mendapatkan lembar kerja dengan nama 'Penyebab Kematian di Indonesia yang Dilaporkan - Clean'.

2. Menghitung jumlah baris dan kolom data yang ada pada lembar kerja tersebut.

3. Mendapatkan seluruh data (kecuali baris header) dari lembar kerja tersebut.

4. Membuat array kosong untuk menyimpan tahun (years) dan jenis (types) unik dari data.

5. Melakukan iterasi untuk setiap baris data, kemudian mengekstrak tahun dan jenis dari setiap baris dan menyimpannya pada array years dan types jika belum ada pada array tersebut.

6. Mengurutkan tahun secara ascending (menaik) pada array years.

7. Membuat lembar kerja baru dengan nama 'Kematian Berdasarkan Tahun dan Tipe'.

8. Menulis label 'Type' pada sel A1 dari lembar kerja baru dan menulis tahun-tahun unik pada baris pertama dari lembar kerja baru.

9. Menghitung total kematian untuk setiap jenis dan tahun, kemudian menulis hasilnya pada lembar kerja baru.

10. Untuk setiap jenis (row) dan tahun (column), fungsi akan melakukan iterasi pada seluruh data asli untuk menghitung total kematian yang sesuai dengan jenis dan tahun tertentu.

11. Hasil perhitungan total kematian kemudian akan dituliskan pada lembar kerja baru sesuai dengan posisi jenis dan tahunnya.

## Hasil Tabulasi Silang

Berikut adalah hasil tabulasi silang yang disimpan pada lembar kerja baru dengan nama 'Kematian Berdasarkan Tahun dan Tipe':

```
| Type                    | 2000 | 2001 | 2002 | ... | 2022 |
|-------------------------|------|------|------|-----|------|
| Bencana Alam            | 0    | 0    | 0    | ... | 0    |
| Bencana Non Alam dan... | 0    | 0    | 0    | ... | 0    |
| Bencana Sosial          | 0    | 0    | 0    | ... | 0    |
```

Data angka yang ada pada tabel di atas merupakan total kematian untuk setiap jenis bencana (Type) pada tiap tahun (Year). 

```javascript
function TypeAndYear() {
  let ss = SpreadsheetApp.getActive();
  let sheet = ss.getSheetByName('Penyebab Kematian di Indonesia yang Dilaporkan - Clean');
  let numberRows = sheet.getDataRange().getNumRows();
  let numberCols = sheet.getLastColumn();

  let data = sheet.getRange(2, 1, numberRows - 1, numberCols).getValues();

  // Get unique years and types
  let years = [];
  let types = [];
  for (let i = 0; i < data.length; i++) {
    let year = data[i][2];
    let type = data[i][1];
    if (years.indexOf(year) == -1) years.push(year);
    if (types.indexOf(type) == -1) types.push(type);
  }

  // Sort years in ascending order
  years.sort((a, b) => a - b);

  // Create new sheet
  let newSheet = ss.insertSheet('Kematian Berdasarkan Tahun dan Tipe');
  newSheet.getRange('A1').setValue('Type');
  for (let i = 0; i < years.length; i++) {
    newSheet.getRange(1, i + 2).setValue(years[i]);
  }

  // Calculate totals and add to sheet
  for (let i = 0; i < types.length; i++) {
    let type = types[i];
    newSheet.getRange(i + 2, 1).setValue(type);
    for (let j = 0; j < years.length; j++) {
      let year = years[j];
      let totalDeaths = 0;
      for (let k = 0; k < data.length; k++) {
        if (data[k][2] == year && data[k][1] == type) {
          totalDeaths += Number(data[k][4]);
        }
      }
      newSheet.getRange(i + 2, j + 2).setValue(totalDeaths);
    }
  }
}
```
'''
