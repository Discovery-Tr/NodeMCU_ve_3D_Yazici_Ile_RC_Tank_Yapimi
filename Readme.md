EVDE 3D YAZICI İLE TANK YAPMAK ( RC + NODEMCU )
22/06/2021 / 3d, Arduino, Kendin Yap, Software, Teknoloji / 0 Yorum


              Merhaba, bu yazımızda evde Rc tank yapımı ve programlamasına değineceğiz. Bütün parça devre şeması ve yazılımını sizlerle paylaşacağım.

 

              Rc yi radyo kontrolü olarak Türkçeye çevirebiliriz. RC (Radio Control ), bir cihazı radyo frekansı ile uzaktan kontrol etmek için kullanılır. TV’lerimizde kullandığımız uzaktan kumanda ile de kontrol edebiliriz fakat mesafe hem uzak olmaz. Hem de kumanda ve cihaz sürekli birbirini görmek zorunda kalır.  Özellikle hobi devrelerinde sıkça tercih kullanılırlar. RC tank, RC araç, RC tekne ve RC drone örnek olarak gösterile bilir. 

 

              Bu projemizde tankımız yaklaşık 25Km hız ve 2-3 Km (Açık alanda) kapsama alanı içerisinde kontrol edebiliriz.  Kamera bağlantısı yapılarak tankımızı görmesek te görüntü ile yönlendirmemiz mümkündür.

 

              Bir oyuncaktan çok bir keşif robotu yada üzerine bağlanacak değişik eklentiler sayesinde istenilen özellikler eklenebilir.

3D Çizim Aşamaları
	
	
Devre Şeması
Projede kullanılan 2 servo motor Turret kontrolü içindir. Biri namlu yukarı aşağı diğeri ise namlunun sağa sola dönüşü içindir.

Proje Yazılımı
/* Receiver 2 ve 4*/
#define Ch_Sag_Motor 14 //D5
#define Ch_Sol_Motor 12 //D6
int ch2=0;
int ch4=0;

//Sol Motor
#define Sol_LPWM  16 //lpwm D0
#define Sol_RPWM  5 //rpwm D1
#define Sol_PWM   4 //pwm enable D7

//Sag Motor
#define Sag_LPWM  0 //lpwm D3
#define Sag_RPWM  2 //rpwm D4
#define Sag_PWM   13 //pwm enable D2

void setup()
{
  pinMode(Sol_LPWM, OUTPUT);
  pinMode(Sol_RPWM, OUTPUT);
  pinMode(Sol_PWM, OUTPUT);

  pinMode(Sag_LPWM, OUTPUT);
  pinMode(Sag_RPWM, OUTPUT);
  pinMode(Sag_PWM, OUTPUT);
  
  pinMode(Ch_Sag_Motor,INPUT);
  pinMode(Ch_Sol_Motor,INPUT);
  Serial.begin(9600); //9600 Baund bir seri haberleşme başlatıyoruz
}

