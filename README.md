# REFLECTION

## ADVPRO_RUST_EDITION

### Modul_6

#### Commit 1 Reflection notes
```rust
fn handle_connection(mut stream: TcpStream) { 
    let buf_reader = BufReader::new(&mut stream);
    let http_request: Vec<_> = buf_reader 
        .lines() 
        .map(|result| result.unwrap())
        .take_while(|line| !line.is_empty()) 
        .collect();
    println!("Request: {:#?}", http_request);
}
```

Di atas adalah fungsi handle_connection yang disiapkan pada tutorial kali ini. <br>
Penjelasan Kode:<br>
1. Baris Pertama : 
    ```rust
    fn handle_connection(mut stream: TcpStream)
    ```
    Mendefinisikan fungsi handle_connection yang mengambil parameter 'stream'berupa koneksi TCP.<br>
2. Baris kedua :   
    ```rust
    let buf_reader = BufReader::new(&mut stream);<br>
    ```
    Membaca data dari koneksi TCP.<br>
3. Baris ketiga :
    ```rust
        let http_request: Vec<_> = buf_reader
        .lines()
        .map(|result| result.unwrap())
        .take_while(|line| !line.is_empty())
        .collect();
    ```
    .lines() bertugas mengembalikan iterator dari baris-baris yang dibaca oleh buf_reader. .map memetakan semua elemen dari iterartor ke nilai sebenarnya..take_while mengambil elemen dari iterator sampai ketemu baris kosong. .collect mengumpulkan elemen-elemen yang didapatkan dari perintah-perintah sebelumnya.
4. Baris keempat : 
    ```rust
    println!("Request: {:#?}", http_request);
    ```
    Mencetak http_requet ke konsol.<br>
    
Kesimpulan: <br>
Fungsi handle_connection membaca request HTTP dari koneksi TCP yang diberikan dan mencetak hasil-hasilnya ke konsol.

#### Commit 2 Reflection notes

Fungsi handle_connection yang dikerjakan pada commit 2 kali ini adalah implementasi sederhana dari fungsi untuk menangani koneksi TCP dan mengirimkan respons HTTP.

    ```rust
    fn handle_connection(mut stream: TcpStream) {
        let buf_reader = BufReader::new(&mut stream); 
        let http_request: Vec<_> = buf_reader
            .lines() 
            .map(|result| result.unwrap()) 
            .take_while(|line| !line.is_empty())
            .collect();

        let status_line = "HTTP/1.1 200 OK"; let contents = fs::read_to_string("hello.html").unwrap(); let length = contents.len();
        let response = format!("{status_line}\r\nContent-Length: {length}\r\n\r\n{contents}");

        stream.write_all(response.as_bytes()).unwrap();
    }
    ```
Fungsi tersebut mengambil parameter stream yang mewakili koneksi TCP yang aktif. Buf_reader digunakan untukmembaca data dari stream.Data tersebut dibaca menggunakan lines() dan dimap dengan mengabaikan kemungkinan kesalahan.take_while digunakan untuk mengambilsetiap baris sampai menemukan baris kosong.<br>

Setelahpermintaan HTTP dibaca, kode melanjutkan untuk menyiapkan respons HTTP.status line diberikan sebagai "HTTP/1.1 200 OK". Kemudian, isi file hello.html dibaca dengan telah diasumsikan bahwa file hello.html memang bisa dibaca oleh program. Panjang konten dibaca dengan variabel length.<br>

Respon HTTP disusun menggunakan format status line diikuti oleh Content-Length, dan diikuti lagi oleh dua baris kosong untuk menandai akhir header dan diikuti oleh isi dari hello.html. Terakhir,respon yang telah disusun dikirimkan melalui stream menggunakan write_all.

Screenshot halaman hello.hmtl dan cmd rust
![Commit 2 screen capture](/assets/Commit2.png)

#### Commit 3 Reflection notes
```rust
fn handle_connection(mut stream: TcpStream) {
    let buf_reader = BufReader::new(&mut stream);
    let request_line = buf_reader.lines().next().unwrap().unwrap();

    if request_line == "GET / HTTP/1.1" {
        let status_line = "HTTP/1.1 200 OK";
        let contents = fs::read_to_string("hello.html").unwrap();
        let length = contents.len();

        let response = format!(
            "{status_line}\r\nContent-Length: {length}\r\n\r\n{contents}"
        );

        stream.write_all(response.as_bytes()).unwrap();
    } else {
        
        let status_line = "HTTP/1.1 404 NOT FOUND";
        let contents = fs::read_to_string("404.html").unwrap();
        let length = contents.len();

        let response = format!(
            "{status_line}\r\nContent-Length: {length}\r\n\r\n{contents}"
        );

        stream.write_all(response.as_bytes()).unwrap();
    }
}
```
Kode handle connection di atas memisahkan respon mulai di baris:
```rust
if request_line == "GET / HTTP/1.1"
```
Dengan kode tersebut, jika permintaan yang diterima adalah GET/HTTP/1.1, maka akan dikirimkan respon 200(OK) bersama dengan isi dari file hello.html. Jika bukan, makan akan dikirimkan respon 404(NOT FOUND) bersama dengan isi dari file 404.html.

Refactoring kode di atas diperlukan agar kode di if else benar-benar dibedakan dengan dua kasus yang berbeda juga.

Screenshot halaman hello.html dan cmd
![Commit 3 screen capture](/assets/Commit3.png)



