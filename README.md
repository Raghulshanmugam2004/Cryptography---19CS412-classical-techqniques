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

// Function to perform Caesar cipher encryption
void encrypt(char *text, int shift) {
    int i;
    for (i = 0; i < strlen(text); i++) {
        // Encrypt uppercase letters
        if (text[i] >= 'A' && text[i] <= 'Z')
            text[i] = (text[i] - 'A' + shift) % 26 + 'A';
        // Encrypt lowercase letters
        else if (text[i] >= 'a' && text[i] <= 'z')
            text[i] = (text[i] - 'a' + shift) % 26 + 'a';
    }
}

// Function to perform Caesar cipher decryption
void decrypt(char *text, int shift) {
    int i;
    for (i = 0; i < strlen(text); i++) {
        // Decrypt uppercase letters
        if (text[i] >= 'A' && text[i] <= 'Z')
            text[i] = (text[i] - 'A' - shift + 26) % 26 + 'A';
        // Decrypt lowercase letters
        else if (text[i] >= 'a' && text[i] <= 'z')
            text[i] = (text[i] - 'a' - shift + 26) % 26 + 'a';
    }
}

int main() {
    char text[100];
    int shift;
    
    printf("Enter text to encrypt: ");
    fgets(text, sizeof(text), stdin);
    
    printf("Enter Key value: ");
    scanf("%d", &shift);

    // Encrypt the text
    encrypt(text, shift);
    printf("Encrypted text: %s\n", text);

    // Decrypt the text
    decrypt(text, shift);
    printf("Decrypted text: %s\n", text);

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
![image](https://github.com/Raghulshanmugam2004/Cryptography---19CS412-classical-techqniques/assets/119561118/0fe2651e-6b45-42fd-8cc6-c608c4a07d4e)


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
#include <string.h>
#include <ctype.h>

#define MAX_LENGTH 100

// Function to encrypt or decrypt text using the Vigenère cipher
void vigenereCipher(char *input, char *key, char *output, int encrypt) {
    int inputLength = strlen(input);
    int keyLength = strlen(key);

    for (int i = 0, j = 0; i < inputLength; ++i) {
        char currentChar = input[i];
        
        if (isalpha(currentChar)) {
            // Determine the shift value based on the key
            int keyIndex = j % keyLength;
            int shift = toupper(key[keyIndex]) - 'A';

            if (!encrypt) {
                shift = -shift;
            }

            // Encrypt or decrypt the current character
            if (isupper(currentChar)) {
                output[i] = ((currentChar - 'A' + shift + 26) % 26) + 'A';
            } else {
                output[i] = ((currentChar - 'a' + shift + 26) % 26) + 'a';
            }

            ++j;
        } else {
            // Non-alphabetic characters remain unchanged
            output[i] = currentChar;
        }
    }

    output[inputLength] = '\0';
}

int main() {
    char input[MAX_LENGTH];
    char key[MAX_LENGTH];
    char encrypted[MAX_LENGTH];
    char decrypted[MAX_LENGTH];

    printf("Enter the text to encrypt: ");
    fgets(input, MAX_LENGTH, stdin);
    input[strcspn(input, "\n")] = '\0'; // Remove trailing newline if present

    printf("Enter the key: ");
    fgets(key, MAX_LENGTH, stdin);
    key[strcspn(key, "\n")] = '\0'; // Remove trailing newline if present

    // Encrypt the input text
    vigenereCipher(input, key, encrypted, 1);
    printf("Encrypted text: %s\n", encrypted);

    // Decrypt the encrypted text
    vigenereCipher(encrypted, key, decrypted, 0);
    printf("Decrypted text: %s\n", decrypted);

    return 0;
}

```


## OUTPUT:
![image](https://github.com/Raghulshanmugam2004/Cryptography---19CS412-classical-techqniques/assets/119561118/d6fbec8b-f18c-4d95-878d-7637426ddd25)


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
#include <stdio.h>
#include <string.h>
#include <ctype.h>

#define MAX_LENGTH 100

// Function to encrypt text using the Rail Fence cipher
void railFenceEncrypt(char *input, int rails, char *output) {
    int inputLength = strlen(input);
    int cycle = 2 * (rails - 1);

    for (int i = 0, k = 0; i < rails; ++i) {
        for (int j = i; j < inputLength; j += cycle) {
            output[k++] = input[j];
            if (i != 0 && i != rails - 1 && j + cycle - 2 * i < inputLength) {
                output[k++] = input[j + cycle - 2 * i];
            }
        }
    }
    output[inputLength] = '\0';
}

int main() {
    char input[MAX_LENGTH];
    char encrypted[MAX_LENGTH];
    int rails;

    printf("Enter the text to encrypt: ");
    fgets(input, MAX_LENGTH, stdin);
    input[strcspn(input, "\n")] = '\0'; // Remove trailing newline if present

    printf("Enter the number of rails: ");
    scanf("%d", &rails);
    getchar(); // Consume the newline character left in the input buffer

    // Encrypt the input text
    railFenceEncrypt(input, rails, encrypted);
    printf("Encrypted text: %s\n", encrypted);

    return 0;
}

```

## OUTPUT:
![image](https://github.com/Raghulshanmugam2004/Cryptography---19CS412-classical-techqniques/assets/119561118/8ddd32bd-eaae-4e4b-8e50-b3391f05019f)


## RESULT:
The program is executed successfully.
