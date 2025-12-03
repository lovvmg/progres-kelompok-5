# progres-kelompok-5

#include <iostream>
#include <string>
#include <limits> // untuk clear input 
using namespace std;

#define MAX 500

struct Barang {
    int id;
    string nama;
    int jumlah;
};

Barang dataInv[MAX];
int countData = 0;

// Fungsi validasi input integer
int inputInteger(string pesan) {
    int nilai;
    while (true) {
        cout << pesan;
        if (cin >> nilai) {
            if (nilai >= 0) return nilai; // ga  boleh negatif
            else cout << "Input tidak boleh negatif!\n";
        } else {
            cout << "Input harus berupa angka!\n";
            cin.clear(); // reset error flag
        }
        cin.ignore(numeric_limits<streamsize>::max(), '\n'); // clear buffer
    }
}

// =============================
// TAMPILKAN DATA
// =============================
void tampilkanData() {
    if (countData == 0) {
        cout << "Data inventaris kosong!\n";
        return;
    }

    cout << "\n=== DATA INVENTARIS ===\n";
    for (int i = 0; i < countData; i++) {
        cout << "ID: " << dataInv[i].id
             << " | Nama: " << dataInv[i].nama
             << " | Jumlah: " << dataInv[i].jumlah << endl;
    }
}

// =============================
// INSERT TERURUT
// =============================
void tambahBarang() {
    if (countData >= MAX) {
        cout << "Inventaris penuh!\n";
        return;
    }

    Barang b;
    b.id = inputInteger("Masukkan ID        : ");

    cin.ignore(); // clear newline agar getline tidak langsung stop
    cout << "Masukkan Nama      : ";
    getline(cin, b.nama);

    b.jumlah = inputInteger("Masukkan Jumlah    : ");

    // Posisi sisip terurut
    int pos = 0;
    while (pos < countData && dataInv[pos].id < b.id) {
        pos++;
    }

    for (int i = countData; i > pos; i--) {
        dataInv[i] = dataInv[i - 1];
    }

    dataInv[pos] = b;
    countData++;

    cout << "Barang berhasil ditambahkan!\n";
}

// =============================
// BINARY SEARCH
// =============================
int binarySearch(int targetID) {
    int left = 0, right = countData - 1;

    while (left <= right) {
        int mid = (left + right) / 2;

        if (dataInv[mid].id == targetID) return mid;
        else if (dataInv[mid].id < targetID) left = mid + 1;
        else right = mid - 1;
    }
    return -1;
}

// =============================
// CARI BARANG
// =============================
void cariBarang() {
    if (countData == 0) {
        cout << "Data kosong!\n";
        return;
    }

    int idcari = inputInteger("Masukkan ID yang dicari: ");
    int idx = binarySearch(idcari);

    if (idx != -1) {
        cout << "\nBarang ditemukan!\n";
        cout << "ID: " << dataInv[idx].id
             << " | Nama: " << dataInv[idx].nama
             << " | Jumlah: " << dataInv[idx].jumlah << endl;
    } else {
        cout << "Barang tidak ditemukan.\n";
    }
}

// =============================
// UPDATE BARANG
// =============================
void updateBarang() {
    if (countData == 0) {
        cout << "Data kosong!\n";
        return;
    }

    int idedit = inputInteger("Masukkan ID barang yang akan diupdate: ");
    int idx = binarySearch(idedit);

    if (idx == -1) {
        cout << "Barang tidak ditemukan!\n";
        return;
    }

    cin.ignore();
    cout << "Masukkan nama baru  : ";
    getline(cin, dataInv[idx].nama);

    dataInv[idx].jumlah = inputInteger("Masukkan jumlah baru: ");
    cout << "Data berhasil diperbarui!\n";
}

// =============================
// HAPUS BARANG
// =============================
void hapusBarang() {
    if (countData == 0) {
        cout << "Data kosong!\n";
        return;
    }

    int idhapus = inputInteger("Masukkan ID yang akan dihapus: ");
    int idx = binarySearch(idhapus);

    if (idx == -1) {
        cout << "Barang tidak ditemukan!\n";
        return;
    }

    for (int i = idx; i < countData - 1; i++) {
        dataInv[i] = dataInv[i + 1];
    }
    countData--;

    cout << "Barang berhasil dihapus!\n";
}

// =============================
// TOTAL STOK
// =============================
void totalStok() {
    int total = 0;
    for (int i = 0; i < countData; i++) total += dataInv[i].jumlah;
    cout << "Total jumlah semua barang: " << total << endl;
}

// =============================
// TAMPILKAN BARANG HABIS
// =============================
void tampilkanStokHabis() {
    bool ada = false;
    cout << "\n=== BARANG STOK HABIS ===\n";

    for (int i = 0; i < countData; i++) {
        if (dataInv[i].jumlah == 0) {
            cout << "ID: " << dataInv[i].id << " | Nama: " << dataInv[i].nama << endl;
            ada = true;
        }
    }

    if (!ada) cout << "Tidak ada barang dengan stok 0\n";
}

// =============================
// SORT JUMLAH
// =============================
void sortByJumlah() {
    for (int i = 0; i < countData - 1; i++) {
        for (int j = 0; j < countData - i - 1; j++) {
            if (dataInv[j].jumlah > dataInv[j + 1].jumlah) {
                Barang temp = dataInv[j];
                dataInv[j] = dataInv[j + 1];
                dataInv[j + 1] = temp;
            }
        }
    }
    cout << "Data berhasil diurutkan berdasarkan jumlah!\n";
}

// =============================
// CLEAR DATA
// =============================
void clearData() {
    countData = 0;
    cout << "Semua data berhasil dihapus!\n";
}

// =============================
// MAIN PROGRAM
// =============================
int main() {
    int pilihan;

    while (true) {
        cout << "\n=== MENU ===\n";
        cout << "1. Tambah barang\n";
        cout << "2. Tampilkan data\n";
        cout << "3. Cari barang\n";
        cout << "4. Hapus barang\n";
        cout << "5. Update barang\n";
        cout << "6. Total stok barang\n";
        cout << "7. Tampilkan barang stok habis\n";
        cout << "8. Urutkan berdasarkan jumlah\n";
        cout << "9. Hapus semua data\n";
        cout << "10. Keluar\n";

        pilihan = inputInteger("Pilih menu (1-10): ");

        switch (pilihan) {
        case 1: tambahBarang(); break;
        case 2: tampilkanData(); break;
        case 3: cariBarang(); break;
        case 4: hapusBarang(); break;
        case 5: updateBarang(); break;
        case 6: totalStok(); break;
        case 7: tampilkanStokHabis(); break;
        case 8: sortByJumlah(); break;
        case 9: clearData(); break;
        case 10: return 0;
        default: cout << "Pilihan menu tidak tersedia!\n";
        }
    }
}
