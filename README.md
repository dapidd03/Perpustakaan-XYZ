/*
 * Click nbfs://nbhost/SystemFileSystem/Templates/Licenses/license-default.txt to change this license
 * Click nbfs://nbhost/SystemFileSystem/Templates/Classes/Main.java to edit this template
 */
package tugasanalysis;

import java.util.ArrayList;
import javax.swing.JOptionPane;
import java.util.HashMap;
import java.util.Map;

/**
 *
 * @author Asus
 */

class Buku {
    String judul;
    String pengarang;

    public Buku(String judul, String pengarang) {
        this.judul = judul;
        this.pengarang = pengarang;
    }
}

class Admin {
    Map<String, String> adminDatabase = new HashMap<>();
    ArrayList<Buku> koleksiBuku = new ArrayList<>();
    ArrayList<String> daftarAnggota = new ArrayList<>();

    public Admin() {
        // Menambahkan beberapa contoh data admin ke dalam database
        adminDatabase.put("EbenJr", "190904");
        adminDatabase.put("Pawil", "301103");
        adminDatabase.put("Dapid", "030704");
    }

    public void tambahBuku(String judul, String pengarang) {
        Buku bukuBaru = new Buku(judul, pengarang);
        koleksiBuku.add(bukuBaru);
        JOptionPane.showMessageDialog(null, "Buku berhasil ditambahkan ke koleksi.");
    }
    
    public void tambahAnggota(String username) {
        daftarAnggota.add(username);
        JOptionPane.showMessageDialog(null, "Anggota berhasil ditambahkan ke daftar.");
    }


    public void lihatDaftarBuku() {
        StringBuilder bukuList = new StringBuilder("Daftar Buku:\n");
        for (Buku buku : koleksiBuku) {
            bukuList.append("Judul: ").append(buku.judul).append(", Pengarang: ").append(buku.pengarang).append("\n");
        }
        JOptionPane.showMessageDialog(null, bukuList.toString());
    }

    public void lihatDaftarAnggota() {
        StringBuilder anggotaList = new StringBuilder("Daftar Anggota:\n");
        for (String anggota : daftarAnggota) {
            anggotaList.append(anggota).append("\n");
        }
        JOptionPane.showMessageDialog(null, anggotaList.toString());
    }
}

class Anggota {
    String username;
    ArrayList<Buku> bukuPinjaman = new ArrayList<>();

    public Anggota(String username) {
        this.username = username;
    }

    public void pinjamBuku(Buku buku) {
        bukuPinjaman.add(buku);
        JOptionPane.showMessageDialog(null, "Buku berhasil dipinjam: " + buku.judul);
    }

    public void kembalikanBuku(Buku buku) {
        bukuPinjaman.remove(buku);
        JOptionPane.showMessageDialog(null, "Buku berhasil dikembalikan: " + buku.judul);
    }

    public void lihatBukuPinjaman() {
       StringBuilder bukuList = new StringBuilder("Buku yang Dipinjam:\n");
       for (int i = 0; i < bukuPinjaman.size(); i++) {
           Buku buku = bukuPinjaman.get(i);
           bukuList.append(i + 1).append(". Judul: ").append(buku.judul).append(", Pengarang: ").append(buku.pengarang).append("\n");
       }
       JOptionPane.showMessageDialog(null, bukuList.toString());
   }
}

public class Tugasanalysis {
    private static Map<String, String> memberDatabase = new HashMap<>();
    private static Admin admin;

    public static void main(String[] args) {
        admin = new Admin();
        // Menampilkan dialog untuk memilih Admin atau Anggota
        showUserTypeSelectionDialog(admin);
    }

    private static void showUserTypeSelectionDialog(Admin admin) {
        Object[] userTypes = {"Admin", "Anggota", "Exit"};
        int userTypeChoice = JOptionPane.showOptionDialog(null, "Silahkan pilih jenis pengguna:", "Pilih Jenis Pengguna",
                JOptionPane.YES_NO_CANCEL_OPTION, JOptionPane.QUESTION_MESSAGE, null, userTypes, userTypes[2]);

        switch (userTypeChoice) {
            case 0:
                // Admin
                showAdminLoginDialog(admin);
                break;
            case 1:
                // Anggota
                showMemberLoginOrRegisterDialog();
                break;
            case 2:
                // Exit
                JOptionPane.showMessageDialog(null, "Terima kasih! Sampai jumpa.");
                System.exit(0);
                break;
            default:
                // Exit on cancel
                JOptionPane.showMessageDialog(null, "Terima kasih! Sampai jumpa.");
                System.exit(0);
        }
    }

