# REFLECTION

## ADVPRO_RUST_EDITION

### Modul_6

#### Reflection 1
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



