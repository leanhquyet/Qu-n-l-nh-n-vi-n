#include <iostream>
#include <string>
#include <iomanip>

using namespace std;

struct NhanVien {
    int maNV;
    string hoTen;
    string ngaySinh;
    double luong;
};

void inTieuDe() {
    cout << left << setw(10) << "Ma NV"
         << setw(25) << "Ho Ten"
         << setw(15) << "Ngay Sinh"
         << setw(18) << "Luong (Trieu)" << endl;
    cout << string(68, '-') << endl;
}

void inNhanVien(const NhanVien &nv) {
    cout << left << setw(10) << nv.maNV
         << setw(25) << nv.hoTen
         << setw(15) << nv.ngaySinh
         << setw(18) << fixed << setprecision(2) << nv.luong << endl;
}

void nhapDanhSach(NhanVien a[], int n) {
    for (int i = 0; i < n; i++) {
        cout << "\n--- Nhap thong tin nhan vien thu " << i + 1 << " ---" << endl;
        cout << "Nhap ma nhan vien: ";
        cin >> a[i].maNV;
        cin.ignore();
        
        cout << "Nhap ho ten: ";
        getline(cin, a[i].hoTen);
        
        cout << "Nhap ngay sinh (dd/mm/yyyy): ";
        getline(cin, a[i].ngaySinh);
        
        cout << "Nhap luong (trieu dong): ";
        cin >> a[i].luong;
    }
}

void xuatDanhSach(const NhanVien a[], int n) {
    inTieuDe();
    for (int i = 0; i < n; i++) {
        inNhanVien(a[i]);
    }
}

void bubbleSort(NhanVien a[], int n) {
    for (int i = 0; i < n - 1; i++) {
        for (int j = 0; j < n - i - 1; j++) {
            if (a[j].luong > a[j + 1].luong) {
                NhanVien temp = a[j];
                a[j] = a[j + 1];
                a[j + 1] = temp;
            }
        }
    }
}

void timKiemNhiPhan(const NhanVien a[], int n, double X) {
    int left = 0, right = n - 1;
    int viTriFind = -1;

    while (left <= right) {
        int mid = left + (right - left) / 2;
        if (a[mid].luong == X) {
            viTriFind = mid;
            break;
        } else if (a[mid].luong < X) {
            left = mid + 1;
        } else {
            right = mid - 1;
        }
    }

    if (viTriFind == -1) {
        cout << "\nKhong tim thay nhan vien nao co luong bang " << fixed << setprecision(2) << X << " trieu dong." << endl;
        return;
    }

    int start = viTriFind;
    while (start > 0 && a[start - 1].luong == X) {
        start--;
    }

    int end = viTriFind;
    while (end < n - 1 && a[end + 1].luong == X) {
        end++;
    }

    cout << "\n=== DANH SACH NHAN VIEN CO LUONG BANG " << fixed << setprecision(2) << X << " TRIEU DONG ===" << endl;
    inTieuDe();
    for (int i = start; i <= end; i++) {
        inNhanVien(a[i]);
    }
}

int main() {
    int n;
    cout << "Nhap so luong nhan vien n: ";
    cin >> n;

    if (n <= 0) {
        cout << "So luong nhan vien phai lon hon 0!" << endl;
        return 0;
    }

    NhanVien dsNV[100];

    nhapDanhSach(dsNV, n);
    cout << "\n=== DANH SACH NHAN VIEN VUA NHAP ===" << endl;
    xuatDanhSach(dsNV, n);

    bubbleSort(dsNV, n);
    cout << "\n=== DANH SACH NHAN VIEN SAU KHI SAP XEP TANG DAN THEO LUONG ===" << endl;
    xuatDanhSach(dsNV, n);

    double X;
    cout << "\nNhap muc luong X can tim kiem: ";
    cin >> X;
    timKiemNhiPhan(dsNV, n, X);

    return 0;
}