    private static void showAdminLoginDialog(Admin admin) {
        String username = JOptionPane.showInputDialog("Masukkan Username Admin :");
        String password = JOptionPane.showInputDialog("Masukkan Kata Sandi Admin :");

        if (checkLogin(username, password, admin.adminDatabase)) {
            JOptionPane.showMessageDialog(null, "Login berhasil! Selamat datang, " + username);
            showAdminMenu(admin);
        } else {
            JOptionPane.showMessageDialog(null, "Login gagal. Periksa kembali nama pengguna dan kata sandi Admin Anda.");
            showUserTypeSelectionDialog(admin);
        }
    }

    private static void showAdminMenu(Admin admin) {
        Object[] adminOptions = {"Tambah Buku", "Lihat Daftar Buku", "Lihat Daftar Anggota", "Logout"};
        int adminChoice = JOptionPane.showOptionDialog(null, "Silahkan pilih opsi Admin:", "Admin Menu",
                JOptionPane.YES_NO_CANCEL_OPTION, JOptionPane.QUESTION_MESSAGE, null, adminOptions, adminOptions[3]);

        switch (adminChoice) {
            case 0:
                // Tambah Buku
                showTambahBukuDialog(admin);
                break;
            case 1:
                // Lihat Daftar Buku
                admin.lihatDaftarBuku();
                showAdminMenu(admin);
                break;
            case 2:
                // Lihat Daftar Anggota
                admin.lihatDaftarAnggota();
                showAdminMenu(admin);
                break;
            case 3:
                // Logout
                showUserTypeSelectionDialog(admin);
                break;
            default:
                // Exit on cancel
                JOptionPane.showMessageDialog(null, "Terima kasih! Sampai jumpa.");
                System.exit(0);
        }
    }

    private static void showTambahBukuDialog(Admin admin) {
        String judul = JOptionPane.showInputDialog("Masukkan judul buku:");
        String pengarang = JOptionPane.showInputDialog("Masukkan nama pengarang:");

        admin.tambahBuku(judul, pengarang);
        showAdminMenu(admin);
    }

    private static void showMemberLoginOrRegisterDialog() {
        Object[] options = {"Login", "Register", "Exit"};
        int choice = JOptionPane.showOptionDialog(null, "Silahkan pilih opsi:", "Anggota Login or Register",
                JOptionPane.YES_NO_CANCEL_OPTION, JOptionPane.QUESTION_MESSAGE, null, options, options[2]);

        switch (choice) {
            case 0:
                // Login
                showLoginDialog(memberDatabase, "Anggota");
                break;
            case 1:
                // Register
                showRegisterDialog(memberDatabase, "Anggota");
                break;
            case 2:
                // Exit
                JOptionPane.showMessageDialog(null, "Terima kasih! Sampai jumpa.");
                System.exit(0);
                break;
            default:
                // Exit on cancel
                JOptionPane.showMessageDialog(null, "Terima kasih! Sampai jumpa.");
                System.exit(0);
        }
    }

    private static void showLoginDialog(Map<String, String> database, String userType) {
        String username = JOptionPane.showInputDialog("Masukkan nama pengguna " + userType + ":");
        String password = JOptionPane.showInputDialog("Masukkan kata sandi " + userType + ":");

        if (checkLogin(username, password, database)) {
            JOptionPane.showMessageDialog(null, "Login berhasil! Selamat datang, " + username);
            showMemberMenu(username);
        } else {
            JOptionPane.showMessageDialog(null, "Login gagal. Periksa kembali nama pengguna dan kata sandi " + userType + " Anda.");
            showMemberLoginOrRegisterDialog();
        }
    }

private static void showRegisterDialog(Map<String, String> database, String userType) {
   String username = JOptionPane.showInputDialog("Masukkan nama pengguna " + userType + ":");
   String password = JOptionPane.showInputDialog("Masukkan kata sandi " + userType + ":");

   // Check if the username is already used
   if (database.containsKey(username)) {
       JOptionPane.showMessageDialog(null, "Nama pengguna sudah digunakan. Silakan pilih nama pengguna lain.");
       showRegisterDialog(database, userType);
   } else {
       // Save user information to the database
       database.put(username, password);
       JOptionPane.showMessageDialog(null, "Pendaftaran berhasil! Silakan login dengan akun baru " + userType + " Anda.");
       admin.tambahAnggota(username); // Add this line
       showMemberLoginOrRegisterDialog();
   }
}


