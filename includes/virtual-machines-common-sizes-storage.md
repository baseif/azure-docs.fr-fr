
La série Ls est optimisée pour les charges de travail qui requièrent un stockage local à faible latence, comme les bases de données NoSQL (par exemple, Cassandra, MongoDB, Cloudera et Redis). La série Ls offre jusqu’à 32 cœurs de processeur, grâce à la [Famille de processeurs Intel® Xeon® E5 v3](http://www.intel.com/content/www/us/en/processors/xeon/xeon-e5-solutions.html). Les performances du processeur sont les mêmes que pour la série G/GS et elles sont associées à 8 Gio de mémoire par cœur de processeur.  

## <a name="ls-series"></a>Série Ls

ACU : 180-240
 
| Taille          | Cœurs d’unité centrale | Mémoire : Gio | SSD local = Gio | Disques de données max. | Débit de disque maximal avec mise en cache : E/S par seconde/ Mbits/s (taille du cache en Gio) | Débit de disque maximal sans mise en cache : E/S / Mbits/s | Cartes réseau (max)/Bande passante réseau | 
|---------------|-----------|-------------|--------------------------|----------------|-------------------------------------------------------------|-------------------------------------------|------------------------------| 
| Standard_L4s  | 4    | 32   | 678   | 8              | NA / NA (0)          | 5 000 / 125                               | 2 / Élevée       | 
| Standard_L8s  | 8    | 64   | 1,388 | 16             | NA / NA (0)          | 10 000 / 250                              | 4 / Très élevée  | 
| Standard_L16s | 16   | 128  | 2,807 | 32             | NA / NA (0)          | 20 000 / 500                              | 8 / Extrêmement élevée | 
| Standard_L32s** | 32 | 256  | 5,630 | 64             | NA / NA (0)          | 40 000 / 1 000                            | 8 / Extrêmement élevée | 
 
Mbits/s = 10^6 octets par seconde, et Gio = 1024^3 octets. 

* Le débit de disque maximal possible (E/S par seconde ou Mbits/s) avec une machine virtuelle de la série Ls peut être limité par le nombre, la taille et la répartition des disques attachés. Pour plus d’informations, consultez l’article [Stockage Premium : stockage hautes performances pour les charges de travail des machines virtuelles Azure](../articles/storage/storage-premium-storage.md). 

**L’instance G5 est isolée sur un matériel dédié à un client unique.