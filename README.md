# 🗓️ Pertemuan Kedelapan - UTS NO 6 
## 📚 Topik Utama 
Langkah-langkah membuat CRUD dengan Java Swing untuk entitas entitas MataKuliah dengan atribut: KodeMK, SKS, NamaMK, SemesterAjar

## 📑 Daftar Isi 

- [Java Swing Mata Kuliah](https://github.com/fauziaeka/PBO_SimulasiUTS/blob/main/FrameBuku.java)
- [MataKuliah DB](https://github.com/fauziaeka/PBO_SimulasiUTS/blob/main/BukuDB.java) 
- kesimpulan 

## 🖥️ Langkah- Langkah 
1. Buat project baru dengan nama "PBO_UTS
   
   ![GetImage](https://github.com/user-attachments/assets/3da403a7-e2f0-430f-94b6-618e84a89f8b)]
2. Membuat class JFrame Form pada project "PBO_UTS"
   
   ![GetImage (1)](https://github.com/user-attachments/assets/a13e64da-1021-4858-9406-fbc7680f46de)
   ![GetImage (2)](https://github.com/user-attachments/assets/b5d8b926-63c1-45cb-b345-3ad6b77562f6)

3. Membuat Desain pada JFrame Form. Gunakan JLable pada judul tampilan “DATA MATA KULIAH”, KodeMK, SKS, NamaMK, SemesterAjar. Gunakan JTextField untuk mengisi atau menginput data KodeMK, SKS, NamaMK, SemesterAjar. Gunakan JButton pada tombol Insert, Update, Delete, Clear, dan Exit. Gunakan JTable untuk menampilkan Table Data Mata Kuliah yang berisi 4 kolom yakni KodeMK, SKS, NamaMK, SemesterAjar.

   ![GetImage (3)](https://github.com/user-attachments/assets/f7f3b506-7182-4985-b152-b6b9f1bfc8e6)

4. Mengisi class MataKuliahDB. Class ini berfungsi untuk mengonversi ResultSet pada database menjadi TableModel agar dapat ditampilkan pada JTable di GUI Java Swing sehingga saat pengisian data kita tidak perlu menulis kodenya secara manual.

```
package pbo_simulasiuts;

import java.sql.ResultSet;
import java.sql.ResultSetMetaData;
import java.util.Vector;
import javax.swing.table.DefaultTableModel;
import javax.swing.table.TableModel;

public class BukuDB {

   public static TableModel resultSetToTableModel(ResultSet rs) {
        try {
            ResultSetMetaData metaData = rs.getMetaData();
            int numberOfColumns = metaData.getColumnCount();
            Vector columnNames = new Vector();

            // Get the column names
            for (int column = 0; column < numberOfColumns; column++) {
                columnNames.addElement(metaData.getColumnLabel(column + 1));
            }

            // Get all rows.
            Vector rows = new Vector();

            while (rs.next()) {
                Vector newRow = new Vector();

                for (int i = 1; i <= numberOfColumns; i++) {
                    newRow.addElement(rs.getObject(i));
                }

                rows.addElement(newRow);
            }

            return new DefaultTableModel(rows, columnNames);
        } catch (Exception e) {
            e.printStackTrace();

            return null;
        }
    }
} 
```



5. Membuat Table pada postgreSQL 

   ![GetImage (4)](https://github.com/user-attachments/assets/9b5dc71c-8a04-4a8f-a5c0-01a16d6025da)
 
6. Menyambungkan Netbeans dengan Database 

- Ketuk ‘Services’ , pilih ‘Databases’ kemudian pilih ‘New Connection’ 

![GetImage (5)](https://github.com/user-attachments/assets/18121277-e900-45b3-bf50-b370c3213327)

- Pilih PostgreSQL

![GetImage (6)](https://github.com/user-attachments/assets/ff4859ae-bdf8-4a00-b517-d2aac9f9587b)

- Isi kolom Database sesuai dengan nama Database pada PostgreeSQL, kemudian isi password sesuai dengan password PostgreeSQL

![GetImage (7)](https://github.com/user-attachments/assets/62169171-4586-4208-b219-7f1967387ed7)

- Select schema menjadi ‘public’

![GetImage (8)](https://github.com/user-attachments/assets/bec9db0d-e7e0-4f2f-bd83-cbd9fdd1f41c)

- Pada kolom input connection ini akan otomatis terisi, klik Finish

![GetImage (9)](https://github.com/user-attachments/assets/07a9f25a-fb5e-4671-a590-63d8890c8a36)

- Klik ‘Drive’ pilih ‘New Driver’

![GetImage (10)](https://github.com/user-attachments/assets/50bc3e6f-1607-4365-aa16-c9e7634fc655)

- Kembali pada bagian ‘Project’, klik ‘Library’ setelah itu klik ‘Add Library’

![GetImage (11)](https://github.com/user-attachments/assets/4ba0d532-d609-4736-ba30-b5206a73cd2b)

- Pilih ‘PostgreeSQL JDBC Driver’ kemudian klik ‘Add Library’

![GetImage (18)](https://github.com/user-attachments/assets/c60b4822-e386-4f7c-89e5-a01a06870deb)

![GetImage (12)](https://github.com/user-attachments/assets/6c4fa933-19e2-49ec-b181-444c8ea539bd)

7. Pada JFrame Form klik source, maka akan muncul source code dari desain secara otomatis. Isikan bagian tombol-tombol yang di desain agar bisa bekerja 

a. Mendeklarasikan import kelas-kelas yang diperlukan untuk database, pengolahan input, antarmuka pengguna dengan swing serta menyimpan detail koneksi database. 
```
package pbo_simulasiuts;

import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;
import java.util.logging.Level;
import java.util.logging.Logger;
import javax.swing.JOptionPane;
import javax.swing.table.TableModel;
import javax.swing.table.DefaultTableModel;
import pbo_simulasiuts.BukuDB;
import java.lang.System.Logger.*;

/**
*
* @author win 10
*/
public class FrameBuku extends javax.swing.JFrame {
   Connection conn;
   PreparedStatement pstmt;
   ResultSet rs;
    String driver = "org.postgresql.Driver";
    String koneksi = "jdbc:postgresql://localhost:5432/PBO_SimulasiUTS";
    String user = "postgres";
    String password = "123456";
    private BufferedReader input = new BufferedReader(new InputStreamReader(System.in));

```

b. Membuat method tampil untuk membuat koneksi ke database dengan menggunakan perintah DriverManager.getConnection dan menampilkan informasi tersebut dalam sebuah JTable dengan menggunakan perintah "SELECT * FROM Buku;" 
```
  /**
     * Creates new form FrameBuku
     */
    public FrameBuku() {
        initComponents();
        tampil();
    }
    public void tampil() {
        // TODO code application logi
        try (Connection conn = DriverManager.getConnection(koneksi, user, password); Statement stmt = conn.createStatement(); ResultSet rs = stmt.executeQuery("SELECT * FROM Buku ORDER BY ISBN")) {

            // Dapatkan model tabel yang ada
            DefaultTableModel model = (DefaultTableModel) tblBuku.getModel();

            // Kosongkan data tabel terlebih dahulu
            model.setRowCount(0);

            // Tambahkan data dari ResultSet ke model tabel
            while (rs.next()) {
                Object[] rowData = {rs.getString(1), rs.getString(2), rs.getString(3), rs.getString(4)}; 
                model.addRow(rowData);
            }
        } catch (SQLException ex) {
            // Tangani eksepsi
            System.err.println("Terjadi kesalahan: " + ex.getMessage());
        }
    }

```

c. Mengisi code pada btnInsert yang berfungsi untuk menambahkan data. Button ini berfungsi untuk menambahkan data pada 'Data Buku'. Sebagai pengguna, kita diminta untuk mengisi informasi mengenai ISBN, judul buku, tahun terbit buku serta penerbit buku. Setelah dimasukkan, kita bisa menekan button Insert. Jika berhasil, akan muncul pesan 'Data Berhasil disimpan'. 
```
 private void btnInsertActionPerformed(java.awt.event.ActionEvent evt) {                                          
        // TODO add your handling code here:
         String isbn, judul_buku, tahun_terbit, penerbit;
        try {
            Class.forName(driver);
            conn = DriverManager.getConnection(koneksi, user, password);
            conn.setAutoCommit(false);
            String sql = "INSERT INTO Buku(isbn, judul_buku, tahun_terbit, penerbit) VALUES(?,?,?,?)";
            pstmt = conn.prepareStatement(sql);

            isbn = tfISBN.getText();
            judul_buku = tfJudulBuku.getText();
            tahun_terbit = tfTahunTerbit.getText();
            penerbit = tfPenerbit.getText();
            
            pstmt.setLong(1, Long.parseLong(isbn));
            pstmt.setString(2, judul_buku);
            pstmt.setLong(3, Long.parseLong(tahun_terbit));
            pstmt.setString(4, penerbit);
            pstmt.executeUpdate();

            conn.commit();
            pstmt.close();
            conn.close();

            JOptionPane.showMessageDialog(null, "Data berhasil disimpan");
        } catch (ClassNotFoundException | SQLException ex) {
            JOptionPane.showMessageDialog(null, "Terjadi kesalahan: " + ex.getMessage());
        }

        bersih();
        tampil();                    
    }                                         

```

d. Mengisi code pada btnUpdate yang berfungsi untuk memperbarui data. Pertama-tama masukkan ISBN yang ingin diubah, kemudian masukkan informasi mengenai judul buku, tahun terbit buku serta penerbit buku baru. Jika berhasil, sistem akan mencetak pemberitahuan bahwa 'Data berhasil diupdate'. 
```
 private void btnUpdateActionPerformed(java.awt.event.ActionEvent evt) {                                          
        // TODO add your handling code here:                                   
        String isbn, judul_buku, tahun_terbit, penerbit;

        if (tfISBN.getText().isEmpty()) {
            JOptionPane.showMessageDialog(null, "ISBN buku harus diisi");
            return;
        }
        if (tfJudulBuku.getText().isEmpty()) {
            JOptionPane.showMessageDialog(null, "Judul buku harus diisi");
            return;
        }
        if (tfTahunTerbit.getText().isEmpty()) {
            JOptionPane.showMessageDialog(null, "Tahun terbit buku harus diisi");
            return;
        }
        if (tfPenerbit.getText().isEmpty()) {
            JOptionPane.showMessageDialog(null, "Penerbit buku harus diisi");
            return;
        }

        try {
            Class.forName(driver);
            String sql = "UPDATE Buku SET judul_buku = ?, tahun_terbit = ?, penerbit = ? WHERE ISBN = ?";
            conn = DriverManager.getConnection(koneksi, user, password);
            pstmt = conn.prepareStatement(sql);

            isbn = tfISBN.getText();
            judul_buku = tfJudulBuku.getText();
            tahun_terbit = tfTahunTerbit.getText();
            penerbit = tfPenerbit.getText();

            pstmt.setString(1, judul_buku);
            pstmt.setLong(2, Long.parseLong(tahun_terbit));
            pstmt.setString(3, penerbit);
            pstmt.setString(4, isbn);

            int rowsAffected = pstmt.executeUpdate();
            if (rowsAffected > 0) {
                JOptionPane.showMessageDialog(null, "Data berhasil diupdate");
                bersih(); // Pastikan metode bersih() ada
            } else {
                JOptionPane.showMessageDialog(null, "Data tidak ditemukan");
            }
        } catch (ClassNotFoundException | SQLException ex) {
            Logger.getLogger(FrameBuku.class.getName()).log(Level.SEVERE, null, ex);
            JOptionPane.showMessageDialog(null, "Terjadi kesalahan: " + ex.getMessage());
        } catch (NumberFormatException ex) {
            JOptionPane.showMessageDialog(null, "Input tidak valid: " + ex.getMessage());
        } finally {
            try {
                if (pstmt != null) {
                    pstmt.close();
                }
                if (conn != null) {
                    conn.close();
                }
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }

        tampil();
    }                                         

```

e. Mengisi code pada btnDelete yang berfungsi untuk menghapus data pada 'Data Buku'. Pengguna bisa memasukkan data yang ingin dihapus atau bisa juga dengan mengeklik data yang ingin dihapus pada kolom list. Kemudian akan muncul pesan konfirmasi apakah pengguna ingin menghapus data atau tidak. Jika 'iya' data otomatis akan dihapus dan jika 'tidak' maka data akan batal dihapus. 
```
private void btnDeleteActionPerformed(java.awt.event.ActionEvent evt) {                                          
        // TODO add your handling code here:                                     
        String isbn = tfISBN.getText();

        if (isbn.isEmpty()) {
            JOptionPane.showMessageDialog(null, "ISBN tidak boleh kosong!", "Error", JOptionPane.ERROR_MESSAGE);
            return;
        }

        try {
            Class.forName(driver);
            conn = DriverManager.getConnection(koneksi, user, password);

            int jawab = JOptionPane.showConfirmDialog(null, "Apakah Anda yakin ingin menghapus buku dengan ISBN: " + isbn + "?", "Konfirmasi Penghapusan", JOptionPane.YES_NO_OPTION);
            if (jawab == JOptionPane.YES_OPTION) {
                String deleteSql = "DELETE FROM Buku WHERE ISBN = ?";
                pstmt = conn.prepareStatement(deleteSql);
                pstmt.setString(1, isbn);

                int rowsAffected = pstmt.executeUpdate();
                if (rowsAffected > 0) {
                    JOptionPane.showMessageDialog(null, "Data berhasil dihapus");
                } else {
                    JOptionPane.showMessageDialog(null, "Data tidak ditemukan untuk ISBN: " + isbn);
                }
            } else {
                JOptionPane.showMessageDialog(this, "Penghapusan dibatalkan");
            }
        } catch (ClassNotFoundException | SQLException ex) {
            JOptionPane.showMessageDialog(null, "Cek Lagi!!!", "Error", JOptionPane.ERROR_MESSAGE);
            ex.printStackTrace();
        } catch (NumberFormatException ex) {
            JOptionPane.showMessageDialog(null, "Format ISBN tidak valid: " + ex.getMessage(), "Error", JOptionPane.ERROR_MESSAGE);
        } finally {

            try {
                if (pstmt != null) {
                    pstmt.close();
                }
                if (conn != null) {
                    conn.close();
                }
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }
    tampil();                                      
    }                                         

```

f. Mengisi code pada btnExit yang berfungsi untuk keluar dari halaman yang menampilkan GUI dengan kembali ke halaman awal. 
```
 private void btnExitActionPerformed(java.awt.event.ActionEvent evt) {                                        
        // TODO add your handling code here:
          System.exit(0);
    }        
```

g. Mengisi code pada program tblBukuMouseClicked yang berfungsi untuk menampilkan data dalam bentuk tabel selain itu, pengguna juga dapat mengedit data tersebut. 
```
 private void tblBukuMouseClicked(java.awt.event.MouseEvent evt) {                                     
        // TODO add your handling code here:
        int row = tblBuku.getSelectedRow();
        tfISBN.setText(tblBuku.getValueAt(row, 0).toString());
        tfJudulBuku.setText(tblBuku.getValueAt(row, 1).toString());
        tfTahunTerbit.setText(tblBuku.getValueAt(row, 2).toString());
        tfPenerbit.setText(tblBuku.getValueAt(row, 3).toString());
    }          
```

h. Mengisi code pada program bersih() yang berfungsi untuk memastikan bahwa setelah data dimasukkan atau operasi selesai, semua input dari pengguna direset untuk memudahkan pengguna memasukkan data baru tanpa harus menghapusnya secara manual. 
```
private void bersih() {
        tfISBN.setText("");
        tfJudulBuku.setText("");
        tfTahunTerbit.setText("");
        tfPenerbit.setText("");
    }    
```

8. Hasil eksekusi Insert data

![GetImage (13)](https://github.com/user-attachments/assets/aba143ce-d14c-49d6-b1b2-7d15547482c4)

9. Hasil eksekusi Update data

![GetImage (14)](https://github.com/user-attachments/assets/4b6a7afe-b076-4f37-b318-61fff5dfb571)

10. Hasil eksekusi Delete data

![GetImage (15)](https://github.com/user-attachments/assets/568b1f4e-8786-4d8f-8116-6ba588ecc760)

11. Hasil eksekusi Exit

![GetImage (16)](https://github.com/user-attachments/assets/ef65f596-fa1e-41a9-adf9-fc8f889f71ec)

![GetImage (17)](https://github.com/user-attachments/assets/4665aff5-1a83-4a3d-a58c-c098fa6fa78b)


 

 
 

  
