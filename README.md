# 🗓️ Pertemuan Kedelapan - UTS NO 6 
## 📚 Topik Utama 
Langkah-langkah membuat CRUD dengan Java Swing untuk entitas entitas MataKuliah dengan atribut: KodeMK, SKS, NamaMK, SemesterAjar

## 📑 Daftar Isi 

- [Java Swing Mata Kuliah](https://github.com/fauziaeka/PBO_UTS/blob/main/FrameMataKuliah.java)
- [MataKuliah DB](https://github.com/fauziaeka/PBO_UTS/blob/main/MataKuliahDB.java) 
- kesimpulan 

## 🖥️ Langkah- Langkah 
1. Buat project baru dengan nama "PBO_UTS
   
   ![Gambar WhatsApp 2024-10-08 pukul 12 58 34_e2b5203d](https://github.com/user-attachments/assets/ca3c2328-82e4-4aaf-869e-f0cd4335c036)
2. Membuat class JFrame Form pada project "PBO_UTS"
   
   ![Gambar WhatsApp 2024-10-08 pukul 12 56 15_1bb013f5](https://github.com/user-attachments/assets/4dcd939a-5f3d-4fc4-8775-964eb14e6510)

3. Membuat Desain pada JFrame Form. Gunakan JLable pada judul tampilan “DATA MATA KULIAH”, KodeMK, SKS, NamaMK, SemesterAjar. Gunakan JTextField untuk mengisi atau menginput data KodeMK, SKS, NamaMK, SemesterAjar. Gunakan JButton pada tombol Insert, Update, Delete, Clear, dan Exit. Gunakan JTable untuk menampilkan Table Data Mata Kuliah yang berisi 4 kolom yakni KodeMK, SKS, NamaMK, SemesterAjar.

   ![image](https://github.com/user-attachments/assets/73bdf280-3541-4bbd-a38a-663beb7736ae)

4. Mengisi class MataKuliahDB. Class ini berfungsi untuk mengonversi ResultSet pada database menjadi TableModel agar dapat ditampilkan pada JTable di GUI Java Swing sehingga saat pengisian data kita tidak perlu menulis kodenya secara manual

```
package pbo_uts;

import java.sql.ResultSet;
import java.sql.ResultSetMetaData;
import java.util.Vector;
import javax.swing.table.DefaultTableModel;
import javax.swing.table.TableModel;

public class MataKuliahDB {

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

  ![Gambar WhatsApp 2024-10-08 pukul 12 49 53_1b37588d](https://github.com/user-attachments/assets/a47bbb55-f81d-4a80-9e4b-8043ab9070f2)
 
6. Menyambungkan Netbeans dengan Database 

- Ketuk ‘Services’ , pilih ‘Databases’ kemudian pilih ‘New Connection’ 

![GetImage (5)](https://github.com/user-attachments/assets/18121277-e900-45b3-bf50-b370c3213327)

- Pilih PostgreSQL

![GetImage (6)](https://github.com/user-attachments/assets/ff4859ae-bdf8-4a00-b517-d2aac9f9587b)

- Isi kolom Database sesuai dengan nama Database pada PostgreeSQL, kemudian isi password sesuai dengan password PostgreeSQL

![image](https://github.com/user-attachments/assets/ddff5208-e5fa-4f65-98df-311b54e9192c)

- Select schema menjadi ‘public’

![GetImage (8)](https://github.com/user-attachments/assets/bec9db0d-e7e0-4f2f-bd83-cbd9fdd1f41c)

- Pada kolom input connection ini akan otomatis terisi, klik Finish

![Gambar WhatsApp 2024-10-08 pukul 12 52 59_364b2135](https://github.com/user-attachments/assets/2d2d4d13-1124-487e-8039-f594ab826af3)
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
package pbo_uts;
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
import pbo_uts.MataKuliahDB;
import java.lang.System.Logger.*;
/**
 *
 * @author win 10
 */
public class FrameMataKuliah extends javax.swing.JFrame {
    Connection conn;
    PreparedStatement pstmt;
    ResultSet rs;
    String driver = "org.postgresql.Driver";
    String koneksi = "jdbc:postgresql://localhost:5432/PBO_UTS_Akademik";
    String user = "postgres";
    String password = "123456";
    private BufferedReader input = new BufferedReader(new InputStreamReader(System.in));

    /**
     * Creates new form FrameMataKuliah
     */
    public FrameMataKuliah() {
        initComponents();
    }


```

b. Membuat method tampil untuk membuat koneksi ke database dengan menggunakan perintah DriverManager.getConnection dan menampilkan informasi tersebut dalam sebuah JTable dengan menggunakan perintah "SELECT * FROM MataKuliah;" 
```
  public void tampil() {
        // TODO code application logi
        try (Connection conn = DriverManager.getConnection(koneksi, user, password); Statement stmt = conn.createStatement(); ResultSet rs = stmt.executeQuery("SELECT * FROM MataKuliah ORDER BY KodeMK")) {

            DefaultTableModel model = (DefaultTableModel) tblMataKuliah.getModel();

            model.setRowCount(0);

            while (rs.next()) {
                Object[] rowData = {rs.getString(1), rs.getString(2), rs.getString(3), rs.getString(4)};
                model.addRow(rowData);
            }
        } catch (SQLException ex) {
            System.err.println("Terjadi kesalahan: " + ex.getMessage());
        }
    }

```

c. Mengisi code pada btnInsert yang berfungsi untuk menambahkan data. Button ini berfungsi untuk menambahkan data pada 'Data Mata Kuliah'. Sebagai pengguna, kita diminta untuk mengisi informasi mengenai KodeMK, SKS, NamaMK, SemesterAjar. Setelah dimasukkan, kita bisa menekan button Insert. Jika berhasil, akan muncul pesan 'Data Berhasil disimpan'. 
```
 private void btnInsertActionPerformed(java.awt.event.ActionEvent evt) {//GEN-FIRST:event_btnInsertActionPerformed
        // TODO add your handling code here:
        String kodeMK, sks, namaMK, semesterAjar;
        try {
            Class.forName(driver);
            conn = DriverManager.getConnection(koneksi, user, password);
            conn.setAutoCommit(false);
            String sql = "INSERT INTO MataKuliah(kodeMK, sks, namaMK, semesterAjar) VALUES(?,?,?,?)";
            pstmt = conn.prepareStatement(sql);

            kodeMK = tfKodeMK.getText();
            sks = tfSKS.getText();
            namaMK = tfNamaMK.getText();
            semesterAjar = tfSemesterAjar.getText();
            
            pstmt.setLong(1, Long.parseLong(kodeMK));
            pstmt.setLong(2, Long.parseLong(sks));
            pstmt.setString(3, (namaMK));
            pstmt.setLong(4, Long.parseLong(semesterAjar));
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

d. Mengisi code pada btnUpdate yang berfungsi untuk memperbarui data. Pertama-tama masukkan KodeMK yang ingin diubah, kemudian masukkan informasi mengenai SKS, NamaMK, dan SemesterAjar baru. Jika berhasil, sistem akan mencetak pemberitahuan bahwa 'Data berhasil diupdate'. 
```
 private void btnUpdateActionPerformed(java.awt.event.ActionEvent evt) {                                          
        // TODO add your handling code here:
         String kodeMK, sks, namaMK, semesterAjar;

    if (tfKodeMK.getText().isEmpty()) {
        JOptionPane.showMessageDialog(null, "Kode Mata Kuliah harus diisi");
        return;
    }
    if (tfSKS.getText().isEmpty()) {
        JOptionPane.showMessageDialog(null, "SKS Mata Kuliah harus diisi");
        return;
    }
    if (tfNamaMK.getText().isEmpty()) {
        JOptionPane.showMessageDialog(null, "Nama Mata Kuliah harus diisi");
        return;
    }
    if (tfSemesterAjar.getText().isEmpty()) {
        JOptionPane.showMessageDialog(null, "Semester Ajar harus diisi");
        return;
    }

   try {
    Class.forName(driver);
    String sql = "UPDATE MataKuliah SET sks = ?, namaMK = ?, semesterAjar = ? WHERE KodeMK = ?";
    conn = DriverManager.getConnection(koneksi, user, password);
    pstmt = conn.prepareStatement(sql);

    kodeMK = tfKodeMK.getText();
    sks = tfSKS.getText();
    namaMK = tfNamaMK.getText();
    semesterAjar = tfSemesterAjar.getText();
    
    pstmt.setInt(1, Integer.parseInt(sks));
    pstmt.setString(2, namaMK);
    pstmt.setInt(3, Integer.parseInt(semesterAjar));
    pstmt.setString(4, kodeMK);
   
            int rowsAffected = pstmt.executeUpdate();
            if (rowsAffected > 0) {
                JOptionPane.showMessageDialog(null, "Data berhasil diupdate");
                bersih();
            } else {
                JOptionPane.showMessageDialog(null, "Data tidak ditemukan");
            }
        } catch (ClassNotFoundException | SQLException ex) {
            Logger.getLogger(FrameMataKuliah.class.getName()).log(Level.SEVERE, null, ex);
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

e. Mengisi code pada btnDelete yang berfungsi untuk menghapus data pada 'Data Mata Kuliah'. Pengguna bisa memasukkan data yang ingin dihapus atau bisa juga dengan mengeklik data yang ingin dihapus pada kolom list. Kemudian akan muncul pesan konfirmasi apakah pengguna ingin menghapus data atau tidak. Jika 'iya' data otomatis akan dihapus dan jika 'tidak' maka data akan batal dihapus. 
```
private void btnDeleteActionPerformed(java.awt.event.ActionEvent evt) {                                          
        // TODO add your handling code here:

        String kodeMK = tfKodeMK.getText();

        if (kodeMK.isEmpty()) {
            JOptionPane.showMessageDialog(null, "KodeMK tidak boleh kosong!", "Error", JOptionPane.ERROR_MESSAGE);
            return;
        }

        try {
            Class.forName(driver);
            conn = DriverManager.getConnection(koneksi, user, password);

            int jawab = JOptionPane.showConfirmDialog(null, "Apakah Anda yakin ingin menghapus MataKuliah dengan KodeMK: " + kodeMK + "?", "Konfirmasi Penghapusan", JOptionPane.YES_NO_OPTION);

            if (jawab == JOptionPane.YES_OPTION) {
                String deleteSql = "DELETE FROM MataKuliah WHERE KodeMK = ?";
                pstmt = conn.prepareStatement(deleteSql);

                // Asumsikan KodeMK adalah String, gunakan setString
                pstmt.setString(1, kodeMK);

                int rowsAffected = pstmt.executeUpdate();

                if (rowsAffected > 0) {
                    JOptionPane.showMessageDialog(null, "Data berhasil dihapus");
                } else {
                    JOptionPane.showMessageDialog(null, "Data tidak ditemukan untuk KodeMK: " + kodeMK);
                }
            } else {
                JOptionPane.showMessageDialog(this, "Penghapusan dibatalkan");
            }
        } catch (ClassNotFoundException ex) {
            JOptionPane.showMessageDialog(null, "Driver tidak ditemukan!", "Error", JOptionPane.ERROR_MESSAGE);
            ex.printStackTrace();
        } catch (SQLException ex) {
            JOptionPane.showMessageDialog(null, "Cek Lagi!!!", "Error", JOptionPane.ERROR_MESSAGE);
            ex.printStackTrace();
        } catch (NumberFormatException ex) {
            JOptionPane.showMessageDialog(null, "Format KodeMK tidak valid: " + ex.getMessage(), "Error", JOptionPane.ERROR_MESSAGE);
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

g. Mengisi code pada program tblMataKuliahMouseClicked yang berfungsi untuk menampilkan data dalam bentuk tabel selain itu, pengguna juga dapat mengedit data tersebut. 
```
  private void tblMataKuliahMouseClicked(java.awt.event.MouseEvent evt) {//GEN-FIRST:event_tblMataKuliahMouseClicked
        // TODO add your handling code here:
        int row = tblMataKuliah.getSelectedRow();
        tfKodeMK.setText(tblMataKuliah.getValueAt(row, 0).toString());
        tfSKS.setText(tblMataKuliah.getValueAt(row, 1).toString());
        tfNamaMK.setText(tblMataKuliah.getValueAt(row, 2).toString());
        tfSemesterAjar.setText(tblMataKuliah.getValueAt(row, 3).toString());
    }//GEN-LAST:event_tblMataKuliahMouseClicked
```

h. Mengisi code pada program bersih() yang berfungsi untuk memastikan bahwa setelah data dimasukkan atau operasi selesai, semua input dari pengguna direset untuk memudahkan pengguna memasukkan data baru tanpa harus menghapusnya secara manual. 
```
private void bersih() {
        tfKodeMK.setText("");
        tfSKS.setText("");
        tfNamaMK.setText("");
        tfSemesterAjar.setText("");
    }               
```

8. Hasil eksekusi Insert data

![image](https://github.com/user-attachments/assets/d93c23d6-a89d-46a1-b1c4-2cc92ac20c7f)

9. Hasil eksekusi Update data

![image](https://github.com/user-attachments/assets/7d36d315-5406-4ba2-9467-39e03cd4e591)

10. Hasil eksekusi Delete data

![image](https://github.com/user-attachments/assets/cffd243e-5b1d-491c-ac5f-b8da54da6f86)

11. Hasil eksekusi Exit

![image](https://github.com/user-attachments/assets/6a4078e2-2de0-4e34-9b51-f0506490b265)
![image](https://github.com/user-attachments/assets/9a7b1a85-9379-4988-9ba8-a3d836f7c304)


 

 
 

  
