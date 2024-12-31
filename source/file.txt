#!/bin/bash

# Nama file data
FILE="file.txt"

# Fungsi untuk menampilkan first_name dan last_name berdasarkan join_date
get_names_by_date() {
    echo "Masukkan tanggal join (format YYYY-MM-DD):"
    read join_date
    echo "First Name, Last Name for Join Date: $join_date"
    # Gunakan awk untuk mencari baris yang sesuai dengan tanggal join_date
    awk -F, -v date="$join_date" '$4 == date { print $1, $2 }' "$FILE"
}

# Fungsi untuk menampilkan deskripsi berdasarkan pekerjaan
get_description_by_job() {
    echo "Masukkan pekerjaan yang ingin dicari:"
    read job
    echo "Deskripsi untuk pekerjaan: $job"
    # Gunakan awk untuk mencari deskripsi berdasarkan pekerjaan
    awk -F, -v job="$job" '$3 == job { print $5 }' "$FILE"
}

# Menu utama
echo "Pilih opsi:"
echo "1. Tampilkan first_name dan last_name berdasarkan join_date"
echo "2. Tampilkan deskripsi berdasarkan pekerjaan"
echo "3. Keluar"
read -p "Pilih: " choice

case $choice in
    1)
        get_names_by_date
        ;;
    2)
        get_description_by_job
        ;;
    3)
        echo "Keluar dari program."
        exit 0
        ;;
    *)
        echo "Pilihan tidak valid!"
        ;;
esac