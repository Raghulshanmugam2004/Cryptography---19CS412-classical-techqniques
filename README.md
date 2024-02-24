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
#include<stdio.h>

#include<ctype.h>

int main() {

    char text[500], ch;

    int key;

    // Taking user input.
    printf("Enter a message to encrypt: ");

    scanf("%s", text);

    printf("Enter the key: ");

    scanf("%d", & key);

    // Visiting character by character.

    for (int i = 0; text[i] != '\0'; ++i) {

        ch = text[i];
        // Check for valid characters.
        if (isalnum(ch)) {

            //Lowercase characters.
            if (islower(ch)) {
                ch = (ch - 'a' + key) % 26 + 'a';
            }
            // Uppercase characters.
            if (isupper(ch)) {
                ch = (ch - 'A' + key) % 26 + 'A';
            }

            // Numbers.
            if (isdigit(ch)) {
                ch = (ch - '0' + key) % 10 + '0';
            }
        }
        // Invalid character.
        else {
            printf("Invalid Message");
        }

        // Adding encoded answer.
        text[i] = ch;

    }

    printf("Encrypted message: %s", text);

    return 0;
}
```

## OUTPUT:
```
Enter a message to encrypt: yZq8NS92mdR
Enter the key: 6
Encrypted message: eFw4TY58sjX
```

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
// Implementation of Playfair Cipher in C program

#include <stdio.h>   //header file
#include <stdlib.h>  //header file
#include <string.h>  //header file

#define SIZE 30

// this function will convert the string to lowercase

void toLowerCase(char plain[], int ps)
{
    int i;
    for (i = 0; i < ps; i++) {
        if (plain[i] > 64 && plain[i] < 91)
            plain[i] += 32;
    }
}

// this function will remove all the spaces
int removeSpaces(char* plain, int ps)
{
    int i, count = 0;
    for (i = 0; i < ps; i++)
        if (plain[i] != ' ')
            plain[count++] = plain[i];
    plain[count] = '\0';
    return count;
}

// this function will generate the 5x5 grid square
void generateKeyTable(char key[], int ks, char keyT[5][5])
{
    int i, j, k, flag = 0, *dicty;

    // character hashmap of 26 character that will
    // store count of the alphabet.
    dicty = (int*)calloc(26, sizeof(int));
    for (i = 0; i < ks; i++) {
        if (key[i] != 'j')
            dicty[key[i] - 97] = 2;
    }

    dicty['j' - 97] = 1;

    i = 0;
    j = 0;

    for (k = 0; k < ks; k++) {
        if (dicty[key[k] - 97] == 2) {
            dicty[key[k] - 97] -= 1;
            keyT[i][j] = key[k];
            j++;
            if (j == 5) {
                i++;
                j = 0;
            }
        }
    }

    for (k = 0; k < 26; k++) {
        if (dicty[k] == 0) {
            keyT[i][j] = (char)(k + 97);
            j++;
            if (j == 5) {
                i++;
                j = 0;
            }
        }
    }
}

// this function will search for the characters of a digraph
// in the key and return position of key
void search(char keyT[5][5], char a, char b, int arr[])
{
    int i, j;

    if (a == 'j')
        a = 'i';
    else if (b == 'j')
        b = 'i';

    for (i = 0; i < 5; i++) {

        for (j = 0; j < 5; j++) {

            if (keyT[i][j] == a) {
                arr[0] = i;
                arr[1] = j;
            }
            else if (keyT[i][j] == b) {
                arr[2] = i;
                arr[3] = j;
            }
        }
    }
}

// this function will find the modulus with 5
int mod5(int a) { return (a % 5); }

// this function will make the plain text length even
int prepare(char str[], int ptrs)
{
    if (ptrs % 2 != 0) {
        str[ptrs++] = 'z';
        str[ptrs] = '\0';
    }
    return ptrs;
}

// encryption will done using this function
void encrypt(char str[], char keyT[5][5], int ps)
{
    int i, a[4];

    for (i = 0; i < ps; i += 2) {

        search(keyT, str[i], str[i + 1], a);

        if (a[0] == a[2]) {
            str[i] = keyT[a[0]][mod5(a[1] + 1)];
            str[i + 1] = keyT[a[0]][mod5(a[3] + 1)];
        }
        else if (a[1] == a[3]) {
            str[i] = keyT[mod5(a[0] + 1)][a[1]];
            str[i + 1] = keyT[mod5(a[2] + 1)][a[1]];
        }
        else {
            str[i] = keyT[a[0]][a[3]];
            str[i + 1] = keyT[a[2]][a[1]];
        }
    }
}

// this function will encrypt cipher text using Playfair Cipher algorithm
void encryptByPlayfairCipher(char str[], char key[])
{
    char ps, ks, keyT[5][5];

    
    ks = strlen(key);
    ks = removeSpaces(key, ks);
    toLowerCase(key, ks);

    
    ps = strlen(str);
    toLowerCase(str, ps);
    ps = removeSpaces(str, ps);

    ps = prepare(str, ps);

    generateKeyTable(key, ks, keyT);

    encrypt(str, keyT, ps);
}

// main code
int main()
{
    char str[SIZE], key[SIZE];

    // key text
    strcpy(key, "Algorithm");
    printf("Key text: %s\n", key);

    // Plaintext
    strcpy(str, "Programming");
    printf("Plain text: %s\n", str);

    // encryption using the "Playfair Cipher" algorithmn
    encryptByPlayfairCipher(str, key);

    printf("Cipher text: %s\n", str);

    return 0;
}
```
## OUTPUT:
```
Key text: Algorithm
Plain text: Programming
Cipher text: ulroaliocvrx
```

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
