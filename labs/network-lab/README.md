# Troubleshooting Log

Catatan kegagalan adalah guru terbaik. File ini adalah log dari tantangan teknis yang saya hadapi selama membangun dan mengelola lab ini. Dokumentasi ini membantu saya mengingat solusi untuk masalah yang sama di masa depan.

---

## 1. Kali Linux VM "File Not Found" (VirtualBox Import)
- **Problem**: Saat *setup* Kali VM, VirtualBox tidak mengenali file yang saya unduh.
- **Context**: Saya mencoba menggunakan menu "New" (yang mencari file `.iso`), padahal file yang diunduh adalah *pre-built appliance*.
- **Solution**: 
    1. Jangan gunakan menu "New".
    2. Gunakan menu **Machine** > **Add**.
    3. Pilih file berakhiran `.vbox` dari folder hasil ekstraksi.
- **Lesson Learned**: *Installer Images* (.iso) dan *Virtual Machines* (.vbox) adalah dua hal yang berbeda. Selalu periksa format file sebelum melakukan *import*.

## 2. Inter-VM Communication Failure (Networking)
- **Problem**: Kali Linux dan Ubuntu Server tidak bisa saling berkomunikasi (tidak bisa `ping`).
- **Context**: Keduanya berjalan, tapi terisolasi satu sama lain.
- **Root Cause**: Network Adapter di VirtualBox masih diset ke "NAT".
- **Solution**: 
    1. Masuk ke **Network Settings**.
    2. Ubah adapter menjadi **Host-Only Adapter**.
    3. Ini memastikan kedua VM berada di *subnet* yang sama (misal: `192.168.56.x`).
- **Lesson Learned**: Memahami *Network Mode* (NAT, Bridged, Host-Only) sangat krusial agar VM bisa saling berbicara dalam lingkungan lab.

## 3. Netplan Syntax Errors (Ubuntu Server)
- **Problem**: Konfigurasi IP statis di Ubuntu tidak mau berjalan (error saat `netplan apply`).
- **Context**: Mengedit file `/etc/netplan/01-netcfg.yaml`.
- **Solution**: Ternyata masalahnya adalah *tab indentation*. YAML sangat ketat, gunakan **spasi** saja. Setelah diganti, jalankan:
    ```bash
    sudo netplan apply
    ```
- **Lesson Learned**: *System administration* membutuhkan ketelitian tingkat tinggi. *Typo* sekecil spasi/tab bisa merusak konfigurasi sistem.

---
*Log ini akan terus diperbarui seiring dengan perkembangan lab saya.*
