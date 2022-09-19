using System;
using System.Collections.Generic;
using System.Linq;
using System.Diagnostics;

namespace OyunKart
{
    static class oyun
    {
        private static Random rand = new Random();

        public class kart
        {
            public bool aktif;
            public string harf;
            public int number;
            public kart(bool aktif, string harf, int number)
            {
                this.aktif = aktif;
                this.harf = harf;
                this.number = number;
            }
        }

        public static void karistir<T>(this IList<T> list) //karıştırmak
        {
            int a = list.Count;
            while (a > 1)
            {
                a--;
                int j = rand.Next(a + 1);
                T value = list[j];
                list[j] = list[a];
                list[a] = value;
            }
        }

        public class Program
        {

            public void cizim(List<kart> kart)
            {
                for (int i = 1; i <= 16; i++)
                {
                    if (i < 17)
                    {
                        if (kart[i - 1].aktif == false)
                        {

                            if (i % 4 == 0)
                            {

                                Console.Write($" | {kart[i - 1].harf}  |");
                                Console.WriteLine("");
                            }
                            else
                            {
                                Console.Write($" | {kart[i - 1].harf} ");
                            }
                        }
                        else
                        {
                            if (i % 4 == 0)
                            {
                                Console.Write($" | {i}  |");
                                Console.WriteLine("");
                            }
                            else
                            {
                                Console.Write($" | {i} ");
                            }

                        }
                    }
                    else
                    {
                        if (kart[i - 1].aktif == false)
                        {

                            if (i % 4 == 0)
                            {

                                Console.Write($" | {kart[i - 1].harf}  |");
                                Console.WriteLine("");
                            }
                            else
                            {
                                Console.Write($" | {kart[i - 1].harf} ");
                                Console.WriteLine("");

                            }
                        }
                        else
                        {
                            if (i % 4 == 0)
                            {
                                Console.Write($" | {i} |");
                                Console.WriteLine("");
                            }
                            else
                            {
                                Console.Write($" | {i}");

                            }

                        }

                    }
                }

                Console.WriteLine("");
            }
            static void Main(string[] args)
            {
                Program prog = new Program();
                List<kart> kisi = new List<kart>();
                kisi.Add(new kart(true, "A", 1));
                kisi.Add(new kart(true, "A", 2));
                kisi.Add(new kart(true, "B", 3));
                kisi.Add(new kart(true, "B", 4));
                kisi.Add(new kart(true, "C", 5));
                kisi.Add(new kart(true, "C", 6));
                kisi.Add(new kart(true, "D", 7));
                kisi.Add(new kart(true, "D", 8));
                kisi.Add(new kart(true, "E", 9));
                kisi.Add(new kart(true, "E", 10));
                kisi.Add(new kart(true, "F", 11));
                kisi.Add(new kart(true, "F", 12));
                kisi.Add(new kart(true, "G", 13));
                kisi.Add(new kart(true, "G", 14));
                kisi.Add(new kart(true, "H", 15));
                kisi.Add(new kart(true, "H", 16));

                kisi.karistir();

                bool working = true;
                DateTime baslangic = DateTime.Now;
                int move_count = 0;
                Stopwatch basla = new Stopwatch();
                basla.Start();
                int acilan_karts = 0;
                prog.cizim(kisi);
                while (working)
                {
                    Console.WriteLine("Birinci kart için bir sayı giriniz: ");
                    int ilkHamle;
                    string input1 = Console.ReadLine();
                    while (!int.TryParse(input1, out ilkHamle) || (ilkHamle > 17 || ilkHamle < 0))
                    {
                        Console.WriteLine("17 'den küçük bir değer veya 0'dan büyük bir değer giriniz: ");
                        input1 = Console.ReadLine();
                    }

                    Console.WriteLine("İkinci kart için bir sayı giriniz: ");
                    int ikinciHamle;
                    string input2 = Console.ReadLine();
                    while (!int.TryParse(input2, out ikinciHamle) || (ikinciHamle > 17 || ikinciHamle < 0))
                    {
                        Console.WriteLine("17'den küçük bir değer veya 0'dan büyük bir değer giriniz: ");
                        input2 = Console.ReadLine();
                    }

                    if (kisi[ilkHamle - 1] == kisi[ikinciHamle - 1])
                    {
                        Console.WriteLine("Eşleşme başarılı!!");
                        acilan_karts++;
                        if (acilan_karts == 8)
                        {
                            Console.WriteLine("Oyun bitmiştir!!");
                            Console.WriteLine("Hamle sayınız :" + move_count);
                            TimeSpan bitis = DateTime.Now - baslangic;
                            Console.WriteLine("Geçen zaman: " + bitis);
                            break;
                        }
                    }

                    else if (kisi[ilkHamle - 1].harf.Equals(kisi[ikinciHamle - 1].harf))
                    {
                        kisi[ilkHamle - 1].aktif = false;
                        kisi[ikinciHamle - 1].aktif = false;
                        acilan_karts++;
                    }
                    else
                    {
                        Console.WriteLine("Hatalı eşleşme");
                        kisi[ilkHamle - 1].aktif = false;
                        kisi[ikinciHamle - 1].aktif = false;
                        prog.cizim(kisi);
                        kisi[ilkHamle - 1].aktif = true;
                        kisi[ikinciHamle - 1].aktif = true;
                    }
                    move_count++;
                    if (acilan_karts == 8)
                    {
                        basla.Stop();
                        TimeSpan time = basla.Elapsed;
                        Console.WriteLine($"Oyun sona erdi.");
                        Console.WriteLine($"Toplam adım sayınız: {move_count}");
                        Console.WriteLine($"Toplam süreniz: {time.Minutes}:{time.Seconds}");

                        working = false;
                        break;
                    }
                    prog.cizim(kisi);
                }

            }
        }
    }
}
