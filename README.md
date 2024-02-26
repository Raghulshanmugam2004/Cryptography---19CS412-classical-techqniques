# Cryptography---19CS412-classical-techqniques


# Caeser Cipher
Caeser Cipher using with different key values

# AIM:

To develop a simple C program to implement Caeser Cipher.

## DESIGN STEPS:

### Step 1:

Design of Caeser Cipher algorithnm 

### Step 2:

Implementation using C or pyhton code

### Step 3:

Testing algorithm with different key values. 

## PROGRAM:
```
#include <stdio.h>
#include <string.h>

int main() {
    // Declaring variables
    int i, key;
    char s[1000], c;

    // Taking Input
    printf("Enter a plaintext to encrypt:\n");
    fgets(s, sizeof(s), stdin);
    printf("Enter key:\n");
    scanf("%d", &key);

    int n = strlen(s);

    // Encrypting each character according to the given key
    for (i = 0; i < n; i++) {
        c = s[i];
        if (c >= 'a' && c <= 'z') {
            c = c + key;
            if (c > 'z') {
                c = c - 'z' + 'a' - 1;
            }
            s[i] = c;
        } else if (c >= 'A' && c <= 'Z') {
            c = c + key;
            if (c > 'Z') {
                c = c - 'Z' + 'A' - 1;
            }
            s[i] = c;
        }
    }

    // Output the cipher
    printf("Encrypted message: %s\n", s);

    return 0;
}
```

