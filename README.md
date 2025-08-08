# Magang---Soil-Moisture
esp8266


// --- DEKLARASI PIN ---
// Pin Analog untuk membaca data dari sensor
const int pinSensor = A0; 
// Pin Digital yang terhubung ke modul relay
const int pinRelay = D1;  // D1 adalah GPIO5 pada NodeMCU

// --- BATAS NILAI KERING (THRESHOLD) ---
// Nilai ini menentukan kapan tanah dianggap "kering".
// Nilai ini perlu dikalibrasi sesuai dengan sensor dan tanah Anda.
// Semakin tinggi nilainya, berarti semakin kering tanahnya.
// Nilai awal yang baik untuk memulai adalah sekitar 600-700.
int nilaiBatasKering = 650; 

void setup() {
  // Memulai komunikasi serial pada 9600 baud untuk debugging
  Serial.begin(9600);
  Serial.println("\nInisialisasi Sistem Penyiram Otomatis...");

  // Mengatur pin relay sebagai OUTPUT
  pinMode(pinRelay, OUTPUT);
  
  // Memastikan relay dalam kondisi mati (LOW) saat pertama kali dinyalakan
  digitalWrite(pinRelay, LOW); 
  Serial.println("Sistem Siap. Relay dalam kondisi OFF.");
}

void loop() {
  // Membaca nilai analog dari sensor (nilai berkisar antara 0 - 1023)
  int nilaiSensor = analogRead(pinSensor);

  // Menampilkan nilai sensor ke Serial Monitor untuk memudahkan kalibrasi
  Serial.print("Nilai Sensor Kelembapan: ");
  Serial.print(nilaiSensor);

  // Logika utama: membandingkan nilai sensor dengan batas kering
  if (nilaiSensor > nilaiBatasKering) {
    // KONDISI: Tanah kering
    Serial.println(" -> Tanah KERING! Menyalakan relay.");
    digitalWrite(pinRelay, HIGH); // Menyalakan relay (mengirim sinyal HIGH)
  } else {
    // KONDISI: Tanah basah/lembab
    Serial.println(" -> Tanah Lembab. Mematikan relay.");
    digitalWrite(pinRelay, LOW);  // Mematikan relay (mengirim sinyal LOW)
  }

  // Beri jeda 2 detik sebelum melakukan pembacaan berikutnya
  delay(2000); 
}