    private static void showMemberMenu(String username) {
        Anggota anggota = new Anggota(username);
        Object[] memberOptions = {"Lihat Daftar Buku", "Pinjam Buku", "Kembalikan Buku", "Lihat Buku Pinjaman", "Logout"};
        int memberChoice = JOptionPane.showOptionDialog(null, "Silahkan pilih opsi Anggota:", "Anggota Menu",
                JOptionPane.YES_NO_CANCEL_OPTION, JOptionPane.QUESTION_MESSAGE, null, memberOptions, memberOptions[4]);

        switch (memberChoice) {
            case 0:
                // Lihat Daftar Buku
                admin.lihatDaftarBuku();
                showMemberMenu(username);
                break;
            case 1:
                // Pinjam Buku
                pinjamBuku(anggota);
                showMemberMenu(username);
                break;
            case 2:
                // Kembalikan Buku
                kembalikanBuku(anggota);
                showMemberMenu(username);
                break;
            case 3:
                // Lihat Buku Pinjaman
                anggota.lihatBukuPinjaman();
                int option = JOptionPane.showOptionDialog(
                        null,
                        "Pilih tindakan selanjutnya:",
                        "Opsi",
                        JOptionPane.YES_NO_OPTION,
                        JOptionPane.QUESTION_MESSAGE,
                        null,
                        new Object[]{"Kembali ke Menu Anggota", "Logout"},
                        "default");

                if (option == JOptionPane.YES_OPTION) {
                    showMemberMenu(username);
                } else {
                    showUserTypeSelectionDialog(admin);
                }
                showMemberMenu(username);
                break;
            case 4:
                // Logout
                showUserTypeSelectionDialog(admin);
                break;
            default:
                // Exit on cancel
                JOptionPane.showMessageDialog(null, "Terima kasih! Sampai jumpa.");
                System.exit(0);
        }
    }

    private static void pinjamBuku(Anggota anggota) {
        // Menampilkan daftar buku dari koleksi admin
        admin.lihatDaftarBuku();
        int bukuIndex = Integer.parseInt(JOptionPane.showInputDialog("Masukkan nomor buku yang ingin dipinjam:")) - 1;

        if (bukuIndex >= 0 && bukuIndex < admin.koleksiBuku.size()) {
            Buku bukuDipinjam = admin.koleksiBuku.get(bukuIndex);
            anggota.pinjamBuku(bukuDipinjam);
        } else {
            JOptionPane.showMessageDialog(null, "Nomor buku tidak valid.");
        }
    }

    private static void kembalikanBuku(Anggota anggota) {
        if (anggota.bukuPinjaman.isEmpty()) {
            JOptionPane.showMessageDialog(null, "Anda tidak memiliki buku yang dipinjam.");
            return;
        }

        // Menampilkan daftar buku yang dipinjam oleh anggota
        anggota.lihatBukuPinjaman();
        int bukuIndex = Integer.parseInt(JOptionPane.showInputDialog("Masukkan nomor buku yang ingin dikembalikan:")) - 1;

        if (bukuIndex >= 0 && bukuIndex < anggota.bukuPinjaman.size()) {
            Buku bukuDikembalikan = anggota.bukuPinjaman.get(bukuIndex);
            anggota.kembalikanBuku(bukuDikembalikan);
        } else {
            JOptionPane.showMessageDialog(null, "Nomor buku tidak valid.");
        }
    }

    private static boolean checkLogin(String username, String password, Map<String, String> database) {
        // Check if the username exists in the database
        if (database.containsKey(username)) {
            // Check if the entered password matches the stored password for the given username
            return database.get(username).equals(password);
        }
        // If the username is not found in the database, return false
        return false;
    }
}
