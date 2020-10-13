### Задание

На имеющемся образе
/dev/mapper/VolGroup00-LogVol00 38G 738M 37G 2% /

1. Уменьшить том под / до 8G [ResizeRootDirectory.md](https://github.com/DenisDudyk/otus-lvm/blob/main/ResizeRootDirectory.md)
2. выделить том под /home [HomeToLv.md](https://github.com/DenisDudyk/otus-lvm/blob/main/HomeToLV.md)
3. выделить том под /var и сделать в mirror
4. /home - сделать том для снэпшотов
5. прописать монтирование в fstab, попробовать с разными опциями и разными файловыми системами ( на выбор)

6. Работа со снапшотами:
  * сгенерить файлы в /home/
  * снять снэпшот
  * удалить часть файлов
  * восстановится со снэпшота


7. * на нашей куче дисков попробовать поставить btrfs/zfs - с кешем, снэпшотами - разметить здесь каталог /opt
