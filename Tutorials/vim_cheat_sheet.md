# Vim Tutorials & Cheat Sheets
###### tags: `3. Miscellaneous` `tutorials` `vim`

## Introduction 
`vim` is a free and open-source text editor for UNIX system. This is a good text editor option to write and edit files on your terminal and cluster. 

You might encounter `vi` as a text editor as well. They are very similar and essentially the same for general use within the scope of our project. `Vim`, which stands for `Vi Improved`, is supposed to have many enhancements over `vi` (as the name suggests).

An alternative to `vim` is `nano`. I (RP) rarely use `nano`. For more information, go [here.](https://linuxize.com/post/how-to-use-nano-text-editor/#:~:text=GNU%20nano%20is%20an%20easy,%2D8%20encoding%2C%20and%20more.) 

## Getting Started 
### Opening `vim`
1. Assuming you are already on your terminal, to open vim, type
```
vim filename.txt
```
If there is no file that has the identical name (including file format) with the one from your command in your *current directory*, this command will not only open vim, **but also simultaneously makes a new file with that name**. You can name the file whatever name you want, as well as the file format. 

2. Your terminal should look like the image below. If you try to type something now, nothing will show up. Because you need to access its insert mode first. 

![Opened Vim](https://i.imgur.com/DGDpw9a.png) 

To enter the insert mode, press the `i` key. Now the terminal should look slightly different, as the prompt `-- INSERT --` shows up at the bottom left (see inside the red box). Now you can start editing the file. 

![Insert Mode](https://imgur.com/RMp9uRe.png)

3. To exit an insert mode, press the `esc` key. 



![Cheat Sheet](https://res.cloudinary.com/acloud-guru/image/fetch/c_thumb,f_auto,q_auto,w_1920/https://acg-wordpress-content-production.s3.us-west-2.amazonaws.com/app/uploads/2020/06/vim-2.png)