## OUTPUT:
![image](https://github.com/Raghulshanmugam2004/Cryptography---19CS412-classical-techqniques/assets/119561118/6e84dc05-af77-44f4-b880-0f9e995262cc)


## RESULT:
The program is executed successfully

---------------------------------

# PlayFair Cipher
Playfair Cipher using with different key values

# AIM:

To develop a simple C program to implement PlayFair Cipher.

## DESIGN STEPS:

### Step 1:

Design of PlayFair Cipher algorithnm 

### Step 2:

Implementation using C or pyhton code

### Step 3:

Testing algorithm with different key values. 

## PROGRAM:
```

#include <stdio.h>
#include <stdlib.h>
#include <string.h> 

/* Function for Playfair Cipher encryption  */
void Playfair(char str[], char keystr[]) {
    char keyMat[5][5];
    // Key & plainText
    char ks = strlen(keystr);
    char ps = strlen(str);
    void toUpperCase(char encrypt[], int ps) {
        for (int i= 0; i < ps; i++) {
            if (encrypt[i] > 96 && encrypt[i] < 123)
                encrypt[i] -= 32;
        }
    }
    int removeSpaces(char* plain, int ps) {
        int i, count = 0;
        for (i = 0; i < ps; i++)
            if (plain[i] != ' ')
                plain[count++] = plain[i];
        plain[count] = '\0';
        return count;
    }
    /* this function will create a 5 by 5 matrix. */
    void createMatrix(char keystr[], int ks, char keyMat[5][5]) {
        int flag = 0, *dict;
        /* here we are creating a hashmap for alphabets */
        dict = (int*)calloc(26, sizeof(int));
        for (int i = 0; i < ks; i++) {
            if (keystr[i] != 'j')
                dict[keystr[i] - 97] = 2;
        }
        dict['j' - 97] = 1;
        int i = 0, j = 0;
        for (int k = 0; k < ks; k++) {
            if (dict[keystr[k] - 97] == 2) {
                dict[keystr[k] - 97] -= 1;
                keyMat[i][j] = keystr[k];
                j++;
                if (j == 5) {
                    i++;
                    j = 0;
                }
            }
        }
        for (int k = 0; k < 26; k++) {
            if (dict[k] == 0) {
                keyMat[i][j] = (char)(k + 97);
                j++;
                if (j == 5) {
                    i++;
                    j = 0;
                }
            }
        }
    }
    /*this function looks for a digraph's characters in the key matrix and returns their positions.*/
    void search(char keyMat[5][5], char a, char b, int arr[]) {
        if (a == 'j')
            a = 'i';
        else if (b == 'j')
            b = 'i';
        for(int i = 0; i < 5; i++) {
            for(int j = 0; j < 5; j++) {
                if (keyMat[i][j] == a) {
                    arr[0] = i;
                    arr[1] = j;
                }
                else if (keyMat[i][j] == b) {
                    arr[2] = i;
                    arr[3] = j;
                }
            }
        }
    }
    /* This function avoids duplication and levels out the length of plain text by making it even.*/
    int prep(char str[], int p) {
        int sub = p;
        for (int i = 0; i < sub; i += 2) {
            if(str[i]==str[i+1]){
                for(int j=sub; j>i+1; j--){
                   str[j]=str[j-1];
                }
                str[i+1]='x';
                sub+=1;
            }
        }
        str[sub]='\0';
        if (sub % 2 != 0) {
            str[sub++] = 'z';
            str[sub] = '\0';
        }
        return sub;
    }
    // Here, the encryption is done.
    void encrypt(char str[], char keyMat[5][5], int pos) {
        int a[4];
        for(int i=0; i<pos; i+=2){
            search(keyMat, str[i], str[i + 1], a);
            if (a[0] == a[2]) {
                str[i] = keyMat[a[0]][(a[1] + 1)%5];
                str[i + 1] = keyMat[a[0]][(a[3] + 1)%5];
            }
            else if (a[1] == a[3]) {
                str[i] = keyMat[(a[0] + 1)%5][a[1]];
                str[i + 1] = keyMat[(a[2] + 1)%5][a[1]];
            }
            else {
                str[i] = keyMat[a[0]][a[3]];
                str[i + 1] = keyMat[a[2]][a[1]];
            }
        }
    }
    ks = removeSpaces(keystr, ks);
    ps = removeSpaces(str, ps);
    ps = prep(str, ps);
    createMatrix(keystr, ks, keyMat);
    encrypt(str, keyMat, ps);
    toUpperCase(str, ps);
    /* str is the final encrypted string in uppercase */
    printf("Cipher text: %s\n", str);
}


int main() {
    char string[200], keyString[200];
    printf("Enter key: ");
    scanf("%[^\n]s", &keyString);
    printf("Enter plaintext: ");
    scanf("\n");
    scanf("%[^\n]s", &string);
    
    //Playfair Cipher Program in C functional call
    Playfair(string, keyString);
    return 0;
}
```
## OUTPUT:
![image](https://github.com/Raghulshanmugam2004/Cryptography---19CS412-classical-techqniques/assets/119561118/8fd8e37d-653e-4b89-93e0-ba9e45088ab9)


## RESULT:
The program is executed successfully


---------------------------

# Hill Cipher
Hill Cipher using with different key values

# AIM:

To develop a simple C program to implement Hill Cipher.

## DESIGN STEPS:

### Step 1:

Design of Hill Cipher algorithnm 

### Step 2:

Implementation using C or pyhton code

### Step 3:

Testing algorithm with different key values. 

## PROGRAM:
```
#include<stdio.h>
#include<string.h>
int main() {
    unsigned int a[3][3] = { { 6, 24, 1 }, { 13, 16, 10 }, { 20, 17, 15 } };
    unsigned int b[3][3] = { { 8, 5, 10 }, { 21, 8, 21 }, { 21, 12, 8 } };
    int i, j;
    unsigned int c[20], d[20];
    char msg[20];
    int determinant = 0, t = 0;
    ;
    printf("Enter plain text\n ");
    scanf("%s", msg);
    for (i = 0; i < 3; i++) {
        c[i] = msg[i] - 65;
        printf("%d ", c[i]);
    }
    for (i = 0; i < 3; i++) {
        t = 0;
        for (j = 0; j < 3; j++) {
            t = t + (a[i][j] * c[j]);
        }
        d[i] = t % 26;
    }
    printf("\nEncrypted Cipher Text :");
    for (i = 0; i < 3; i++)
        printf(" %c", d[i] + 65);
    for (i = 0; i < 3; i++) {
        t = 0;
        for (j = 0; j < 3; j++) {
            t = t + (b[i][j] * d[j]);
        }
        c[i] = t % 26;
    }
    printf("\nDecrypted Cipher Text :");
    for (i = 0; i < 3; i++)
        printf(" %c", c[i] + 65);
    return 0;
}




```

## OUTPUT:
```
Enter plain text: SAN
 18 0 13 
Encrypted Cipher Text : R A J
Decrypted Cipher Text : S A N
```

## RESULT:
The program is executed successfully

-------------------------------------------------

# Vigenere Cipher
Vigenere Cipher using with different key values

# AIM:

To develop a simple C program to implement Vigenere Cipher.

## DESIGN STEPS:

### Step 1:

Design of Vigenere Cipher algorithnm 

### Step 2:

Implementation using C or pyhton code

### Step 3:

Testing algorithm with different key values. 

## PROGRAM:
```
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <ctype.h>
 
void upper_case(char *src) {
    while (*src != '\0') {
        if (islower(*src))
            *src &= ~0x20;
        src++;
    }
}
 
char* encipher(const char *src, char *key, int is_encode) {
    int i, klen, slen;
    char *dest;
 
    dest = strdup(src);
    upper_case(dest);
    upper_case(key);
 
    /* strip out non-letters */
    for (i = 0, slen = 0; dest[slen] != '\0'; slen++)
        if (isupper(dest[slen]))
            dest[i++] = dest[slen];
 
    dest[slen = i] = '\0'; /* null pad it, make it safe to use */
 
    klen = strlen(key);
    for (i = 0; i < slen; i++) {
        if (!isupper(dest[i]))
            continue;
        dest[i] = 'A' + (is_encode ? dest[i] - 'A' + key[i % klen] - 'A'
                : dest[i] - key[i % klen] + 26) % 26;
    }
 
    return dest;
}
 
int main() {
    const char *str = "Beware the Jabberwock, my son! The jaws that bite, "
        "the claws that catch!";
    const char *cod, *dec;
    char key[] = "VIGENERECIPHER";
 
    printf("Text: %s\n", str);
    printf("key:  %s\n", key);
 
    cod = encipher(str, key, 1);
    printf("Code: %s\n", cod);
    dec = encipher(cod, key, 0);
    printf("Back: %s\n", dec);
 
    /* free(dec); free(cod); *//* nah */
    return 0;
}
```


## OUTPUT:
```
Text: Beware the Jabberwock, my son! The jaws that bite, the claws that catch!
key:  VIGENERECIPHER
Code: WMCEEIKLGRPIFVMEUGXQPWQVIOIAVEYXUEKFKBTALVXTGAFXYEVKPAGY
Back: BEWARETHEJABBERWOCKMYSONTHEJAWSTHATBITETHECLAWSTHATCATCH
```

## RESULT:
The program is executed successfully

-----------------------------------------------------------------------

# Rail Fence Cipher
Rail Fence Cipher using with different key values

# AIM:

To develop a simple C program to implement Rail Fence Cipher.

## DESIGN STEPS:

### Step 1:

Design of Rail Fence Cipher algorithnm 

### Step 2:

Implementation using C or pyhton code

### Step 3:

Testing algorithm with different key values. 

## PROGRAM:
```
#include<stdio.h>
#include<string.h>
 
void encryptMsg(char msg[], int key){
    int msgLen = strlen(msg), i, j, k = -1, row = 0, col = 0;
    char railMatrix[key][msgLen];
 
    for(i = 0; i < key; ++i)
        for(j = 0; j < msgLen; ++j)
            railMatrix[i][j] = '\n';
 
    for(i = 0; i < msgLen; ++i){
        railMatrix[row][col++] = msg[i];
 
        if(row == 0 || row == key-1)
            k= k * (-1);
 
        row = row + k;
    }
 
    printf("\nEncrypted Message: ");
 
    for(i = 0; i < key; ++i)
        for(j = 0; j < msgLen; ++j)
            if(railMatrix[i][j] != '\n')
                printf("%c", railMatrix[i][j]);
}
 
void decryptMsg(char enMsg[], int key){
    int msgLen = strlen(enMsg), i, j, k = -1, row = 0, col = 0, m = 0;
    char railMatrix[key][msgLen];
 
    for(i = 0; i < key; ++i)
        for(j = 0; j < msgLen; ++j)
            railMatrix[i][j] = '\n';
 
    for(i = 0; i < msgLen; ++i){
        railMatrix[row][col++] = '*';
 
        if(row == 0 || row == key-1)
            k= k * (-1);
 
        row = row + k;
    }
 
    for(i = 0; i < key; ++i)
        for(j = 0; j < msgLen; ++j)
            if(railMatrix[i][j] == '*')
                railMatrix[i][j] = enMsg[m++];
 
    row = col = 0;
    k = -1;
 
    printf("\nDecrypted Message: ");
 
    for(i = 0; i < msgLen; ++i){
        printf("%c", railMatrix[row][col++]);
 
        if(row == 0 || row == key-1)
            k= k * (-1);
 
        row = row + k;
    }
}
 
int main(){
    char msg[] = "Hello World";
    char enMsg[] = "Horel ollWd";
    int key = 3;
 
    printf("Original Message: %s", msg);
 
    encryptMsg(msg, key);
    decryptMsg(enMsg, key);
 
    return 0;
}
```

## OUTPUT:
```
Original Message: Hello World
Encrypted Message: Horel ollWd
Decrypted Message: Hello World
```

## RESULT:
The program is executed successfully
