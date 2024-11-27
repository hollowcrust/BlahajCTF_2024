# Corruptedimage

We are given a ```.jpg``` file that appears to be corrupted (literally the chall's name). When opening the file in hex editor (I used <a href="hexed.it">hexed.it</a>), the first 8 bytes are the magic numbers of a ```.jpg / .jpeg``` file. However, there are bytes used specifically for ```.png``` file chunks, namely ```IHDR, IDAT, IEND```. This suggests we need to change the first few bytes as the magic numbers of a ```.png``` file instead.

<br/><img width="677" alt="Screenshot 2024-11-27 at 22 12 10" src="https://github.com/user-attachments/assets/0850fb72-07ec-49f1-9cc8-189cd5e83d00"><br/>

To fix the file, we change the first 8 bytes into ```89 50 4E 47 0D 0A 1A 0A```, but that's not all. The next 4 bytes represent the length of the IHDR chunk, which through some <a href="">researching<a> would be 13 bytes in this case. Hence we change these 4 bytes into ```00 00 00 0D```

<br/><img width="674" alt="Screenshot 2024-11-27 at 22 15 16" src="https://github.com/user-attachments/assets/0a362884-f805-4895-a8e4-9184b2c1c732"><br/>

After that, we save the file as a ```.png``` file, open it and get the flag.

<br/>![Flag](https://github.com/user-attachments/assets/361c73a7-5cd0-4c77-b1e6-7e6213d7f4e1)<br/>

## Flag
  ```
  blahaj{F4K3_1M6}
  ```


