using System;
using System.Collections.Generic; // Liste, Kuyruk, Sözlük gibi koleksiyonları için hazır kod kullandım.
namespace TRY_AGAIN
{
    
    class Karakter
    {
        
        private int can; // özel ve herkese açık olarak class sınıfları tanımladım
        private int guc;
        public string Ad;
        public int MaxCan;

        public int Can // private olduğu için bu değişkenlere sınıf dışından doğrudan erişim olmaz bu yüzden dışarıdan erişebilmek için public özellik tanımladım.
        {
            get { return can; }
            set { can = value; }
        }

        public int Guc
        {
            get { return guc; }
            set { guc = value; }
        }

        
        public static int ts = 0;  //tüm karakterlerin toplam saldırı(ts) sayısını saymak için kullandım.


        public Karakter(string ad, int can, int guc) // tanımlama yaptım.
        {
            this.Ad = ad;
            this.MaxCan = can;
            this.Can = can;
            this.Guc = guc;
        }

        
        public virtual int Saldir()
        {
            ts++;  // saldırı sayacını arttırır. 
            int hasar = (Guc - 3, Guc + 3);    // hasar miktarı -+3 olarak hesaplanır 
            return hasar;   // hasar değerini dondürür
           
        }

        public void HasarAl(int hasar) // hasar aldıkça can miktarını azaltır ve konsola yazdırmmayı hedefledim hocam
        {
            Can = Can - hasar;
            if (Can < 0)
                Can = 0;
            Console.WriteLine(Ad + " " + hasar + " hasar aldı!");
        }
    }

    
    class Oyuncu : Karakter  // mana ve max manayı tanımladık 
    {
        public int Mana;
        public int MaxMana;

        
        public Oyuncu(string ad) : base(ad, 100, 15) // maxmana ve mana 40 a tanımladık ve karakter oyuncu arasında kalıtım kullandım
        {
            Mana = 40;
            MaxMana = 40;
        }

        
        public override int Saldir()    // üst sınıftan olan karakter sınıfındaki saldır() kommutunu override yaparak çağırır ve karakter ismiyle konsola yazdırır 
        {
            Console.WriteLine(Ad + "Saldırı yapıyor")
        }


    
    class Dusman : Karakter // kalıtım kullanarak düşmanı tanımladık
    {
        public Dusman(string ad, int can, int guc) : base(ad, can, guc)
        {
        }

        public override int Saldir() // override kullanarak düşmanının karakterinin ismini çağırarak kullnarak konsola yazdırır 
        {
            Console.WriteLine(Ad + "Saldırıyo");
            return base.Saldir();
        }
    }

    
    class Zombi : Dusman // düşmanı kalıtım yaparak düşman tanımlaması ve düşmanın özelliklerini tanımladım
    {
        public Zombi() : base("Zombi", 50, 10)
        {
        }
    }
    class Goblin : Dusman
    {
        public Goblin() : base("Goblin", 35, 12)
        {
        }
    }
    class Ejderya : Dusman
    {
        public Ejderya() : base("Ejderya", 80, 18)
        {
        }
    }
    class Program
    {
        static void Main()
        {
           
            Console.Write("Adınızı girin: "); // oyun oluşturmak için adımlar tanımmladım
            string ad = Console.ReadLine(); // konsoldan giriş almak için kullanılır 

            Oyuncu oyuncu = new Oyuncu(ad);
            List<Dusman> dusmanlar = new List<Dusman>(); // düşman ve saldırı için liste oluşturdum
            List<string> saldirilar = new List<string>();
         

            Console.WriteLine("Oyun başlıyor!");

            while (oyuncu.Can > 0)
            {
                
                Dusman dusman = null;
                int dusmanTip = rnd.Next(1, 4); // düşman sayısı rastegele atanır ve düşman tiplemelerini tanımladım               
                if (dusmanTip == 1)
                    dusman = new Zombi();
                else if (dusmanTip == 2)
                    dusman = new Goblin();
                else
                    dusman = new Ejderya();

                dusmanlar.Add(dusman);

                Console.WriteLine("Yeni düşman: " + dusman.Ad);
                Console.WriteLine("Düşman Canı: " + dusman.Can);

                
                while (dusman.Can > 0 && oyuncu.Can > 0) // oyuncular hakkında konsolda bilgi vermeyi sağladım
                {
                    Console.WriteLine( oyuncu.Ad);
                    Console.WriteLine("Canı: " + oyuncu.Can + "/" + oyuncu.MaxCan);
                    Console.WriteLine("Mana: " + oyuncu.Mana + "/" + oyuncu.MaxMana);
                    Console.WriteLine("Düşman: " + dusman.Ad + " (Canı: " + dusman.Can + ")");
                    }
                    
                    if (dusman.Can > 0) // düşman hakkında bilgi vermeyi hedefledim 
                    {
                        Console.WriteLine(dusman.Ad + " saldırıyor!");
                        int dusmanHasar = dusman.Saldir();
                        oyuncu.HasarAl(dusmanHasar);
                        saldirilar.Add(dusman.Ad + " " + oyuncu.Ad + " ya " + dusmanHasar + " hasar verdi");
                    }

                    if (dusman.Can <= 0)
                    {
                        Console.WriteLine(dusman.Ad + " öldü!");
                    }

                    else 
                    {
                        Console.WriteLine("Yanlış tuşa bastınız..");

                    }



                        Console.WriteLine("Devam etmek için tuşa basınız...");
                    Console.ReadLine();
                    
                }

                if (oyuncu.Can <= 0)
                    break;
            }

            Console.WriteLine("!!OYUN BİTTİ!!"); // son detaylar haklında detay verir
            Console.WriteLine(oyuncu.Ad + " öldü!");
            Console.WriteLine("Toplam Saldırı: " + Karakter.ts);
            Console.WriteLine("Öldürülen Düşman: " + dusmanlar.Count);

            Console.WriteLine("Son saldırılar:");
            for (int i = 0; i < saldirilar.Count && i < 5; i++)
            {
                Console.WriteLine(saldirilar[i]);
            }

            Console.ReadLine();
        }
    }
}