void loop()
{
  ch2= pulseIn(Ch_Sag_Motor,HIGH,50000);
  ch4= pulseIn(Ch_Sol_Motor,HIGH,50000);

  switch(ch4)
        {
            case 1858 ... 2100: 
            Serial.print("Sol Motor - İleri - Hızlı: ");
            Serial.println(ch4);
            analogWrite(Sol_PWM, 650);
            Sol_Motor_Ileri ();
            break;
            
            case 1730 ... 1857: 
            Serial.print("Sol Motor - İleri - Orta: ");
            Serial.println(ch4);  
            analogWrite(Sol_PWM, 500);
            Sol_Motor_Ileri ();

            break;
            
            case 1601 ... 1729: 
            Serial.print("Sol Motor - İleri - Yavas: ");
            Serial.println(ch4); 
            analogWrite(Sol_PWM, 400);
            Sol_Motor_Ileri ();

            break;
            
            case 1391 ... 1600: 
            Serial.print(""); 
            analogWrite(Sol_PWM, 0);
            Sol_Motor_Dur ();

            break;
            
            case 1266 ... 1390: 
            Serial.print("Sol Motor - Geri - Yavas: ");
            Serial.println(ch4); 
            analogWrite(Sol_PWM, 400); //0-255
            Sol_Motor_Geri();

            break;
            
            case 1143 ... 1265: 
            Serial.print("Sol Motor - Geri - Orta: ");
            Serial.println(ch4);  
            analogWrite(Sol_PWM, 500); //0-255
            Sol_Motor_Geri();

            break;
            
            case 900 ... 1142:  
            Serial.print("Sol Motor - Geri - Hızlı: ");
            Serial.println(ch4); 
            analogWrite(Sol_PWM, 650); //0-255
            Sol_Motor_Geri();

            break;
            
            default: printf("");break;
        }

switch(ch2)
        {
            case 1858 ... 2100: 
            Serial.print("\t\t\t\t\t"); 
            Serial.print("Sag Motor - İleri - Hızlı: ");
            Serial.println(ch2);  
            analogWrite(Sag_PWM, 650);
            Sag_Motor_Ileri ();
            break;
            
            case 1730 ... 1857: 
            Serial.print("\t\t\t\t\t"); 
            Serial.print("Sag Motor - İleri - Orta: ");
            Serial.println(ch2);  
            analogWrite(Sag_PWM, 500);
            Sag_Motor_Ileri ();
            break;
            
            case 1601 ... 1729: 
            Serial.print("\t\t\t\t\t"); 
            Serial.print("Sag Motor - İleri - Yavas: ");
            Serial.println(ch2); 
            analogWrite(Sag_PWM, 400);
            Sag_Motor_Ileri ();
            break;
            
            case 1391 ... 1600: 
            Serial.print(""); 
            analogWrite(Sag_PWM, 0);
            Sag_Motor_Dur ();
            break;
            
            case 1266 ... 1390: 
            Serial.print("\t\t\t\t\t"); 
            Serial.print("Sag Motor - Geri - Yavas: ");
            Serial.println(ch2);
            analogWrite(Sag_PWM, 400); //0-255
            Sag_Motor_Geri(); 
            break;
            
            case 1143 ... 1265: 
            Serial.print("\t\t\t\t\t"); 
            Serial.print("Sag Motor - Geri - Orta: ");
            Serial.println(ch2);
            analogWrite(Sag_PWM, 500); //0-255
            Sag_Motor_Geri(); 
            break;
            
            case 900 ... 1142:  
            Serial.print("\t\t\t\t\t"); 
            Serial.print("Sag Motor - Geri - Hızlı: ");
            Serial.println(ch2);  
            analogWrite(Sag_PWM, 650); //0-255
            Sag_Motor_Geri();
            break;
            
            default: printf("");break;
        }  
}

void  Sol_Motor_Ileri ()
{
digitalWrite(Sol_LPWM, HIGH);
digitalWrite(Sol_RPWM, LOW);
}

void Sol_Motor_Dur ()
{
  digitalWrite(Sol_LPWM, LOW);
  digitalWrite(Sol_RPWM, LOW);
}
void Sol_Motor_Geri()
{
digitalWrite(Sol_LPWM, LOW);
digitalWrite(Sol_RPWM, HIGH);
}

///
void  Sag_Motor_Ileri ()
{
digitalWrite(Sag_LPWM, HIGH);
digitalWrite(Sag_RPWM, LOW);
}

void Sag_Motor_Dur ()
{
  digitalWrite(Sag_LPWM, LOW);
  digitalWrite(Sag_RPWM, LOW);
}
void Sag_Motor_Geri()
{
digitalWrite(Sag_LPWM, LOW);
digitalWrite(Sag_RPWM, HIGH);
}
 

MALZEMELER
Kumanda:
Flysky i6 X1



 

 

 

 

 

 

 

Kumanda Ayarlaması için tıklayınız.

Reciever:
FlySky FS-iA6 Receiver 6CH 2.4G



Motorlar:
55T Motor x2



Servo Motor x2

Motor Sürücü:
20 Amper Motor Sürücü Kartı BTS7960B  x2



Rulman:
8x22x7mm 608 12x

4x13x5mm 624 12x

Vidalar:
M4x15mm 8x

M4x60mm 12x

M3x20mm 4x

M3x10mm 4x

M3x45mm 84x

Somun:
M4 20x

M3 88x