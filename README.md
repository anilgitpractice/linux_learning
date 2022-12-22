# By Using OpenSSL tool Encrypt and Decrypt a Folder in Linux

## Overview

- In this tutorial, we’ll take a look at openssl tool we can use to encrypt and decrypt directories and their contents in Linux.

### Encryption

- Encryption is the process of encoding data with the intent of keeping it safe from unauthorized access.

- In this quick tutorial, we’ll learn how to `encrypt` and `decrypt` files in Linux systems.

- By using openssl which is popular and free software.

### Types of Encryption

- Depending on the number of data strings involved in the encryption and decryption process, we have two kinds of encryption.
      
	  1).Symmetric
	  
	  2).Asymmetric
	  
> **Note** In this tutorial we use Symmetric method for more information [clickhere](https://www.baeldung.com/linux/encrypt-decrypt-files).

### Symmetric process

- When only one data string a passphrase is used for both encryption and decryption, it’s called symmetric encryption.

- We generally use symmetric encryption when we don’t need to share the encrypted files with anyone else.

## Install OpenSSL

- You need to install OpenSSL package in your machine frist check if it is installed or not by executing the below commnd
```
openssl version
```

- By executing the above command it will print result if the OpenSSL is avilable in your machine it will prints the version of the OpenSSL package.

- If it is not print any result and giving result `command not found` error. So you can install OpenSSL package by using the `apt-get`command shown below.

```
sudo apt-get install openssl -y
```

- And agin run the command `openssl version` to view the OpenSSL is installed or not.

## Encrypting a folder/directory 

- First of all you can create a folder/directory named as `patch` by using the `mkdir` commnand.
```
mkdir patch
```
- After creation of the `patch` folder `cd` into that folder, And create a text files by using the `touch` command named as `file1.txt`,`file2.txt`and `file2.txt`. 
```
touch file1.txt file2.tet file3.txt
```

- onece the files created add some information into that files by using the `vi` or `nano` editors.


#### Zip a folder 

- After adding information into that files `zip` the folder `patch` by using the below command and name it as `patch1.zip` or your requirement name with `.zip` exetension.
```
$ zip -r <output_file> <folder_1> <folder_2> ... <folder_n>

$ zip -r patch1.zip patch 
```

- In this tutorial we zipped a `patch` folder with the name of `patch1.zip`.

- After zipping the file now execute the checksum command on `patch1.zip` it will gives string value store this for future purpose. Execute below command.
```
md5sum < option file > path of the file 

md5sum patch1.zip .
```
> `.` means current directory

- The string value should compare with the decrypted file string value. If it two strings matched it is successfully decrypted.

### Executing OpenSSL encryption command 

- After creating `.zip` folder now encrypt `patch1.zip` folder by executing the OpenSSL encrypt command shown below.

```
openssl aes-256-cbc -a -salt -pbkdf2 -in patch1.zip -out patch_enc.zip

```
> the above command `-in` means input source file and `-out` means final encryption file with required name. In this tutorial we used `-in` value as a `patch1.zip` and `-out` value is `patch_enc.zip`.
         

- When you run the above command it will ask a password enter your password and confirm your password for encrypting a folder. 

![image](https://user-images.githubusercontent.com/97168620/209156084-af92dae8-1f53-4f1e-9b0e-ee9fdadd096a.png)

- After setting password it will genarates `patch_enc.zip` folder in the same location.

- Now you can try to `unzip` encrypted `patch_enc.zip` folder it will gives an error shown below.
```
mystroom@stroom:~$ unzip patch_enc.zip
Archive:  patch_enc.zip
  End-of-central-directory signature not found.  Either this file is not
  a zipfile, or it constitutes one disk of a multi-part archive.  In the
  latter case the central directory and zipfile comment will be found on
  the last disk(s) of this archive.
unzip:  cannot find zipfile directory in one of patch_enc.zip or
        patch_enc.zip.zip, and cannot find patch_enc.zip.ZIP, period.
```

- Encryption is done on the `patch1.zip` folder.

## Decrypting an Encrypted folder/directory

- Let’s now try to decrypt the encrypted folder`patch_enc.zip`, By executing the below command.

```
openssl aes-256-cbc -d -a -pbkdf2 -in patch_enc.zip -out patch.zip
```
- When you run this command it will ask's `password`. That is when you enterd at encrypting a folder `patch1.zip` process.

![image](https://user-images.githubusercontent.com/97168620/209157289-0467074b-7836-44db-8c1f-dcb4d42a138e.png)

- This will create the decrypted folder `patch.zip` in the same location.

- Now agin  verify file authenticity and integrity by executing the command shown below.
```
md5sum < option file > path of the file 

md5sum patch.zip .
```
> Here `.` means current directory.

- Again it gives string value compare this string with existing string occured when you executed `md5sum` command on `patch1.zip` folder.

- Now try to `unzip` the decrypted folder `patch.zip` in the same location. It will overrides existing files.

- So create one folder with help of `mkdir` command and named it as a `sandbox` or your requirement.
 
- After creating the folder `cp`or `mv`by using this commands move decrypted file to `sandbox` folder.
```
mkdir sandbox

mv patch.zip sandbox/
```
- After moveing `patch.zip` `cd` to  `sandbox`, Now execute `unzip` command on decrypted folder`patch.zip`.
```
cd sandbox

unzip patch.zip
```
- It show's like this 
```
mystroom@stroom:~/sandbox$ unzip Patch.zip
Archive:  Patch.zip
   creating: patch/
 extracting: patch/f2
 extracting: patch/f3
 extracting: patch/f1
```
- After `unzip` execute a command `ls -l` or `ll` it will showes the unziped folder 
```
mystroom@stroom:~/example1/sandbox$ ls -l
total 24
drwxrwxr-x 2 mystroom mystroom 4096 Dec 22 10:09 patch/
mystroom@stroom:~/example1/sandbox$ cd patch
mystroom@stroom:~/example1/sandbox/patch$ ll
total 20
drwxrwxr-x 2 mystroom mystroom 4096 Dec 22 10:09 ./
drwxrwxr-x 3 mystroom mystroom 4096 Dec 22 13:38 ../
-rw-rw-r-- 1 mystroom mystroom   20 Dec 22 10:08 f1
-rw-rw-r-- 1 mystroom mystroom   20 Dec 22 10:08 f2
-rw-rw-r-- 1 mystroom mystroom   21 Dec 22 10:09 f3
```
- It will show all the files inside of a zip file after decryption and unzip operations.


### Conclusion

- In this article, we learned how to encrypt folders, depending on the requirement, we can decide upon the most suitable one for the task.

- We also learned about checksum by executing the `md5sum` command  verify file authenticity and integrity.

- successfully Encrypted and Decrypted a folder by using the OpenSSL command line tool :joy:

