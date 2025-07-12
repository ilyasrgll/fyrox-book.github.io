# Dönüşüm

Dönüşüm (kısaca dönüşüm) - koordinat sistemini birinden diğerine değiştiren özel bir varlıktır.
Öncelikle sahne düğümlerinde konum/dönüş/ölçek/pivot/vb. bilgilerini depolamak için kullanılır. Fyrox oldukça karmaşık dönüşümler içerir ve
aşağıdakileri destekler:



1) Konum (`T`)

2) Dönüş (`R`)

3) Ölçek (`S`)

4) Ön dönüş (`Rpre`)

5) Son dönüş (`Rpost`)

6) Dönüş Pivotu (`Rp`)

7) Dönüş Ofseti (`Roff`)

8) Ölçekleme Ofseti (`Soff`)

9) Ölçekleme Pivotu (`Sp`)


Nihai dönüşüm matrisi `Transform = T * Roff * Rp * Rpre * R * Rpost * Rp⁻¹ * Soff * Sp * S * Sp⁻¹` olacaktır. %99,9
vakada ilk üçü hemen hemen her görev için yeterlidir. Diğer altı bileşen belirli işler için kullanılır (esas olarak FBX dosya formatından içe aktarılan düğümler

için).