# Shark-lotto

Reading the source code of the website via Inspect suggests that we need to raise our ```Money``` from ```20``` to ```13371337``` to get the flag. We can only bet no more than what we currently have and for each spin, if we have 3 same images we win the bet amount, while we lose by that amount otherwise. Winning by betting seems inplausible, unless we can bet a negative number and lose (highly likely) and actually "earn" from losing. We can put, for example, ```-13371337``` in the ```Bet``` field then spin the wheels. Luckily, there is no negative number check and we can get the flag once we lost the spin.

## Flag
  ```
  blahaj{d0n7_7rus7_th3_cl13nt}
  ```
