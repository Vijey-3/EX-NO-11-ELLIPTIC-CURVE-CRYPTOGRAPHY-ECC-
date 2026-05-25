# EX-NO-11-ELLIPTIC-CURVE-CRYPTOGRAPHY-ECC

## Aim:
To Implement ELLIPTIC CURVE CRYPTOGRAPHY(ECC)


## ALGORITHM:

1. Elliptic Curve Cryptography (ECC) is a public-key cryptography technique based on the algebraic structure of elliptic curves over finite fields.

2. Initialization:
   - Select an elliptic curve equation \( y^2 = x^3 + ax + b \) with parameters \( a \) and \( b \), along with a large prime \( p \) (defining the finite field).
   - Choose a base point \( G \) on the curve, which will be used for generating public keys.

3. Key Generation:
   - Each party selects a private key \( d \) (a random integer).
   - Calculate the public key as \( Q = d \times G \) (using elliptic curve point multiplication).

4. Encryption and Decryption:
   - Encryption: The sender uses the recipient’s public key and the base point \( G \) to encode the message.
   - Decryption: The recipient uses their private key to decode the message and retrieve the original plaintext.

5. Security: ECC’s security relies on the Elliptic Curve Discrete Logarithm Problem (ECDLP), making it highly secure with shorter key lengths compared to traditional methods like RSA.

## Program:
```
#include <stdio.h>

struct Point
{
    int x;
    int y;
};

int inverse(int a, int p)
{
    a = (a % p + p) % p;

    for(int i = 1; i < p; i++)
    {
        if((a * i) % p == 1)
        {
            return i;
        }
    }

    return -1;
}

struct Point add(struct Point P, struct Point Q, int a, int p)
{
    struct Point R;
    int l;

    if(P.x == Q.x && P.y == Q.y)
    {
        l = ((3 * P.x * P.x + a) * inverse(2 * P.y, p)) % p;
    }
    else
    {
        l = ((Q.y - P.y) * inverse(Q.x - P.x, p)) % p;
    }

    l = (l % p + p) % p;

    R.x = (l * l - P.x - Q.x) % p;
    R.y = (l * (P.x - R.x) - P.y) % p;

    R.x = (R.x % p + p) % p;
    R.y = (R.y % p + p) % p;

    return R;
}

struct Point multiply(struct Point P, int k, int a, int p)
{
    struct Point R = P;

    for(int i = 0; i < k - 1; i++)
    {
        R = add(R, P, a, p);
    }

    return R;
}

int main()
{
    int p, a;
    int gx, gy;
    int Vijey, Friend;

    struct Point G, pubVijey, pubFriend;
    struct Point secret1, secret2;

    printf("ECC Key Exchange\n");

    printf("Enter prime number: ");
    scanf("%d", &p);

    printf("Enter value of a: ");
    scanf("%d", &a);

    printf("Enter Gx: ");
    scanf("%d", &gx);

    printf("Enter Gy: ");
    scanf("%d", &gy);

    printf("Enter Vijey private key: ");
    scanf("%d", &Vijey);

    printf("Enter Friend private key: ");
    scanf("%d", &Friend);

    G.x = gx;
    G.y = gy;

    pubVijey = multiply(G, Vijey, a, p);
    pubFriend = multiply(G, Friend, a, p);

    secret1 = multiply(pubFriend, Vijey, a, p);
    secret2 = multiply(pubVijey, Friend, a, p);

    printf("\nShared Secret by Vijey: (%d, %d)\n", secret1.x, secret1.y);
    printf("Shared Secret by Friend: (%d, %d)\n", secret2.x, secret2.y);

    return 0;
}
```


## Output:
<img width="1919" height="910" alt="image" src="https://github.com/user-attachments/assets/045eeab3-16b8-4de0-9ae6-eadccf483b5b" />


## Result:
The program is executed successfully

